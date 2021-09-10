# Cassandra基础

## 1. 概念
Cassandra 是高度可扩展的、高性能的分布式 NoSQL 数据库。 Cassandra 旨在处理许多商品服务器上的大量数据，提供高可用性而无需担心单点故障。

Cassandra 具有能够处理大量数据的分布式架构。 数据放置在具有多个复制因子的不同机器上， 以获得高可用性，而无需担心单点故障。

## 2. 数据模型
**Key Space（对应 SQL 数据库中的 database）**

一个Key Space中可包含若干个CF，如同SQL数据库中一个database可包含多个table。

**Key（对应 SQL 数据库中的主键）**

在Cassandra中，每一行数据记录是以key/value的形式存储的，其中key是唯一标识。

**column（对应 SQL 数据库中的列）**

Cassandra中每个key/value对中的value又称为column，它是一个三元组，即：name， value和timestamp，其中 name需要是唯一的。

**super column（SQL 数据库不支持）**

cassandra允许key/value中的value是一个map(key/value_list)，即某个column有多个子列。

**Standard Column Family（相对应 SQL 数据库中的 table）**

每个CF由一系列row组成，每个row包含一个key以及其对应的若干column。

**Super Column Family（SQL 数据库不支持）**

每个SCF由一系列row组成，每个row包含一个key以及其对应的若干super column。

## 3. Cassandra 一致 Hash 和虚拟节点

一致性 Hash（多米诺 down 机） 为每个节点分配一个 token，根据这个 token 值来决定节点在集群中的位置以及这个节点所存储 的数据范围。

虚拟节点（down 机多节点托管） 由于这种方式会造成数据分布不均的问题，在 Cassandra1.2 以后采用了虚拟节点的思想：不需要为每个节点分配token，把圆环分成更多部分，让每个节点负责多个部分的数据，这样一个节点移除后，它所负责的多个token会托管给多个节点处理，这种思想解决了数据分布不均的问题。

