# 1. Oceanbase
## 1.1 Oceanbase基本架构
![image.png](https://pic.leetcode-cn.com/1631543717-MsATTh-image.png)

* 多副本：OceanBase一般部署为三个Zone，每个Zone由多个节点/服务器（OBServer）组成
* 全对等节点：每个节点均有自己的SQL引擎和存储引擎，各自管理不同的数据分区，完全对等
* 无共享：OceanBase数据分布在各个节点上，不基于任何共享存储结构

![image.png](https://pic.leetcode-cn.com/1631543725-SbJZLj-image.png)



数据分区：OceanBase数据架构的基本单元，是传统数据库的分区表在分布式系统上的实现。
高可用+强一致：多副本+Paxos协议，保证数据（日志）写（持久化）到三台机器中至少两台 （每个Zone中基线数据分布在ChunkSever上，有两个或三个副本，增量数据在UpdateSever上是一主一备）

![image.png](https://pic.leetcode-cn.com/1631537756-WwLTWU-image.png)
ObProxy：百万级处理能力的代理，路由转发，轻量级SQLParser，无状态
* 反向代理功能
* 性能需求
* 运维需求

## 1.2 Oceanbase特性
* **高性能**：存储采用读写分离架构，计算引擎全链路性能优化，准内存数据库性能。
* **低成本**：使用PC服务器和低端SSD，高存储压缩率降低存储成本，高性能降低计算成本，多租户混部充分利用系统资源。
* **高可用**：数据采用多副本存储，少数副本故障不影响数据可用性。通过“三地五中心”部署实现城市级故障自动无损容灾。
* **强一致**：数据多副本通过paxos协议同步事务日志，多数派成功事务才能提交。缺省情况下读、写操作都在主副本进行，保证强一致。
* **可扩展**：集群节点全对等，每个节点都具备计算和存储能力，无单点瓶颈。可线性、在线扩展和收缩。
* **兼容性**：兼容常用MySQL/ORACLE功能及MySQL/ORACLE前后台协议，业务零修改或少量修改即可从MySQL/ORACLE迁移至OceanBase。

## 1.3 应用场景

* 金融级数据可靠性需求
金融环境下通常对数据可靠性有更高的要求，OceanBase每一次事务提交，对应日志总是会在多个数据中心实时同步，并持久化。即使是数据中心级别的灾难发生，总是可以在其他的数据中心恢复每一笔已经完成的交易，实现了真正金融级别的可靠性要求。
![image.png](https://pic.leetcode-cn.com/1631537961-ONeQBd-image.png)

* 让数据库适应飞速增长的业务
业务的飞速成长，通常会给数据库带来成倍压力。OceanBase作为一款真正意义的分布式关系型数据库，由一个个独立的通用计算机作为系统各个节点，数据根据容量大小、可用性自动分布在各个节点，当数据量不断增长时，OceanBase可以自动扩展节点的数量，满足业务需求。
![image.png](https://pic.leetcode-cn.com/1631537969-NXgBmz-image.png)

* 连续不间断的服务
企业连续不间断的服务，通常意味着给客户最流畅的产品体验。分布式的OceanBase集群，如果某个节点出现异常时，可以自动剔除此服务节点，该节点对应的数据有多个其他副本，对应的数据服务也由其他节点提供。甚至当某个数据中心出现异常，OceanBase可以在短时间内将服务节点切换到其他数据中心，可以保证业务持续可用。
![image.png](https://pic.leetcode-cn.com/1631537981-tCwZcs-image.png)

## 1.4 正在使用的企业
![image.png](https://pic.leetcode-cn.com/1631539184-dtYuvQ-image.png)

## 1.5 薪资情况
![image.png](https://pic.leetcode-cn.com/1631538121-BSjtxg-image.png)

## 1.6 相关文章或视频

[阿里巴巴开源数据库--OceanBase从使用聊到架构剖析](https://github.com/0voice/newsql_nosql_library#nav_sec4_chapter4_02)


# 2. TiDB
## 2.1 TiDB整体架构
![image.png](https://pic.leetcode-cn.com/1631538375-qfHlLl-image.png)

### TiDB Server
TiDB Server 负责接收SQL请求，处理SQL相关的逻辑，并通过PD找到存储计算所需数据的TiKV地址，与TiKV交互获取数据，最终返回结果。TiDB Server 是无状态的，其本身并不存储数据，只负责计算，可以无限水平扩展，可以通过负载均衡组件（LVS、HAProxy或F5）对外提供统一的接入地址。

### PD Server
Placement Driver（简称PD）是整个集群的管理模块，其主要工作有三个：一是存储集群的元信息（某个Key存储在那个TiKV节点）；二是对TiKV集群进行调度和负载均衡（如数据的迁移、Raft group leader的迁移等）；三是分配全局唯一且递增的事务ID。
PD 是一个集群，需要部署奇数个节点，一般线上推荐至少部署3个节点。PD在选举的过程中无法对外提供服务，这个时间大约是3秒。

### TiKV Server
TiKV Server 负责存储数据，从外部看TiKV是一个分布式的提供事务的Key-Value存储引擎。存储数据的基本单位是Region，每个Region负责存储一个Key Range（从StartKey到EndKey的左闭右开区间）的数据，每个TiKV节点会负责多个Region。TiKV使用Raft协议做复制，保持数据的一致性和容灾。副本以Region为单位进行管理，不同节点上的多个Region构成一个Raft Group，互为副本。数据在多个TiKV之间的负载均衡由PD调度，这里也就是以Region为单位进行调度

## 2.2 TiDB特性
* 一键水平扩容或者缩容，得益于TiDB存储计算分离的架构设计，可按需对计算、存储分别进行在线扩容、缩容，调整过程中对应用运维人员透明。

* 金融级高可用数据采用多副本存储，数据副本通过Multi-Raft协议同步事务日志，多数派写入成功事务才能提交，确保数据强一致性且少数副本发生故障时不影响数据的可用性。可按需配置副本地理位置、副本数量等策略满足不同容灾级别的要求。

* 实时HTAP 提供行存储引擎TiKV、列存储引擎TiFlash两款存储引擎，TiFlash通过Multi-Raft Learner协议实时从TiKV复 制数据，确保行存储引擎TiKV和列存储引擎TiFlash之间的数据强一致。TiKV、TiFlash可按需部署在不同的机器，解决HTAP资源隔离的问题。

* 云原生的分布式数据库专为云而设计的分布式数据库，通过TiDBOperator可在公有云、私有云、混合云中实现部署工具化、自动化。

* 兼容MySQL5.7协议和MySQL生态兼容MySQL5.7协议、MySQL常用的功能、MySQL生态，应用无需或者修改少量代码即可从MySQL迁移到 TiDB。提供丰富的数据迁移工具帮助应用便捷完成数据迁移。

## 2.3 应用场景

* **对数据一致性及高可靠、系统高可用、可扩展性、容灾要求较高的金融行业属性的场景**。众所周知，金融行业对数据一致性及高可靠、系统高可用、可扩展性、容灾要求较高。传统的解决方案是同城两个机房提供服务、异地一个机房提供数据容灾能力但不提供服务，此解决方案存在以下缺点：资源利用率低、维护成本高、RTO(RecoveryTimeObjective)及RPO(RecoveryPointObjective)无法真实达到企业所期望的值。TiDB采用多副本+Multi-Raft协议的方式将数据调度到不同的机房、机架、机器，当部分机 器出现故障时系统可自动进行切换，确保系统的RTO<=30s及RPO=0。

* **对存储容量、可扩展性、并发要求较高的海量数据及高并发的OLTP场景**。
随着业务的高速发展，数据呈现爆炸性的增长，传统的单机数据库无法满足因数据爆炸性的增长对数据库的容量要求，可行方案是采用分库分表的中间件产品或者NewSQL数据库替代、采用高端的存储设备等，其中性价比最大的是NewSQL数据库，例如：TiDB。
TiDB采用计算、存储分离的架构，可对计算、 存储分别进行扩容和缩容，计算最大支持512节点，每个节点最大支持1000并发，集群容量最大支持 PB级别。

* **Real-time HTAP场景**。随着5G、物联网、人工智能的高速发展，企业所生产的数据会越来越多，其规模可能达到数百TB甚至PB级别，传统的解决方案是通过OLTP型数据库处理在线联机交易业务，通过ETL工具将数据同步到 OLAP型数据库进行数据分析，这种处理方案存在存储成本高、实时性差等多方面的问题。TiDB在4.0版 本中引入列存储引擎TiFlash结合行存储引擎TiKV构建真正的HTAP数据库，在增加少量存储成本的情况 下，可以同一个系统中做联机交易处理、实时数据分析，极大地节省企业的成本。

* **数据汇聚、二次加工处理的场景**。 当前绝大部分企业的业务数据都分散在不同的系统中，没有一个统一的汇总，随着业务的发展，企业的决策层需要了解整个公司的业务状况以便及时做出决策，故需要将分散在各个系统的数据汇聚在同一个系统并进行二次加工处理生成T+0或T+1的报表。传统常见的解决方案是采用ETL + Hadoop来完成， 但Hadoop体系太复杂，运维、存储成本太高无法满足用户的需求。与Hadoop相比，TiDB就简单得多， 业务通过ETL工具或者TiDB的同步工具将数据同步到TiDB，在TiDB中可通过SQL直接生成报表。
TiDB v4.0在稳定性、易用性、性能、安全和功能方面进行了大量的改进

## 2.4 正在使用的企业

TiDB 凭借领先的技术能力及完善的商业服务支持体系，帮助金融、互联网、零售、物流、制造、公共服务等行业用户构建面向未来的数据服务平台。
目前，TiDB 已在全球超过 1500 家头部企业的生产环境中得到应用，包括 **中国银行、光大银行、浦发银行、浙商银行、北京银行、微众银行、亿联银行、中国银联、中国人寿、平安人寿、陆金所、中国移动、中国联通、中国电信、中体骏彩、国家电网、理想汽车、小鹏汽车、VIVO、OPPO、百胜中国、中国邮政、顺丰速运、中通快递、腾讯、美团、京东、拼多多、小米、新浪微博、58同城、360、知乎、爱奇艺、哔哩哔哩、Square（美国）、Dailymotion（法国）、Shopee（新加坡）、ZaloPay（越南）、BookMyShow（印度）** 等。

## 2.5 薪资情况

![image.png](https://pic.leetcode-cn.com/1631538965-rWtDbY-image.png)

## 2.6 相关文章或视频

[TiDB概述和整体架构](https://github.com/0voice/newsql_nosql_library#nav_sec4_chapter4_01)

# 3. Cassandra

Cassandra是一个开源的、分布式、无中心节点、弹性可扩展、高可用、容错、一致性协调、面向列的NoSQL数据库

## 3.1 Cassandra内部架构
### 3.1.1 Cassandra集群（Cluster）
* Cluster
    * Data center(s)
        * Rack(s)
            * Server(s)
                * Node (more accurately, a vnode)
* Node（节点）：一个运行cassandra的实例
* Rack（机架）：一组nodes的集合
* DataCenter（数据中心）：一组racks的集合
* Cluster（集群）：映射到拥有一个完整令牌圆环所有节点的集合
![image.png](https://pic.leetcode-cn.com/1631539597-GEXPPH-image.png)

### 3.1.2 协调者（Coordinator）
客户端连接到某一节点发起读写请求时，该节点充当客户端应用与集群中拥有相应数据节点间的桥梁，称为协调者，以根据集群配置确定环(ring)中的哪个节点应当获取这个请求。
使用CQL连接指定的-h节点就是协调节点

* 集群中任何一个节点都可能成为协调者
* 每个客户端请求都可能由不同的节点来协调
* 由协调者管理复制因子（复制因子：一条新数据应该被复制到多少个节点）
* 协调者申请一致性级别（一致性级别：集群中有多少节点必须相应读写的请求）

![image.png](https://pic.leetcode-cn.com/1631539657-OXewUO-image.png)

### 3.1.3 分区器（Partitioner）
分区器决定了数据如何在集群内被分发。在Cassandra中，table的每行由唯一的primarykey标识，partitioner实际上为一hash函数用，以计算primary key的token。Cassandra依据这个token值在集群中放置对应的行。

Cassandra提供了三种不同的分区器
* Murmur3Partitioner（默认）- 基于MurmurHash hash值将数据均匀的分布在集群 
* RandomPartitioner - 基于MD5 hash值将数据均匀的分布在集群中
* ByteOrderedPartitioner - 通过键的字节来保持数据词汇的有序分布

### 3.1.4 虚拟节点（Vnode）
每个虚拟节点对应一个token值，每个token决定了节点在环中的位置以及节点应当承担的一段连续的数据hash值的范围，因此每个节点都拥有一段连续的token，这一段连续的token，组成了一个封闭的圆环。

没有使用虚拟节点, Ring环的tokens数量=集群的机器数量. 比如一共有6个节点,所以token数=6.因为副本因子=3,一条记录要在集群中的三个节点存在. 简单地方式是计算rowkey的hash值,落在环中的哪个token上,第一份数据就在那个节点上,剩余两个副本落在这个节点在token环上的后两个节点.

图中的A,B,C,D,E,F是key的范围,真实的值是hash环空间,比如0~2^32区间分成10份.每一段是2^32的1/10.节点1包含A,F,E表示key范围在A,F,E的数据会存储到节点1上.以此类推.

若不使用虚拟节点则需手工为集群中每个节点计算和分配一个token。每个token决定了节点在环中的位置以及节点应当承担的一段连续的数据hash值的范围。如图上半部分，每个节点分配了一个单独的token代表环中的一个位置，每个节点存储将row key映射为hash值之后落在该节点应当承担的唯一的一段连续的hash值范围内的数据。每个节点也包含来自其他节点的row的副本。

![image.png](https://pic.leetcode-cn.com/1631539726-ImHAgu-image.png)

而使用虚拟节点允许每个节点拥有多个较小的不连续的hash值范围。如图中下半部分，集群中的节点使用了虚拟节点，虚拟节点随机选择且不连续。数据的存放位置也由row key映射而得的hash值确定，但是是落在更小的分区范围内。

使用virtual nodes的好处

* 无需为每个节点计算、分配token
* 添加移除节点后无需重新平衡集群负载
* 重建异常的节点更快

### 3.1.5 复制（Replication）
当前有两种可用的复制策略：
* SimpleStrategy：仅用于单数据中心，将第一个replica放在由partitioner确定的节点中，其余的replicas放在上述节点顺时针方向的后续节点中。

![image.png](https://pic.leetcode-cn.com/1631539838-GqbVoh-image.png)

* NetworkTopologyStrategy：可用于较复杂的多数据中心。可以指定在每个数据中心分别存储多少份replicas。

![image.png](https://pic.leetcode-cn.com/1631539868-zcENTf-image.png)

复制策略在创建keyspace时指定，如
```C
CREATE KEYSPACE Excelsior WITH REPLICATION = { 'class' : 'SimpleStrategy','replication_factor' : 3 };  
CREATE KEYSPACE Excalibur WITH REPLICATION = {'class' :'NetworkTopologyStrategy', 'dc1' : 3, 'dc2' : 2};
```
其中dc1、dc2这些数据中心名称要与snitch中配置的名称一致.上面的拓扑策略表示在dc1配置3个副本,在dc2配置2个副本

### 3.1.6 八卦（gossip）
Gossip协议是群集中节点相互通信的内部通信技术。 Gossip是一种高效，轻量，可靠的节点间用于交换数据的广播协议，。 它是分散式的、容错和点对点的通信协议。 Cassandra使用gossipibing来进行对等发现和元数据传播。

gossip协议的具体表现形式就是配置文件中的seeds种子节点. 一个注意点是同一个集群的所有节点的种子节点应该一致.否则如果种子节点不一致, 有时候会出现集群分裂, 即会出现两个集群. 一般先启动种子节点,尽早发现集群中的其他节点.每个节点都和其他节点交换信息, 由于随机和概率,一定会穷举出集群的所有节点.同时每个节点都会保存集群中的所有其他节点.这样随便连到哪一个节点,都能知道集群中的所有其他节点. 比如cql随便连接集群的一个节点,都能获取集群所有节点的状态.也就是说任何一个节点关于集群中的节点信息的状态都应该是一致的!

![image.png](https://pic.leetcode-cn.com/1631539931-fMhfJM-image.png)

gossip进程每秒运行一次，与至多3个其他节点交换信息，这样所有节点可很快了解集群中的其他节点信息。由于整个过程是分散的，并没有一个节点去协调每个节点的gossip信息，每个节点都会独立地选择一个到三个节点进行gossip通信，它始终选择集群中活动着的对等节点，概率性的选择seed节点和不可达的节点。

![image.png](https://pic.leetcode-cn.com/1631539946-ZsZWfa-image.png)

gossip协议和tcp三次握手比较相似，使用常规的广播协议，每轮只能有一条消息，并且允许消息在集群内部逐渐传播，但随着gossip协议，每条gossip消息都只会包含三条消息，增加了一定程度的逆熵。这个过程允许相互进行数据交换的节点之间快速的收敛。

首先系统需要配置几个种子节点，比如说A、B, 每个参与的节点都会维护所有节点的状态，node->(Key,Value,Version),版本号较大的说明其数据较新，节点P只能直接更新它自己的状态，节点P只能间接的通过gossip协议来更新本机维护的其他节点的数据。

大致的过程如下:
* SYN:节点A向随机选择一些节点，这里可以只选择发送摘要，即不发送valus,避免消息过大
* ACK:节点B接收到消息后，会将其与本地的合并，这里合并采用的是对比版本，版本较大的说明数据较新. 比如节点A向节点B发送数据C(key,valuea,2),而节点B本机存储的是C(key,valueb,3),那么因为B的版本比较新，合并之后的数据就是B本机存储的数据，然后会发回A节点。
* ACK2:节点A接收到ACK消息，将其应用到本机的数据中.

### 3.1.7 一致性（Consistency Level）
一致性指一行数据的所有副本集是否最新和同步。Cassandra扩展了最终一致性的概念，对一个读或者写操作，所谓可调节的一致性的概念，指发起请求的客户端，可以通过consistency level参数，指定本次请求，需要的一致性。
写操作的一致性
![image.png](https://pic.leetcode-cn.com/1631540011-PdsCEH-image.png)
注意：

即使指定了consistency level ON或LOCAL_QUORUM，写操作还是会被发送给所有的replica节点，包括其他数据中心的里replica节点。consistency level只是决定了，通知客户端请求成功之前，需要确保写操作成功的replica节点的数量。

读一致性
![image.png](https://pic.leetcode-cn.com/1631540037-DzcGbm-image.png)

## 3.2 Cassandra特点
* 高可扩展性：Cassandra具有高度的可扩展性，可以帮助您可随时添加更多硬件，以便根据需求附加更多客户和更多数据。
* 刚性结构：Cassandra没有一个单一的故障点，它可用于无法承受故障的关键业务应用程序。
* 快速线性规模的性能：Cassandra线性可扩展。它可以提高吞吐量，因为它可以帮助您增加群集中的节点数量。 因此，它保持快速的响应时间。
* 容错：Cassandra是容错的。 假设集群中有4个节点，这里每个节点都有相同数据的副本。 如果一个节点不再服务，则其他三个节点可以按照请求进行服务。
* 灵活的数据存储：Cassandra支持所有可能的数据格式，如结构化，半结构化和非结构化。 它可以帮助您根据需要更改数据结构。 
* 简单的数据分发：Cassandra中的数据分发非常简单，因为它可以灵活地通过在多个数据中心复制数据来分发所需的数据。
* 事务支持：Cassandra支持事务，诸如原子性，一致性，隔离和持久性(ACID)等属性。
* 快速写入：Cassandra的设计是在便宜的商品硬件上运行。 它执行快速写入，可以存储数百TB的数据，而不会牺牲读取效率。

## 3.3 Cassandra应用场景

理想的Cassandra应用程序具有以下特征：
* 写入大幅度超出读。
* 数据很少更新，并且在进行更新时它们是幂等的。
* 通过主键查询，非二级索引。
* 可以通过partitionKey均匀分区。
* 不需要Join或聚合。

推荐使用Cassandra的一些好场景是：
* 交易日志：购买，测试分数，观看的电影等。
* 存储时序数据（需要您自行聚合）。
* 跟踪几乎任何事情，包括订单状态，包裹等。
* 存储健康追踪数据。
* 气象服务历史。
* 物联网状态和事件历史。
* 汽车的物联网数据。
* 电子邮件

## 3.4 Cassandra正在使用的企业

### 3.4.1 思科案例
* 用例
![image.png](https://pic.leetcode-cn.com/1631540440-UKRJUP-image.png)
* 思科商业更新云
![image.png](https://pic.leetcode-cn.com/1631540407-UsYhYr-image.png)
* 商业分析与报告
![image.png](https://pic.leetcode-cn.com/1631540425-eIgxad-image.png)

### 3.4.4 其他使用情况
![image.png](https://pic.leetcode-cn.com/1631540352-HisbSx-image.png)

## 3.5 相关文章或视频

[高性能存储引擎RocksDB模块详解](https://github.com/0voice/newsql_nosql_library#nav_sec3_chapter2_01)
[RocksDB概念及简单应用](https://github.com/0voice/newsql_nosql_library#nav_sec3_chapter2_01)


# 4.RocksDB

RocksDB是用C++编写的嵌入式KV存储引擎，**由Facebook基于levelDB开发**，它支持多种存储硬件，使用日志结构的数据库引擎(基于LSM-Tree)来存储数据。

## 4.1 RocksDB架构
Rocksdb基础架构
![image.png](https://pic.leetcode-cn.com/1631541394-MlYouM-image.png)
RocksDB读写流程
![image.png](https://pic.leetcode-cn.com/1631540836-jpXcDr-image.png)


## 4.2 RocksDB特点

RocksDB相对传统的关系数据库的一大改进是采用LSM树存储引擎。

### 4.2.1 LSM 树（Log-Structured Merge Tree）
LSM-tree
![image.png](https://pic.leetcode-cn.com/1631541193-KIiJBq-image.png)

LSM树而且通过批量存储技术规避磁盘随机写入问题。 LSM树的设计思想非常朴素, 它的原理是把一颗大树拆分成N棵小树， 它首先写入到内存中（内存没有寻道速度的问题，随机写的性能得到大幅提升），在内存中构建一颗有序小树，随着小树越来越大，内存的小树会flush到磁盘上。磁盘中的树定期可以做merge操作，合并成一棵大树，以优化读性能。

### 4.2.2 LevelDB 特点
LevelDB
![image.png](https://pic.leetcode-cn.com/1631541222-wbalgP-image.png)

1. LevelDB是一个持久化存储的KV系统，和Redis这种内存型的KV系统不同，LevelDB不会像Redis一样狂吃内存，而是将大部分数据存储到磁盘上。
2. LevleDb在存储数据时，是根据记录的key值有序存储的，就是说相邻的key值在存储文件中是依次顺序存储的，而应用可以自定义key大小比较函数。
3. LevelDB支持数据快照（snapshot）功能，使得读取操作不受写操作影响，可以在读操作过程中始终看到一致的数据。
4. LevelDB还支持数据压缩等操作，这对于减小存储空间以及增快IO效率都有直接的帮助。

### 4.2.3 RocksDB对LevelDB的优化
1. 增加了column family，这样有利于多个不相关的数据集存储在同一个db中，因为不同column family的数据是存储在不同的sst和memtable中，所以一定程度上起到了隔离的作用。
2. 采用了多线程同时进行compaction的方法，优化了compact的速度。
3. 增加了merge operator，优化了modify的效率。
4. 将flush和compaction分开不同的线程池，能有效的加快flush，防止stall。
5. 增加了对write ahead log(WAL)的特殊管理机制，这样就能方便管理WAL文件，因为WAL是binlog文件。

### 4.2.4 RocksDB架构分析
优势:
1. 所有的刷盘操作都采用append方式，这种方式对磁盘和SSD是相当有诱惑力的；
2. 写操作写完WAL和Memtable就立即返回，写效率非常高。  
3. 由于最终的数据是存储在离散的SST中，SST文件的大小可以根据kv的大小自由配置，因此很适合做变长存储。

劣势：
1. 为了支持批量和事务以及上电恢复操作，WAL是多个CF共享的，导致了WAL的单线程写模式，不能充分发挥高速设备的性能优势（这是相对介质讲，相对B树等其他结构还是有优势）；
2. 读写操作都需要对Memtable进行互斥访问，在多线程并发写及读写混合的场景下容易形成瓶颈。
3. 由于Level0层的文件是按照时间顺序刷盘的，而不是根据key的范围做划分，所以导致各个文件之间范围有重叠，再加上文件自上向下的合并，读的时候有可能需要查找level0层的多个文件及其他层的文件，这也造成了很大的读放大。尤其是当纯随机写入后，读几乎是要查询level0层的所有文件，导致了读操作的低效。
4. 针对第三点问题，Rocksdb中依据level0层文件的个数来做前台写流控及后台合并触发，以此来平衡读写的性能。这又导致了性能抖动及不能发挥高速介质性能的问题。  
5. 合并流程难以控制，容易造成性能抖动及写放大。尤其是写放大问题，在笔者的使用过程中实际测试的写放大经常达到二十倍左右。这是不可接受的，当前我们也没有找到合适的解决办法，只是暂时采用大value分离存储的方式来将写放大尽量控制在小数据。

## 4.3 应用场景

### 4.3.1 适用场景
1. 对写性能要求很高，同时有较大内存来缓存SST块以提供快速读的场景；
2. SSD等对写放大比较敏感以及磁盘等对随机写比较敏感的场景；
3. 需要变长kv存储的场景；  
4. 小规模元数据的存取；

### 4.3.2 不适合场景
1. 大value的场景，需要做kv分离；
2. 大规模数据的存取

## 4.4 正在使用的企业

## 4.5 相关文章或视频

[高性能存储引擎RocksDB模块详解](https://github.com/0voice/newsql_nosql_library#nav_sec3_chapter1_02)
[RocksDB概念及简单应用](https://github.com/0voice/newsql_nosql_library#nav_sec3_chapter1_02)

# 5.MemDB
## 5.1 MemDB架构

内存数据库memdb是一个灵活的内存数据库，提供事务支持，具有SQL读写能力，支持多中心广域网集群部署，用于构建和加速需要超高速数据交互并具有高可扩展性的应用系统。在云的另一端云洞察带你去玩大数据不能做成花瓶的新鲜肉云洞察的三项高超技能富含身体系统。
![image.png](https://pic.leetcode-cn.com/1631535513-ceIIFk-image.png)

## 5.2 MemDB特点

1. MemDB是定位于【memcache、redis】与【mysql】间的一个key/value持久存储平台。
2. 与Memcache、redis不同，MemDB通过扩充存储引擎满足不同类型数据、业务规则的数据的高效存储于操作。
3. 对于不同的引擎，Unistor对外提供一致的访问API。但存储引擎可以通过MemDB API的扩展字段，对接口进行裁剪、扩展，以满足自己业务的需要。
4. MemDB虽自身不支持分组，但用户可以基于Key的范围进行划分(也可基于hash)。系统对基于key范围的数据导出提供支持。key的大小比较及hash，有用户的存储引擎决定
5. MemDB通过zookeeper实现集群以保证系统的高可用。一个集群对外不分主、从内部进行消息的转发。支持用户建立master、slave集群。
6. MemDB提供可配置的Read、write Cache以保证读写的高效。
7. MemDB有自己的binlog，保证系统数据的高可靠，而且数据同步采用多连接防止阻塞。支持高效的跨IDC数据同步。
8. MemDB提供完备的运行信息共运维使用。此信息可通过监控端口的mc stats指令获取，也可以通过get/gets接口获取，此时i参数的值为2(获取系统信息)。
9. MemDB提供统一的运维工具。
10. MemDB的存储引擎开发非常简单。

## 5.3 应用场景

属于云计算洞察包括三类产品之一，其他两类产品为大数据处理平台Hadoop、分布式并行数据库MPP

## 5.4 正在使用的企业

![image.png](https://pic.leetcode-cn.com/1631538077-EFBynL-image.png)

## 5.5 相关文章或视频

[分布式事务内存数据库--MemDB](https://github.com/0voice/newsql_nosql_library#nav_sec4_chapter4_03)
