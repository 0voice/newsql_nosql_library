# RocksDB的一些基本概念及简单应用

## 1. 简介
RocksDB是使用C++编写的嵌入式kv存储引擎，其键值均允许使用二进制流。由Facebook基于levelDB开发， 提供向后兼容的levelDB API。

## 2. 写入机制

### 2.1 原理
首先，任何的写入都会先写到 WAL，然后在写入 Memory Table(Memtable)。当然为了性能，也可以不写入 WAL，但这样就可能面临崩溃丢失数据的风险。Memory Table 通常是一个能支持并发写入的 skiplist，但 RocksDB 同样也支持多种不同的 skiplist，用户可以根据实际的业务场景进行选择。

当一个 Memtable 写满了之后，就会变成 immutable 的 Memtable，RocksDB 在后台会通过一个 flush 线程将这个 Memtable flush 到磁盘，生成一个 Sorted String Table(SST) 文件，放在 Level 0 层。当 Level 0 层的 SST 文件个数超过阈值之后，就会通过 Compaction 策略将其放到 Level 1 层，以此类推。如图。

![image](https://user-images.githubusercontent.com/87458342/132664994-8281764e-2ce1-4808-9fd8-6e793c2dc77a.png)

### 2.2 相关术语
#### 2.2.1 LSM（log-structed-merge-tree）

上述原理是使用LSM tree这种结构来实现的，上图也是LSM tree的一种直观体现，那为什么要选用LSM tree呢？

B+树（MySQL InnoDB索引）和log型（append）文件操作（数据库WAL日志）是数据读写效率的两个极端。
磁盘读写分为随机读写和顺序读写，一般来说随机读写的效率要低于顺序读写。

B+树解决的是磁盘随机读慢的问题，但随机写无法保证效率，因为写入数据时涉及到树的重构。
log型（append）文件操作解决的是磁盘随机写慢的问题，但读时需要遍历整个文件。

B+树读效率高而写效率差；log型文件操作写效率高而读效率差；因此要在排序和log型文件操作之间做个折中，于是就引入了log-structed merge tree模型，通过名称可以看出LSM既有日志型的文件操作，提升写效率，又在每个sstable中排序，保证了查询效率。

#### 2.2.2 WAL（Write Ahead Log）
顾名思义，就是在实际操作数据前先写日志，便于恢复。WAL在很多数据库中都存在。

RocksDB中的每个更新操作都会写到两个地方：
* 一个内存数据结构，名为memtable(后面会被刷盘到SST文件)
* 写到磁盘上的WAL日志。在出现崩溃的时候，WAL日志可以用于完整的恢复memtable中的数据，以保证数据库能恢复到原来的状态。在默认配置的情况下，RocksDB通过在每次写操作后对WAL调用flush来保证一致性。

#### 2.2.3 Memtable
MemTable是一个内存数据结构，他保存了落盘到SST文件前的数据。他同时服务于读和写——新的写入总是将数据插入到memtable，读取在查询SST文件前总是要查询memtable，因为memtable里面的数据总是更新的。一旦一个memtable被写满，他会变成不可修改的，并被一个新的memtable替换。一个后台线程会把这个memtable的内容落盘到一个SST文件，然后这个memtable就可以被销毁了。

默认的memtable实现是基于skiplist的。除了默认的memtable实现，用户可以使用其他memtable实现，例如HashLinkList，HashSkipList或者Vector，以加快查询速度。

![image](https://user-images.githubusercontent.com/87458342/132665555-982042b8-c06b-4093-b52a-b7911e43c2d9.png)

影响memtable的最重要的几个选项是：

* memtable_factory: memtable对象的工厂。通过声明一个工厂对象，用户可以改变底层memtable的实现，并提供事先声明的选项。
* write_buffer_size：一个memtable的大小。
* db_write_buffer_size：多个列族的memtable的大小总和。这可以用来管理memtable使用的总内存数。
* write_buffer_manager：除了声明memtable的总大小，用户还可以提供他们自己的写缓冲区管理器，用来控制总体的memtable使用量。这个选项会覆盖db_write_buffer_size。
* max_write_buffer_number：内存中可以拥有刷盘到SST文件前的最大memtable数。

这些都可以在RocksDB的Option对象中配置。

#### 2.2.4 Flush
有三种场景会导致memtable落盘被触发：

* Memtable的大小在一次写入后超过write_buffer_size。
* 所有列族中的memtable大小超过db_write_buffer_size了，或者write_buffer_manager要求落盘。在这种场景，最大的memtable会被落盘。
* WAL文件的总大小超过max_total_wal_size。在这个场景，有着最老数据的memtable会被落盘，这样才允许携带有跟这个memtable相关数据的WAL文件被删除。
* 就结果来说，memtable可能还没写满就落盘了。这是为什么生成的SST文件小于对应的memtable大小。压缩是另一个导致SST文件变小的原因，因为memtable里的数据是没有压缩的。

#### 2.2.5 Compaction
LSM-Tree 能将离散的随机写请求都转换成批量的顺序写请求（WAL + Compaction），以此提高写性能。但也带来了一些问题：

* 读放大（Read Amplification）。LSM-Tree 的读操作需要从新到旧（从上到下）一层一层查找，直到找到想要的数据。这个过程可能需要不止一次 I/O。特别是 range query 的情况，影响很明显。
* 空间放大（Space Amplification）。因为所有的写入都是顺序写（append-only）的，不是 in-place update ，所以过期数据不会马上被清理掉。
* 写放大。实际写入 HDD/SSD 的数据大小和程序要求写入数据大小之比。正常情况下，HDD/SSD 观察到的写入数据多于上层程序写入的数据。

RocksDB 和 LevelDB 通过后台的 compaction 来减少读放大（减少 SST 文件数量）和空间放大（清理过期数据），但也因此带来了写放大（Write Amplification）的问题。

写放大、读放大、空间放大，三者就像 CAP 定理一样，需要做好权衡和取舍。

压缩算法有很多种，RocksDB也支持很多种，这里我们看两个经典的压缩算法及区别

Tiered Compaction vs Leveled Compaction

![image](https://user-images.githubusercontent.com/87458342/132666300-89414a96-987b-4d59-94cb-773107499560.png)
