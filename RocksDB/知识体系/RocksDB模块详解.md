# 高性能存储引擎RocksDB总体概览

## RocksDB的整体模块
RocksDB作为一个可嵌入式的持久化存储系统，它是一个单点高性能的存储DB，不是我们平常我们说的分布式存储系统。RocksDB能支持非常高吞吐量的IO读写，可以很好地作为大型分布式存储系统元数据的存储媒介，比如Hadoop Ozone就将其元数据使用RocksDB作为元数据的结果写出。

RocksDB有和其它分布式存储系统类似的术语操作定义，比如WAL(Write-Ahead-Log)，Transaction，Compact，Snapshot等等。不同点在于分布式存储系统需要依赖服务间的RPC通信做WAL的操作，而RocksDB没有RPC的概念。所以RocksDB本质上来说它是一个可插拔式的存储引擎选择。

以下是笔者整理出的RocksDB的整体架构预览图：
![image](https://user-images.githubusercontent.com/87458342/132619094-ce0ff68c-8eea-4775-9fd3-1dd2f2e772de.png)

RocksDB5大子模块，分别为：
* Basic Operation，基本操作定义
* Terminology，内部术语定义
* Tool，内部工具
* Logging/Monitoring ,日志和监控
* System Behavior，内部系统行为

篇幅有限，下面只对上述部分模块进行解读。

### Terminology
首先是RocksDB内部的相关术语定义说明，如上图所示，主要有以下一些术语：

* Write-Ahead-Log File，类似于HDFS JournalNode中的editlog，用于记录那些未被成功提交的数据操作，然后在重启时进行数据的恢复。
* SST File，SST文件是一段排序好的表文件，它是实际持久化的数据文件。里面的数据按照key进行排序能方便对其进行二分查找。在SST文件内，还额外包含以下特殊信息：
  * Bloom Fileter，用于快速判断目标查询key是否存在于当前SST文件内。
  * Index / Partition Index，SST内部数据块索引文件快速找到数据块的位置。
* Memtable，内存数据结构，用以存储最近更新的db更新操作，memtable空间写满后，会触发一次写出更新操作到SST文件的操作。
* Block Cache，纯内存存储结构，存储SST文件被经常访问的热点数据。

### Basic Operation
其次是Basic Operation模块，虽说RocksDB在官网中介绍说是作为主要提供K-V形式的键值存储，但是其内部提供的操作接口绝不仅仅是get,put两类操作，比如还有如下几类能适用于特殊使用场景的操作：

* Iteration，RocksDB能够支持区间范围内的key迭代器的遍历查找。
* Compaction Filter，用户可使用Compaction Filter对key值进行删除或其它更新操作的逻辑定义，当系统进行Compact行为的时候。
* Creating and Ingesting SST files，当用户想要快速导入大批量数据到系统内时，可以通过线下创建有效格式的SST文件并导入的方式，而非使用API进行KV值的单独PUT操作。
* Delete Range，区间范围的删除操作，比一个个Key的单独删除调用使用更方便。
* Low Priority Write，当用户执行大批量数据load的操作时但担心可能会影响到系统正常的操作处理时，可以开启此属性进行优先级的调整。
* Read-Modify-Write，这个操作的实际含义是Merge操作的含义，读取现有键值，进行更新(累加计数或依赖原有值的任何更新操作)，将新的值写入到原Key下。 如果使用原始Get/Set API的前提下，我们要调用2次Get 1次，然后再Set 1次，在Merge API下，使用者调用1次就足够了。
* Transaction，RocksDB内部提供乐观式的OptimisticTransactionDB和悲观式(事务锁方式)的TransactionDB来支持并发的键值更新操作。

### System Behavior
在RocksDB内部，有着许多系统操作行为来保障系统的平稳运行。

* Compression，SST文件内的数据能够被压缩存储来减小占用空间。
* Rate Limit行为。用户能够对其写操作进行速度控制，以此避免写入速度过快造成系统读延迟的现象。
* Delete Schedule，系统文件删除行为的速度控制。
* Direct IO，RocksDB支持绕过系统Page Cache，通过应用内存从存储设置中直接进行IO读写操作。
* Compaction，数据的Compact行为，删除SST文件中重复的key以及过期的key数据。

### Logging/Monitoring
RocksDB内部有以下的日志监控工具：

* Logger，可用的Logger使用类。
* Statistic / Perf Context and IO Stats Context，RocksDB内部各类型操作的时间，操作数计数统计信息，此数据信息能被用户用来发现系统的性能瓶颈操作。
* EventListener，此监听接口提供了一些event事件发生后的接口回调，比如完成一次flush操作，开始Compact操作的时候等等。

### Tool
此外，RocksDB还提供以下3大类型的工具

* 第一类，性能测试工具
  * Benchmark Tool
  * Stress Tool，压力测试工具

* 第二类，workload模拟工具
  * 用户数据访问行为模拟工具
  * Workload生成工具

* 第三类，性能分析工具
  * DB Analyzer
