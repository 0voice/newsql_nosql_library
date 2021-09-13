# 1. OceanBase 概述：
OceanBase是由蚂蚁金服、阿里巴巴完全自主研发的金融级分布式关系数据库，始创于2010年。OceanBase具有数据强一致、高可用、高性能、在线扩展、高度兼容SQL标准和主流关系数据库、低成本等特点。OceanBase至今已成功应用于支付宝全部核心业务：交易、支付、会员、账务等系统以及阿里巴巴淘宝（天猫）收藏夹、P4P广告报表等业务。除在蚂蚁金服和阿里巴巴业务系统中获广泛应用外，从2017年开始，OceanBase开始服务外部客户，客户包括南京银行、西安银行、印度Paytm、人保健康险等。
2021年6月：OceanBase 3.0版本正式发布，此次推出的OceanBase 3.0版本产品同时具备在事务处理和数据分析两类任务的高性能能力，升级为一款支持 HTAP 混合负载的企业级分布式数据库。同时，OceanBase宣布正式开源，并成立OceanBase开源社区，社区官网同步上线，300万行核心代码向社区开放。

# 2. OceanBase 特点：
* 高可用 单服务器故障能够自愈，支持跨城多机房容灾，数据零丢失，可满足金融行业 6 级容灾标准（RPO=0，RTO<=30 秒）。
线性扩展 透明扩展，自动负载均衡，应用透明的水平扩展，集群规模可超过 1500 节点，数据量可达 PB 级，单表记录万亿行。
* MySQL 高度兼容 兼容 MySQL 协议、语法和使用习惯，MySQL 客户端工具可以直接访问 OceanBase 数据库。
高性能 准内存级数据变更操作、独创的编码压缩技术，结合线性水平扩展，TPC-C 测试达到 7.07 亿 tpmC。
* 低成本 使用 PC 服务器和低端 SSD，高存储压缩率降低存储成本，高性能降低计算成本，多租户混部充分利用系统资源。
* 多租户 原生支持多租户构架，同一套数据库集群可以为多个独立业务提供服务，租户间数据隔离，降低部署和运维成本。
* OceanBase 数据库支持支付宝的全部核心业务，以及银行、保险、证券、运营商等多个行业的数百个客户的核心业务系统。