![image](https://user-images.githubusercontent.com/87458342/132792121-f1627e96-3c04-47d0-af34-b34827d59dbc.png)

如图所示，上面部分是标准一致性哈希，每个节点负责圆环中连续的一段，如果 Node2 突然 down 掉，Node2 负责的数据托管给 Node1，即 Node1 负责 EFAB 四段，如果 Node1 里面有 很多热点用户产生的数据导致Node1 已经有点撑不住了，恰巧 B 也是热点用户产生的数据，这样 一来Node1可能会接着down机，Node1down机，Node6还hold住吗？

下面部分是虚拟节点实现，每个节点不再负责连续部分，且圆环被分为更多的部分。如果 Node2 突然down掉，Node2负责的数据不全是托管给Node1，而是托管给多个节点。而且也保持了一 致性哈希的特点。

## 4. Gossip 协议
Gossip 算法，有众多的别名，可叫作：“闲话算法”、 “疫情传播算法”、“病毒感染算法”、“谣言传播算法”。

Gossip 的特点：在一个有界网络中， 每个节点都随机地与其他节点通信，经过一番杂乱无章的通信，最终所有节点的状态都会达成一 致。因为 Gossip 不要求节点知道所有其他节点，因此又具有去中心化的特点，节点之间完全对等， 不需要任何的中心节点。实际上 Gossip 可以用于众多能接受“最终一致性”的领域：失败检测、 路由同步、Pub/Sub、动态负载均衡。

Gossip 节点的通信方式及收敛性

Gossip

两个节点（ A 、 B ）之间存在三种通信方式 （ push 、 pull 、 push&pull ）

1. push: A节点将数据(key,value,version)及对应的版本号推送给B节点，B节点更新A中比自 己新的数据。
2. pull：A 仅将数据 key,version 推送给 B，B 将本地比 A 新的数据（Key,value,version）推送 给A，A更新本地。
3. push/pull：与pull类似，只是多了一步，A再将本地比B新的数据推送给B，B更新本地。

如果把两个节点数据同步一次定义为一个周期，则在一个周期内，push 需通信 1 次，pull 需 2 次， push/pull 则需 3 次，从效果上来讲，push/pull 最好，理论上一个周期内可以使两个节点完全一 致。直观上也感觉，push/pull的收敛速度是最快的。

gossip的协议 和 seed list （防止集群分列）

cassandra使用称为gossip的协议来发现加入C集群中的其他节点的位置和状态信息。gossip进 程每秒都在进行，并与至多三个节点交换状态信息。节点交换他们自己和所知道的信息，于是所 有的节点很快就能学习到整个集群中的其他节点的信息。gossip 信息有一个相关的版本号，于是 在一次gossip信息交换中，旧的信息会被新的信息覆盖重写。要阻止分区进行gossip交流，那么 在集群中的所有节点中使用相同的 seed list，种子节点的指定除了启动起 gossip 进程外，没有其它的目的。种子节点不是一个单点故障，他们在集群操作中也没有其他的特殊目的，除了引导节点以外。

## 5. 数据复制
**Partitioners（计算 primary key token 的 hash 函数）**

在Cassandra中，table的每行由唯一的 primarykey标识，partitioner实际上为一 hash函数用 以计算primary key的token。Cassandra依据这个token值在集群中放置对应的行。
两种可用的复制策略：

SimpleStrategy ： 仅用于单数据：

将第一个 replica 放在由 partitioner 确定的节点中，其余的 replicas 放在上述节点顺时针方向的后续节点中。

NetworkTopologyStrategy ：可用于较复杂的多数据中心。

可以指定在每个数据中心分别存储多少份replicas。 复制策略在创建keyspace时指定，如：
```C
CREATE KEYSPACE Excelsior WITH REPLICATION = { 'class' : 'SimpleStrategy','replication_factor' : 3 };   
CREATE KEYSPACE Excalibur WITH REPLICATION = {'class' :'NetworkTopologyStrategy', 'dc1' : 3, 'dc2' : 2};
```

## 6. 数据写请求和协调者
协调者(coordinator) 协调者(coordinator)将write请求发送到拥有对应row的所有replica节点，只要节点可用便获取 并执行写请求。写一致性级别(write consistency level)确定要有多少个replica节点必须返回成功 的确认信息。成功意味着数据被正确写入了commit log和memtable。

![image](https://user-images.githubusercontent.com/87458342/132792449-d7b1b8b5-a56d-486c-a5dc-848c81156ac8.png)

其中 dc1、dc2 这些数据中心名称要与 snitch 中配置的名称一致.上面的拓扑策略表示在 dc1 配置 3个副本,在dc2配置2个副本 。

## 7. 数据读请求和后台修复

1. 协调者首先与一致性级别确定的所有replica联系，被联系的节点返回请求的数据。
2. 若多个节点被联系，则来自各replica的row会在内存中作比较，若不一致，则协调者使用含 最新数据的replica向client返回结果。那么比较操作过程中只需要传递时间戳就可以,因为要 比较的只是哪个副本数据是最新的。
3. 协调者在后台联系和比较来自其余拥有对应 row 的 replica 的数据，若不一致，会向过时的 replica发写请求用最新的数据进行更新read repair。

![image](https://user-images.githubusercontent.com/87458342/132792610-9cd9c40d-19eb-49ca-89fa-d20df128f47b.png)

## 8. 数据存储（CommitLog、MemTable、SSTable）
写请求分别到 CommitLog 和 MemTable, 并且 MemTable 的数据会刷写到磁盘 SSTable 上. 除 了写数据,还有索引也会保存到磁盘上。

先将数据写到磁盘中的commitlog，同时追加到中内存中的数据结构memtable 。这个时候就会 返回客户端状态，memtable 内容超出指定容量后会被放进将被刷入磁盘的队列 (memtable_flush_queue_size 配置队列长度)。若将被刷入磁盘的数据超出了队列长度，将内存 数据刷进磁盘中的SSTable,之后commit log被清空。

SSTable 文件构成（BloomFilter、index、data、static）

SSTable 文件有fileer（判断数据key是否存在，这里使用了BloomFilter提高效率），index（寻 找对应 column 值所在 data 文件位置）文件，data（存储真实数据）文件，static（存储和统计 column和row大小）文件。

## 9. 二级索引
二级索引，对要索引的 value 摘要，生成 RowKey。

在Cassandra中，数据都是以Key-value的形式保存的。

![image](https://user-images.githubusercontent.com/87458342/132792731-9cb095b5-41d8-418d-a9bd-24c2a8f3c331.png)

KeysIndex 所创建的二级索引也被保存在一张 ColumnFamily 中。在插入数据时，对需要进行索 引的value进行摘要，生成独一无二的key，将其作为RowKey保存在索引的ColumnFamily中； 同时在 RowKey 上添加一个 Column，将插入数据的 RowKey 作为 name 域的值，value 域则赋 空值，timestamp域则赋为插入数据的时间戳。

如果有相同的 value 被索引了，则会在索引 ColumnFamily 中相同的 RowKey 后再添加新的 Column。如果有新的 value 被索引，则会在索引 ColumnFamily 中添加新的 RowKey 以及对应 新的Column。

当对 value 进行查询时，只需计算该 value 的 RowKey，在索引 ColumnFamily 中的查找该 RowKey，对其Columns进行遍历就能得到该value所有数据的RowKey。

## 10. 数据读写
**数据写入和更新（数据追加）**

Cassandra 的设计思路与这些系统不同，无论是 insert 还是 remove 操作，都是在已有的数据后 面进行追加，而不修改已有的数据。这种设计称为 Log structured 存储，顾名思义就是系统中的 数据是以日志的形式存在的，所以只会将新的数据追加到已有数据的后面。Log structured 存储系统有两个主要优点：

**数据的写和删除效率极高**

传统的存储系统需要更新元信息和数据，因此磁盘的磁头需要反复移动，这是一个比较耗时的操作，而 Log structured 的系统则是顺序写，可以充分利用文件系统的 cache，所以效率很高。

**错误恢复简单**

由于数据本身就是以日志形式保存，老的数据不会被覆盖，所以在设计 journal 的时候不需 要考虑 undo，简化了错误恢复。

**读的复杂度高**

但是，Log structured 的存储系统也引入了一个重要的问题：读的复杂度和性能。理论上 说，读操作需要从后往前扫描数据，以找到某个记录的最新版本。相比传统的存储系统，这 是比较耗时的。

**数据删除（column 的墓碑）**

如果一次删除操作在一个节点上失败了（总共 3 个节点，副本为 3， RF=3).整个删除操作仍然被 认为成功的（因为有两个节点应答成功，使用 CL.QUORUM 一致性）。接下来如果读发生在该节 点上就会变的不明确，因为结果返回是空，还是返回数据，没有办法确定哪一种是正确的。

Cassandra 总是认为返回数据是对的，那就会发生删除的数据又出现了的事情，这些数据可以叫” 僵尸”，并且他们的表现是不可预见的。

**墓碑**

删除一个 column 其实只是插入一个关于这个 column 的墓碑（tombstone），并不直接删除原 有的 column。该墓碑被作为对该 CF 的一次修改记录在 Memtable 和 SSTable 中。墓碑的内容 是删除请求被执行的时间，该时间是接受客户端请求的存储节点在执行该请求时的本地时间 （local delete time），称为本地删除时间。需要注意区分本地删除时间和时间戳，每个 CF 修改 记录都有一个时间戳，这个时间戳可以理解为该 column 的修改时间，是由客户端给定的。

**垃圾回收 compaction**

由于被删除的 column 并不会立即被从磁盘中删除，所以系统占用的磁盘空间会越来越大，这就 需要有一种垃圾回收的机制，定期删除被标记了墓碑的 column。垃圾回收是在 compaction 的过程中完成的。

**数据读取 （ memtable + SStables ）**

为了满足读 cassandra 读取的数据是 memtable 中的数据和 SStables 中数据的合并结果。读取 SSTables 中的数据就是查找到具体的哪些的 SSTables 以及数据在这些 SSTables 中的偏移量 (SSTables是按主键排序后的数据块)。首先如果row cache enable了话，会检测缓存。缓存命中 直接返回数据，没有则查找 Bloom filter，查找可能的SSTable。然后有一层Partition key cache， 找partition key的位置。如果有根据找到的partition去压缩偏移量映射表找具体的数据块。如果 缓存没有，则要经过Partition summary,Partition index去找partition key。然后经过压缩偏移 量映射表找具体的数据块。

1. 检查 memtable。
2. 如果enabled了,检查row cache。
3. 检查Bloom filter。
4. 如果enable了,检查partition key 缓存。
5. 如果在partition key缓存中找到了partition key,直接去compression offset 命中，如果没有，检查 partition summary。
6. 根据compression offset map找到数据位置。
7. 从磁盘的SSTable中取出数据。

![image](https://user-images.githubusercontent.com/87458342/132792853-1a219c5a-64c7-4f2c-9a8e-95b440b3a3e4.png)

行缓存和键缓存请求流程：

![image](https://user-images.githubusercontent.com/87458342/132792889-06e0166e-4c87-4bf1-b6f1-f3bd2ff2e477.png)


**MemTable**

如果 memtable 有目标分区数据，这个数据会被读出来并且和从 SSTables 中读出 来的数据进行合并。SSTable的数据访问如下面所示的步骤。

**Row Cache （ SSTables中频繁被访问的数据 ）**

在 Cassandra2.2+，它们被存储在堆外内存，使用全新的实现避免造成垃圾回收对 JVM 造成压力。 存在在 row cache 的子集数据可以在特定的一段时间内配置一定大小的内存。row cache 使用 LRU(least-recently-userd)进行回收在申请内存。存储在row cache中的数据是SSTables中频繁 被访问的数据。存储到row cache中后，数据就可以被后续的查询访问。row cache不是写更新。 如果写某行了，这行的缓存就会失效，并且不会被继续缓存，直到这行被读到。类似的，如果一 个partition更新了，整个partition的cache都会被移除，但目标的数据在row cache中找不到， 就会去检查Bloom filter。

**Bloom Filter （ 查找数据可能对应的 SSTable ）**

首先，Cassandra 检查 Bloom filter 去发现哪个 SSTables 中有可能有请求的分区数据。Bloom filter是存储在堆外内存。每个SSTable都有一个关联的Bloom filter。一个Bloom filter可以建 立一个 SSTable 没有包含的特定的分区数据。同样也可以找到分区数据存在 SSTable 中的可能性。 它可以加速查找partition key 的查找过程。然而，因为Bloom filter是一个概率函数，所以可能 会得到错误的结果，并不是所有的 SSTables 都可以被 Bloom filter 识别出是否有数据。如果 Bloom filter不能够查找到SSTable，Cassandra会检查partition key cache。Bloom filter 大小 增长很适宜，每 10 亿数据 1~2GB。在极端情况下，可以一个分区一行。都可以很轻松的将数十 亿的entries存储在单个机器上。Bloom filter是可以调节的，如果你愿意用内存来换取性能。

**Partition Key Cache （ 查找数据可能对应的 Partition key ）**

partition key 缓存如果开启了，将partition index存储在堆外内存。key cache使用一小块可配置大小的内存。在读的过程中，每个”hit”保存一个检索。如果在key cache 中找到了 partition key。就直接到compression offset map中招对应的块。partition key cache热启动后工作的更 好，相比较冷启动，有很大的性能提升。如果一个节点上的内存非常受限制，可能的话，需要限制保存在key cache中的partition key数目。如果一个在key cache中没有找到partition key。 就会去partition summary中去找。partition key cache 大小是可以配置的，意义就是存储在key cache中的partition keys数目。

**Partition Summary （ 内存中 存储一些 partition index的样本 ）**

partition summary 是存储在堆外内存的结构，存储一些 partition index 的样本。如果一个 partition index 包含所有的 partition keys。鉴于一个 partition summary 从每 X 个 keys 中取样，然后将每X个key map到index 文件中。例如，如果一个partition summary设置了20keys 进行取样。它就会存储 SSTable file 开始的一个 key,20th 个 key，以此类推。尽管并不知道 partition key 的具体位置，partition summary 可以缩短找到 partition 数据位置。当找到了 partition key 值可能的范围后，就会去找 partition index。通过配置取样频率，你可以用内存来换取性能，当 partition summary 包含的数据越多，使用的内存越多。可以通过表定义的 index interval 属性来改变样本频率。固定大小的内存可以通过 index_summary_capacity_in_mb 属性 来设置，默认是堆大小的5%。

**Partition Index （磁盘中）**
partition index 驻扎在磁盘中，索引所有 partition keys 和偏移量的映射。如果 partition summary 已经查到partition keys的范围，现在的检索就是根据这个范围值来检索目标partition key。需要进行单次检索和顺序读。根据找到的信息。然后去compression offset map中去找磁盘中有这个数据的块。如果 partition index 必须要被检索，则需要检索两次磁盘去找到目标数据。

**Compression offset map （磁盘中）**

compression offset map存储磁盘数据准确位置的指针。存储在堆外内存，可以被partition key cache或者partition index访问。一旦compression offset map识别出来磁盘中的数据位置， 就会从正确的SStable(s)中取出数据。查询就会收到结果集。



原文作者： ITWUYI