# 3. OceanBase 上手：
## 3.1 快速上手
查看 [快速使用指南](https://open.oceanbase.com/quickStart) 开始试用 OceanBase 数据库。
## 3.2 文档
[简体中文](https://open.oceanbase.com/docs)
英文（English）（Coming soon）
## 3.3 客户端
[OBClient](https://github.com/oceanbase/obclient)
## 3.4 许可证
OceanBase 数据库使用 [MulanPubL - 2.0](https://license.coscl.org.cn/MulanPubL-2.0/index.html) 许可证。您可以免费复制及使用源代码。当您修改或分发源代码时，请遵守木兰协议。
## 3.5 兼容性列表

# 4. OceanBase 构建
## 4.1 前提条件
在构建前您需要确认您的机器已经安装必要的软件：
## 4.2 Fedora 系列 （包括 CentOS，Fedora，OpenAnolis，RedHat 等）
```C
yum install git wget rpm* cpio make glibc-devel glibc-headers binutils
```
## 4.3 Debian 系列 （包括 Debian，ubuntu 等）
```C
apt-get install git wget rpm rpm2cpio cpio make build-essential binutils
```
## 4.4 SUSE 系列 （包括 SUSE，openSUSE 等）
```C
zypper install git wget rpm cpio make glibc-devel binutils
```
## 4.5 debug 模式
```C
bash build.sh debug --init --make
```
## 4.6 release 模式
```C
bash build.sh release --init --make
```
## 4.7 构建 RPM 软件包
```C
bash build.sh rpm --init && cd build_rpm && make -j16 rpm
```

# 5. OceanBase 其他概念
## 5.1 OceanBase 云平台
OceanBase 云平台（OceanBase Cloud Platform，OCP）伴随OceanBase数据库而生，OceanBase 云平台（OCP）是一款以 OceanBase 为核心的企业级数据库管理平台，不仅提供对OceanBase集群和租户等组件的全生命周期管理服务，同时也对OceanBase相关的资源（主机、网络和软件包等）提供管理服务，让您能够更加高效地管理OceanBase集群，降低企业的IT运维成本。
## 5.2 OceanBase 开发者中心
OceanBase 开发者中心（OceanBase Developer Center，ODC）是为 OceanBase 数据库量身打造的企业级数据库开发平台。ODC 支持连接 OceanBase 中 MySQL 和 Oracle 模式下的数据库，同时为数据库开发者提供了数据库日常开发操作、WebSQL、SQL 诊断、会话管理和数据导入导出等功能。 
## 5.3 OceanBase迁移服务
OceanBase迁移服务（OceanBaseMigrationService，OMS）是OceanBase提供的一种支持同构或异构RDBMS与OceanBase之间进行数据交互的服务，它提供了数据的在线迁移和实时增量同步的数据复制能力。
## 5.4 OceanBase公有云
构建基础设施上的公有云数据库服务，基于完全自主研发的原生分布式能力，提供7.07亿tpmC的卓越性能、PB级存储容量、万亿级单表记录、主流数据库兼容、极致弹性、异地容灾等金融级核心能力。支持一站式部署、扩容、监控运维管理、开发工具、数据迁移、备份恢复等端到端数据库服务化解决方案。 
## 5.5 OceanBase数据库一体机
OceanBase 数据库一体机是基于蚂蚁金融级分布式数据库和自研可信硬件打造的软硬一体化产品，针对数据库业务软硬件深度性能调优，提供高可靠性、高安全性、高性价比、智能管控和一站式快速交付部署能力。它极大的降低复杂性，开箱即用，让您更快享受到OceanBase分布式数据库高效之旅。
## 5.6 OceanBase数据库架构
![image.png](https://pic.leetcode-cn.com/1631354823-kxTxOK-image.png)


# 6. OceanBase系统架构
![image.png](https://pic.leetcode-cn.com/1631354835-gjaRus-image.png)
OceanBase整体架构图

## 6.1 OceanBase由如下几个部分组成：
* **客户端**：用户使用OccanBase的方式和MySQL数据库完全相同，支持JDBC、C客户端访问，等等。基于MySQL数据库开发的应用程序、工具能够直接迁移到OceanBase。
* **RootServer**:  管理集群中的所有服务器，子表（tablet）数据分布以及副本管理。RootServer一般为一主一备，主备之间数据强同步。
* **UpdateServer**: 存储OccanBase系统的增量更新数据。UpdateServer一般为一主一备，主备之间可以配置不同的同步模式。部署时，UpdateServer进程和RootServer 进程往往共用物理服务器。
* **ChunkServer**: 存储OccanBase系统的基线数据。基线数据一般存储两份或者三份，可配置。
* **MergeServer**: 接收并解析用户的sQL请求，经过词法分析、语法分析、查询优化等一系列操作后转发给相应的* ChunkServer或者UpdateServer。如果请求的数据分布在多台ChunkServer上，MergeServer 还需要对多台* ChunkServer返回的结果进行合并。客户端和MergeScrver之间采用原生的MySQL通信协议，MySQL客户端可以直接访问MergeServer。
* **OceanBase支持部署多个机房**，每个机房部署一个包含RootServer、MergeServer、ChunkServer以及UpdateServer的完整OceanBase集群，每个集群由各自的RootServer负责数据划分、负载均衡、集群服务器管理等操作，集群之间数据同步通过主集群的主UpdateServer往备集群同步增量更新操作日志实现。 客户端配置了多个集群的RootServer地址列表，使用者可以设置每个集群的流量分配比例，客户端根据这个比例将读写操作发往不同的集群。
![image.png](https://pic.leetcode-cn.com/1631354953-zRVjnb-image.png)


## 6.2 客户端
OceanBase客户端与MergeServer通信，目前主要支持如下几种客户端：
* **Mysql客户端**：MergeServer兼容Mysql协议，Mysql客户端及相关工具（如Java数据库访问方式JDBC）只需要将服务器的地址设置为任意一台MergeServer的地址就可以直接使用。
* **Java客户端**：OceanBase内部部署了多台MergeServer，Java客户端提供对Mysql标准JDBC Driver的封装，并提供流量分配、负载均衡、MergeServer异常处理等功能。简单来讲，Java客户端首先按照一定的策略定位到某台MergeServer，接着调用Mysql JDBC Driver往这台MergeServer发送读写请求。Java客户端实现符合JDBC标准，能够支持Spring、iBatis等Java编程框架。
* **C客户端**：OceanBase C客户端的功能和Java客户端类似。它首先按照一定的策略定位到某台MergeServer，接着调用Mysql 标准C客户端往这台MergeServer发送读写请求。C客户端的接口和Mysql 标准C客户端接口完全相同，因此，能够通过LD_PRELOAD的方式将应用程序依赖的Mysql标准C客户端替换为OceanBase C客户端，而无需修改应用程序的代码。
OceanBase集群有多台MergeServer，这些MergeServer的服务器地址存储在OceanBase服务器端的系统表（与Oracle的系统表类似，存储OceanBase系统的元数据）内。OceanBase Java/C客户端首先请求服务器端获取MergeServer地址列表，接着按照一定的策略将读写请求发送给某台MergeServer，并负责对出现故障的MergeServer进行容错处理。
Java/C客户端访问OceanBase的流程大致如下：

### 6.2.1 请求RootServer获取集群中MergeServer的地址列表。
### 6.2.2 按照一定的策略选择某台MergeServer发送读写请求。客户端与MergeServer之间的通信协议兼容原生的Mysql协议，因此，只需要调用Mysql JDBC Driver或者Mysql C客户端这样的标准库即可。客户端支持的策略主要有两种：随机以及一致性哈希。一致性哈希的主要目的是将相同的SQL请求发送到同一台MergeServer，方便MergeServer对查询结果进行缓存。
### 6.2.3 如果请求MergeServer失败，则从MergeServer列表中重新选择一台MergeServer重试；如果请求某台MergeServer失败超过一定的次数，将这台MergeServer加入黑名单并从MergeServer列表中删除。另外，客户端会定期请求RootServer更新MergeServer地址列表。
如果OceanBase部署多个集群，客户端还需要处理多个集群的流量分配问题。使用者可以设置多个集群之间的流量分配比例，客户端获取到流量分配比例后，按照这个比例将请求发送到不同的集群。
OceanBase程序升级版本时，往往先将备集群的读取流量调整为0，这时所有的读写请求都只发往主集群，接着升级备集群的程序版本。备集群升级完成后将流量逐步切换到备集群观察一段时间，如果没有出现异常，则将所有的流量切到备集群，并将备集群切换为主集群提供写服务。原来的主集群变为新的备集群，升级新的备集群的程序版本后重新分配主备集群的流量比例。

## 6.3 RootServer
RootServer的功能主要包括：集群管理、数据分布以及副本管理。
* RootServer管理集群中的所有MergeServer、ChunkServer以及UpdateServer。每个集群内部同一时刻只允许一个UpdateServer提供写服务，这个UpdateServer成为主UpdateServer。这种方式通过牺牲一定的可用性获取了强一致性。RootServer通过租约（Lease）机制选择唯一的主UpdateServer，当原先的主UpdateServer发生故障后，RootServer能够在原先的租约失效后选择一台新的UpdateServer作为主UpdateServer。另外，RootServer与MergeServer&ChunkServer之间保持心跳（heartbeat），从而能够感知到在线和已经下线的MergeServer&ChunkServer机器列表。
* OceanBase内部使用主键对表格中的数据进行排序和存储，主键由若干列组成并且具有唯一性。在OceanBase内部，基准数据按照主键排序并且划分为数据量大致相等的数据范围，称为tablet。 每个tablet的缺省大小是256MB（可配置）。OceanBase的数据分布方式与Bigtable一样采用顺序分布，不同的是，OceanBase没有采用RootTable + MetaTable两级索引结构，而是采用RootTable一级索引结构。
![image.png](https://pic.leetcode-cn.com/1631355140-SGvWSl-image.png)


如上图所示，主键值在[1, 100]之间的表格被划分为四个tablet：1~ 25，26~ 50，51~ 80以及81~100。RootServer中的RootTable记录了每个tablet所在的ChunkServer位置信息，每个tablet包含多个副本（一般为三个副本，可配置），分布在多台ChunkServer中。当其中某台ChunkServer发生故障时，RootServer能够检测到，并且触发对这台ChunkServer上的tablet增加副本的操作；另外，RootServer也会定期执行负载均衡，选择某些tablet从负载较高的机器迁移到负载较低的机器。
RootServer采用一主一备的结构，主备之间数据强同步，并通过Linux HA（http://www.linux-ha.org）软件实现高可用性。 主备RootServer之间共享VIP，当主RootServer发生故障后，VIP能够自动漂移到备RootServer所在的机器，备RootServer检测到以后切换为主RootServer提供服务。

### 6.3.1 MergeServer
MergeServer的功能主要包括：协议解析、SQL解析、请求转发、结果合并、多表操作等。
OceanBase客户端与MergeServer之间的协议为Mysql协议。MergeServer首先解析Mysql协议，从中提取出用户发送的SQL语句，接着进行词法分析和语法分析，生成SQL语句的逻辑查询计划和物理查询计划，最后根据物理查询计划调用OceanBase内部的各种操作符。
MergeServer缓存了tablet分布信息，根据请求涉及的tablet将请求转发给该tablet所在的ChunkServer。如果是写操作，还会转发给UpdateServer。某些请求需要跨多个tablet，此时MergeServer会将请求拆分后发送给多台ChunkServer，并合并这些ChunkServer返回的结果。如果请求涉及到多个表格，MergeServer需要首先从ChunkServer获取每个表格的数据，接着再执行多表关联或者嵌套查询等操作。
MergeServer支持并发请求多台ChunkServer，即将多个请求发给多台ChunkServer，再一次性等待所有请求的应答。另外，在SQL执行过程中，如果某个tablet所在的ChunkServer出现故障，MergeServer会将请求转发给该tablet的其他副本所在的ChunkServer。这样，ChunkServer故障是不会影响用户查询的。
MergeServer本身是没有状态的，因此，MergeServer宕机不会对使用者产生影响，客户端会自动将发生故障的MergeServer屏蔽掉。

### 6.3.2  ChunkServer
ChunkServer的功能包括：存储多个tablet、提供读取服务、执行定期合并以及数据分发。
OceanBase将大表划分为大小约为256MB的tablet，每个tablet由一个或者多个SSTable组成（一般为一个），每个SSTable由多个块（Block，大小为4KB ~ 64KB之间，可配置）组成，数据在SSTable中按照主键有序存储。查找某一行数据时，需要首先定位这一行所属的tablet，接着在相应的SSTable中执行二分查找。 SSTable支持两种缓存模式，Block Cache以及Row Cache。Block Cache以Block为单位缓存最近读取的数据，Row Cache以行为单位缓存最近读取的数据。
MergeServer将每个tablet的读取请求发送到tablet所在的ChunkServer，ChunkServer首先读取SSTable中包含的基准数据，接着请求UpdateServer获取相应的增量更新数据，并将基准数据与增量更新融合后得到最终结果。
由于每次读取都需要从UpdateServer中获取最新的增量更新，为了保证读取性能，需要限制UpdateServer中增量更新的数据量，最好能够全部存放在内存中。 OceanBase内部会定期触发合并或者数据分发操作，在这个过程中，ChunkServer将从UpdateServer获取一段时间之前的更新操作。通常情况下，OceanBase集群会在每天的服务低峰期（凌晨1:00开始，可配置）执行一次合并操作。这个合并操作往往也称为每日合并。

### 6.3.3 UpdateServer
UpdateServer是集群中唯一能够接受写入的模块，每个集群中只有一个主UpdateServer。UpdateServer中的更新操作首先写入到内存表，当内存表的数据量超过一定值时，可以生成快照文件并转储到SSD中。 快照文件的组织方式与ChunkServer中的SSTable类似，因此，这些快照文件也称为SSTable。另外，由于数据行的某些列被更新，某些列没被更新，SSTable中存储的数据行是稀疏的，称为稀疏型SSTable。
为了保证可靠性，主UpdateServer更新内存表之前需要首先写操作日志，并同步到备UpdateServer。当主UpdateServer发生故障时，RootServer上维护的租约将失效，此时，RootServer将从备UpdateServer列表中选择一台最新的备UpdateServer切换为主UpdateServer继续提供写服务。 UpdateServer宕机重启后需要首先加载转储的快照文件（SSTable文件），接着回放快照点之后的操作日志。
由于集群中只有一台主UpdateServer提供写服务，因此，OceanBase很容易地实现了跨行跨表事务，而不需要采用传统的两阶段提交协议。当然，这样也带来了一系列的问题。由于整个集群所有的读写操作都必须经过UpdateServer，UpdateServer的性能至关重要。OceanBase集群通过定期合并和数据分发这两种机制将UpdateServer一段时间之前的增量更新源源不断地分散到ChunkServer，而UpdateServer只需要服务最新一小段时间新增的数据，这些数据往往可以全部存放在内存中。 另外，系统实现时也需要对UpdateServer的内存操作、网络框架、磁盘操作做大量的优化。

### 6.3.4 定期合并&数据分发
定期合并和数据分发都是将UpdateServer中的增量更新分发到ChunkServer中的手段，二者的整体流程比较类似：
#### 6.3.4.1 UpdateServer冻结当前的活跃内存表（Active MemTable），生成冻结内存表，并开启新的活跃内存表，后续的更新操作都写入新的活跃内存表。
#### 6.3.4.2 UpdateServer通知RootServer数据版本发生了变化，之后RootServer通过心跳消息通知ChunkServer。
#### 6.3.4.3 每台ChunkServer启动定期合并或者数据分发操作，从UpdateServer获取每个tablet 对应的增量更新数据。
定期合并与数据分发两者之间的不同点在于，数据分发过程中ChunkServer只是将UpdateServer中冻结内存表中的增量更新数据缓存到本地，而定期合并过程中ChunkServer需要将本地SSTable中的基准数据与冻结内存表的增量更新数据执行一次多路归并，融合后生成新的基准数据并存放到新的SSTable中。定期合并对系统服务能力影响很大，往往安排在每天服务低峰期执行（例如凌晨1点开始），而数据分发可以不受限制。

![image.png](https://pic.leetcode-cn.com/1631355175-vCAglw-image.png)


**定期合并不停读服务**
如上图所示，活跃内存表冻结后生成冻结内存表，后续的写操作进入新的活跃内存表。定期合并过程中ChunkServer需要读取UpdateServer中冻结内存表的数据、融合后生成新的Tablet，即：
```C
新Tablet = 旧Tablet + 冻结内存表
```
虽然定期合并过程中各个ChunkServer的各个Tablet合并时间和完成时间可能都不相同，但并不影响读取服务。如果tablet没有合并完成，那么使用旧Tablet，并且读取UpdateServer中的冻结内存表以及新的活跃内存表；否则，使用新Tablet，只读取新的活跃内存表，即：
```C
查询结果 = 旧Tablet + 冻结内存表 + 新的活跃内存表 = 新Tablet + 新的活跃内存表
```

# 7. 架构剖析
## 7.1 一致性选择
Eric Brewer教授的CAP理论指出，在满足分区可容忍性的前提下，一致性和可用性不可兼得。
虽然目前大量的互联网项目选择了弱一致性，但我们认为这是底层存储系统，比如Mysql数据库，在大数据量和高并发需求压力之下的无奈选择。弱一致性给应用带来了很多麻烦，比如数据不一致时需要人工订正数据。如果存储系统既能够满足大数据量和高并发的需求，又能够提供强一致性，且硬件成本相差不大，用户将毫不犹豫地选择它。强一致性将大大简化数据库的管理，应用程序也会因此而简化。因此，OceanBase选择支持强一致性和跨行跨表事务。
OceanBase UpdateServer为主备高可用架构，更新操作流程如下：
* 将更新操作发送到备机；
* 将更新操作的redo日志写入主机硬盘；
* 将redo日志应用到主机的内存表格中；
* 返回客户端写入成功。
OceanBase要求将redo日志同步到主备的情况下才能够返回客户端写入成功，即使主机出现故障，备机自动切换为主机，也能够保证新的主机拥有以前所有的更新操作，严格保证数据不丢失。另外，为了提高可用性，OceanBase还增加了一种机制，如果主机往备机同步redo日志失败，比如备机故障或者主备之间网络故障，主机可以将备机从同步列表中剔除，本地更新成功后就返回客户端写入成功。主机将备机剔除前需要通知RootServer，后续如果主机故障，RootServer能够避免将不同步的备机切换为主机。
OceanBase的高可用机制保证主机、备机以及主备之间网络三者之中的任何一个出现故障都不会对用户产生影响，然而，如果三者之中的两个同时出现故障，系统可用性将受到影响，但仍然保证数据不丢失。如果应用对可用性要求特别高，可以增加备机数量，从而容忍多台机器同时出现故障的情况。
OceanBase主备同步也允许配置为异步模式，支持最终一致性。这种模式一般用来支持异地容灾。例如，用户请求通过杭州主站的机房提供服务，主站的UpdateServer内部有一个同步线程不停地将用户更新操作发送到青岛机房。如果杭州机房整体出现不可恢复的故障，比如地震，还能够通过青岛机房恢复数据并继续提供服务。
另外，OceanBase所有写事务最终都落到UpdateServer，而UpdateServer逻辑上是一个单点，支持跨行跨表事务，实现上借鉴了传统关系数据库的做法。

## 7.2 数据结构
OceanBase数据分为基准数据和增量数据两个部分，基准数据分布在多台ChunkServer上，增量数据全部存放在一台UpdateServer上。 如下图所示，系统中有5个tablet，每个tablet有3个副本，所有的tablet分布到4台ChunkServer上。RootServer中维护了每个tablet所在的ChunkServer的位置信息，UpdateServer存储了这5个tablet的增量更新。

![image.png](https://pic.leetcode-cn.com/1631355247-xhYPMX-image.png)

OceanBase数据结构
* 不考虑数据复制，基准数据的数据结构如下：
* 每个表格按照主键组成一颗分布式B+树，主键由若干列组成；
* 每个叶子节点包含表格一个前开后闭的主键范围(rk1，rk2]内的数据；
* 每个叶子节点称为一个子表（tablet），包含一个或者多个SSTable；
* 每个SSTable内部按主键范围有序划分为多个块（block）并内建块索引（block index）；每个SSTable内部按主键* 范围有序划分为多个块（block）并内建块索引（block index）；
* 每个块的大小通常在4KB ~ 64KB之间并内建块内的行索引；每个块的大小通常在4KB ~ 64KB之间并内建块内的行索引；
* 数据压缩以块为单位，压缩算法由用户并可随时变更；数据压缩以块为单位，压缩算法由用户并可随时变更；
* 叶子节点可能合并或者分裂；叶子节点可能合并或者分裂；
* 所有叶子节点基本上是均匀的，随机地分布在多台ChunkServer机器上；
* 通常情况下每个叶子节点有2~ 3个副本；通常情况下每个叶子节点有2~3个副本；
* 叶子节点时负载平衡和任务调度的基本单元； 叶子节点时负载平衡和任务调度的基本单元；
* 支持bloom filter过滤；

增量数据的数据结构如下：
* 增量数据按照时间从旧到新划分为多个版本；
* 最新版本的数据为一颗内存中的B+树，称为Active Memtable；
* 用户的更新操作写入Active Memtable，到达一定大小后，原有的Active Memtable将被冻结，并开启新的Active Memtable接受更新操作；
* 冻结的Memtable将以SSTable的形式转储到SSD中持久化；
* 每个SSTable内部按主键范围有序划分为多个块并内建块索引，每个块的大小通常为4KB ~ 8KB并内建块内行索引，一般不压缩；
* UpdateServer支持主备，增量数据通常为2个副本，每个副本支持RAID1存储；

## 7.3 可靠性与可用性
分布式系统需要处理各种故障，例如软件故障，服务器故障，网络故障，数据中心故障，地震，火灾，等。与其它分布式存储系统一样，OceanBase通过冗余的方式保障了高可靠性和高可用性。
* OceanBase在ChunkServer中保存了基准数据的多个副本。单集群部署时一般会配置3个副本，主备集群部署时一般会配置每个集群2个副本，总共4个副本。
* OceanBase在UpdateServer中保存了增量数据的多个副本。UpdateServer主备模式下主备两台机器各保存一个副本，另外，每台机器都通过软件的方式实现了RAID1，将数据自动复制到多块磁盘，进一步增强了可靠性。
* ChunkServer的多个副本可以同时提供服务。Bigtable以及HBase这样的系统服务节点不冗余，如果服务器出现故障，需要等待其它节点恢复成功才能提供服务，而OceanBase多个ChunkServer的tablet副本数据完全一致，可以同时提供服务。
* UpdateServer主备之间为热备，同一时刻只有一台机器为主UpdateServer提供写服务。如果主UpdateServer发生故障，OceanBase能够在几秒中之内（一般为3~5秒）检测到并将服务切换到备机，备机几乎没有预热时间。
* OceanBase存储多个副本并没有带来太多的成本。当前的主流服务器的磁盘容量通常是富余的，例如300GB×12或600GB×12的服务器有3TB或6TB左右的磁盘总容量，但存储系统单机通常只能服务少得多的数据量。

## 7.4 读写事务
在OceanBase系统中，用户的读写请求，即读写事务，都发给MergeServer。MergeServer解析这些读写事务的内容，例如词法和语法分析、schema检查等。对于只读事务，由MergeServer发给相应的ChunkServer分别执行后再合并每个ChunkServer的执行结果；对于读写事务，由MergeServer进行预处理后，发送给UpdateServer执行。
只读事务执行流程如下：
1. MergeServer解析SQL语句，词法分析、语法分析、预处理（schema合法性检查、权限检查、数据类型检查等），最后生成逻辑执行计划和物理执行计划。
2. 如果SQL请求只涉及单张表格，MergeServer将请求拆分后同时发给多台ChunkServer并发执行，每台ChunkServer将读取的部分结果返回MergeServer，由MergeServer来执行结果合并。如果SQL请求只涉及单张表格，MergeServer将请求拆分后同时发给多台ChunkServer并发执行，每台ChunkServer将读取的部分结果返回MergeServer，由MergeServer来执行结果合并。
3. 如果SQL请求涉及多张表格，MergeServer还需要执行联表、嵌套查询等操作。如果SQL请求涉及多张表格，MergeServer还需要执行联表、嵌套查询等操作。
4. MergeServer将最终结果返回给客户端。MergeServer将最终结果返回给客户端。

读写事务执行流程如下：
1. 与只读事务相同，MergeServer首先解析SQL请求，得到物理执行计划。
2. MergeServer请求ChunkServer获取需要读取的基线数据，并将物理执行计划和基线数据一起传给UpdateServer。
3. UpdateServer根据物理执行计划执行读写事务，执行过程中需要使用MergeServer传入的基线数据。
4. UpdateServer返回MergeServer操作成功或者失败，MergeServer接着会把操作结果返回客户端。UpdateServer返回MergeServer操作成功或者失败，MergeServer接着会把操作结果返回客户端。

例如，假设某SQL语句为：“update t1 set c1 = c1 + 1 where rowkey=1”，即将表格t1中主键为1的c1列加1，这一行数据存储在ChunkServer中，c1列的值原来为2012。那么，MergeServer执行SQL时首先从ChunkServer读取主键为1的数据行的c1列，接着将读取结果（c1=2012）以及SQL语句的物理执行计划一起发送给UpdateServer。UpdateServer根据物理执行计划将c1加1，即将c1变为2013并记录到MemTable中。当然，更新MemTable之前需要记录操作日志。

## 7.5 单点性能
OceanBase架构的优势在于既支持跨行跨表事务，又支持存储服务器线性扩展。当然，这个架构也有一个明显的缺陷：UpdateServer单点，这个问题限制了OceanBase集群的整体读写性能。
下面从内存容量、网络、磁盘等几个方面分析UpdateServer的读写性能。其实大部分数据库每天的修改次数相当有限，只有少数修改比较频繁的数据库才有每天几亿次的修改次数。另外，数据库平均每次修改涉及的数据量很少，很多时候只有几十个字节到几百个字节。假设数据库每天更新1亿次，平均每次需要消耗100字节，每天插入1000万次，平均每次需要消耗1000字节，那么，一天的修改量为：1亿 * 100 + 1000万 * 1000 = 20GB，如果内存数据结构膨胀2倍，占用内存只有40GB。而当前主流的服务器都可以配置96GB内存，一些高档的服务器甚至可以配置192GB，384GB乃至更多内存。
从上面的分析可以看出，UpdateServer的内存容量一般不会成为瓶颈。然而，服务器的内存毕竟有限，实际应用中仍然可能出现修改量超出内存的情况。例如，淘宝双11网购节数据库修改量暴涨，某些特殊应用每天的修改次数特别多或者每次修改的数据量特别大，DBA数据订正时一次性写入大量数据。为此，UpdateServer设计实现了几种方式解决内存容量问题，UpdateServer的内存表达到一定大小时，可自动或者手工冻结并转储到SSD中，另外，OceanBase支持通过定期合并或者数据分发的方式将UpdateServer的数据分散到集群中所有的ChunkServer机器中，这样不仅避免了UpdateServer单机数据容量问题，还能够使得读取操作往往只需要访问UpdateServer内存中的数据，避免访问SSD磁盘，提高了读取性能。
从网络角度看，假设每秒的读取次数为20万次，每次需要从UpdateServer中获取100字节，那么，读取操作占用的UpdateServer出口带宽为：20万 * 100 = 20MB，远远没有达到千兆网卡带宽上限。另外，UpdateServer还可以配置多块千兆网卡或者万兆网卡，例如，OceanBase线上集群一般给UpdateServer配置4块千兆网卡。当然，如果软件层面没有做好，硬件特性将得不到充分发挥。针对UpdateServer全内存、收发的网络包一般比较小的特点，开发团队对UpdateServer的网络框架做了专门的优化，大大提高了每秒收发网络包的个数，使得网络不会成为瓶颈。
从磁盘的角度看，数据库事务需要首先将操作日志写入磁盘。如果每次写入都需要将数据刷入磁盘，而一块SAS磁盘每秒支持的IOPS很难超过300，磁盘将很快成为瓶颈。为了解决这个问题，UpdateServer在硬件上会配置一块带有缓存模块的RAID卡，UpdateServer写操作日志只需要写入到RAID卡的缓存模块即可，延时可以控制在1毫秒之内。RAID卡带电池，如果UpdateServer发生故障，比如机器突然停电，RAID卡能够确保将缓存中的数据刷入磁盘，不会出现丢数据的情况。另外，UpdateServer还实现了写事务的group commit机制，将多个用户写操作凑成一批一次性提交，进一步减少磁盘IO次数。

## 7.6 SSD支持
磁盘随机IO是存储系统性能的决定因素，传统的SAS盘能够提供的IOPS不超过300。关系数据库一般采用Buffer Cache的方式缓解这个问题，读取操作将磁盘中的页面缓存到Buffer Cache中，并通过LRU或者类似的方式淘汰不经常访问的页面；同样，写入操作也是将数据写入到Buffer Cache中，由Buffer Cache按照一定的策略将内存中页面的内容刷入磁盘。这种方式面临一些问题，例如Cache冷启动问题，即数据库刚启动时性能很差，需要将读取流量逐步切入。另外，这种方式不适合写入特别多的场景。
最近几年，SSD磁盘取得了很大的进展，它不仅提供了非常好的随机读取性能，功耗也非常低，大有取代传统机械磁盘之势。一块普通的SSD磁盘可以提供35000 IOPS甚至更高，并提供300MB/s或以上的读出带宽。然而，SSD盘的随机写性能并不理想。这是因为，尽管SSD的读和写以页（page，例如4KB，8KB等）为单位，但SSD写入前需要首先擦除已有内容，而擦除以块（block）为单位，一个（block）由若干个连续的页（page）组成，大小通常在512KB ~ 2MB左右。假如写入的页（page）有内容，即使只写入一个字节，SSD也需要擦除整个512KB ~ 2MB大小的块（block），然后再写入整个页（page）的内容，这就是SSD的写入放大效应。虽然SSD硬件厂商都针对这个问题做了一些优化，但整体上看，随机写入不能发挥SSD的优势。
OceanBase设计之初就认为SSD为大势所趋，整个系统设计时完全摒弃了随机写：除了操作日志总是顺序追加写入到普通SAS盘上，剩下的写请求都是对响应时间要求不是很高的批量顺序写，SSD盘可以轻松应对，而大量查询请求的随机读，则发挥了SSD良好的随机读的特性。摒弃随机写，采用批量的顺序写，也使得固态盘的使用寿命不再成为问题：主流SSD盘使用MLC SSD芯片，而MLC号称可以擦写1万次（SLC可以擦写10万次，但因成本高而较少使用），即使按最保守的2500次擦写次数计算，而且每天全部擦写一遍，其使用寿命为2500/365=6.8年。

## 7.7 数据正确性
数据丢失或者数据错误对于存储系统来说是一种灾难。前面8.4.1节中已经提到，OceanBase设计为强一致性系统，设计方案上保证不丢数据。然而，TCP协议传输、磁盘读写都可能出现数据错误，程序Bug则更为常见。为了防止各种因素导致的数据损毁，OceanBase采取了以下数据校验措施：
* 数据存储校验
每个存储记录（通常是几个KB到几十KB）同时保存64位CRC校验码，数据被访问时，重新计算和比对校验码。
* 数据传输校验
每个传输记录同时传输64位CRC校验码，数据被接收后，重新计算和比对校验码。
* 数据镜像校验
UpdateServer在机群内有主UpdateServer和备UpdateServer，集群间有主集群和备集群，这些UpdateServer的内存表（memtable）必须保持一致。为此，UpdateServer为memtable生成一个校验码，memtable每次更新时，校验码同步更新并记录在对应的commit log中。备UpdateServer收到commit log重放更新memtable时，也同步更新memtable校验码并与接收到的校验码对照。UpdateServer重新启动后重放日志恢复memtable时也同步更新memtable校验码并与保存在每条commit log中校验码对照。
* 数据副本校验
定期合并时，新的tablet由各个ChunkServer独立地融合旧的tablet与冻结的memtable而生成，如果发生任何异常或者错误（比如程序bug），同一tablet的多个副本可能不一致，则这种不一致可能随着定期合并而逐步累积或扩散且很难被发现，即使被察觉，也可能因为需要追溯较长时间而难以定位到源头。为了防止这种情况出现，ChunkServer在定期合并生成新的tablet时，也同时为每个tablet生成一个校验码，并随新tablet汇报给RootServer，以便RootServer核对同一tablet不同副本的校验码。

## 7.8 分层结构
OceanBase对外提供的是与关系数据库一样的SQL操作接口，而内部却实现成一个线性可扩展的分布式系统。系统从逻辑实现上可以分为两个层次：分布式存储引擎层以及数据库功能层。
OceanBase一期只实现了分布式存储引擎，这个存储引擎支持如下特性：
* 支持分布式数据结构，基线数据逻辑上构成一颗分布式B+树，增量数据为内存中的B+树；
* 支持目前OceanBase的所有分布式特性，包括数据分布、负载均衡、主备同步、容错、自动增加/减少服务器，等；
* 支持根据主键更新、插入、删除、随机读取一条记录，另外，支持根据主键范围顺序查找一段范围的记录；
二期的OceanBase版本在分布式存储引擎之上增加了SQL支持：
* 支持SQL语言以及Mysql协议，Mysql客户端可以直接访问；
* 支持读写事务；
* 支持多版本并发控制；
* 支持读事务并发执行；

从另外一个角度看，OceanBase融合了分布式存储系统和关系数据库这两种技术。通过分布式存储技术将基准数据分布到多台ChunkServer，实现数据复制、负载均衡、服务器故障检测与自动容错，等等；UpdateServer相当于一个高性能的内存数据库，底层采用关系数据库技术实现。我们后来发现，有一个号称“世界上最快的内存数据库”MemSQL采用了和OceanBase UpdateServer类似的设计，在拥有64个CPU核心的服务器上实现了每秒150万次单行写事务。OceanBase相当于GFS + MemSQL，ChunkServer的实现类似GFS，UpdateServer的实现类似MemSQL，目标是成为可扩展的、支持每秒百万级单行事务操作的分布式数据库。
