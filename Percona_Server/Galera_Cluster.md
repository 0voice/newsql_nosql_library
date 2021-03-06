# Galera Cluster

何谓Galera Cluster？就是集成了Galera插件的MySQL集群，是一种新型的，数据不共享的，高度冗余的高可用方案，目前Galera Cluster有两个版本，分别是Percona Xtradb Cluster（也成PXC）及MariaDB Cluster，都是基于Galera的，所以这里都统称为Galera Cluster了，因为Galera本身是具有多主特性的，所以Galera Cluster也就是multi-master的集群架构，如图<br/>
![image](https://user-images.githubusercontent.com/87458342/132350016-c7a49ea4-fa97-46ff-9234-e963613d00b8.png)

## 1. 优点
* 1. 真正的多主，群集随时读取和写入任何节点。
* 2. 同步复制没有从属滞后，在节点崩溃时不会丢失任何数据。
* 3. 紧密耦合所有节点都保持相同状态。节点之间不允许有分散的数据。
* 4. 对于任何工作量，使用多线程从站可以获得更好的性能。
* 5. 没有主从故障转移操作或VIP使用。
* 6. 热备在故障转移期间没有停机时间（因为没有故障转移）。
* 7. 自动节点置备无需手动备份数据库并将其复制到新节点。
* 8. 支持InnoDB。
* 9. 对应用程序透明对应用程序无要求（或进行最小的更改）。
* 10. 无需读写拆分。
* 11. 易于使用和部署

## 2. Galera复制
Galera复制在事务提交时发生，方法是将事务写集广播到群集以进行应用<br/>
客户端直接连接到DBMS，并且体验接近本机DBMS的行为<br/>
wsrep API（写集复制API），定义Galera复制和DBMS之间的接口<br/>

## 3. 同步与异步复制
同步复制和异步复制之间的基本区别在于，
1. “同步”可确保如果更改在集群的一个节点上发生，则更改将在“其他”节点上发生。
2. “异步”不保证在“主”节点上应用更改与将更改传播到“从”节点之间的延迟。延迟可能是短暂的，也可能是较长的–这是运气的问题。这还意味着，如果主节点崩溃，则一些最新更改可能会丢失。

理论上，同步复制比异步复制具有许多优点：
1. 它始终是高度可用的（当其中一个节点崩溃时，数据不会丢失，并且数据副本始终是一致的）
2. 事务可以在所有节点上并行执行。
3. 它可以保证整个集群的因果关系（在事务T之后发出的SELECT S将始终看到事务的效果，即使它在另一个节点上执行也是如此）

但是，实际上，同步数据库复制传统上是通过所谓的“两阶段提交”或分布式锁定来实现的，事实证明这很慢。同步复制的低性能和复杂性实施导致异步复制仍然是数据库性能可伸缩性和可用性的主要手段。广泛采用的开源数据库（例如 MySQL 或 PostgreSQL） 仅提供异

步复制解决方案。

## 4. 基于认证的复制方法
基于认证的复制的主要思想是，假定没有冲突，事务将按常规执行直到到达提交点为止。这称为乐观执行。<br/>
实现原理：
1. 当客户端发出COMMIT命令时，但在实际提交之前，由事务和已更改行的主键对数据库所做的所有更改都将收集到一个写集中。然后，数据库将此写集发送到所有其他节点。
2. 然后，使用主键对写集进行确定性认证测试。这是在群集中的每个节点上完成的，包括源自写集的节点。它确定节点是否可以应用写集。
3. 如果认证测试失败，则节点将丢弃写集，并且群集将回滚原始事务。如果测试成功，则提交事务，并将写集应用于集群的其余部分。

许多研究人员提出了一种使用组通信和事务排序技术的同步数据库复制的替代方法（例如， 数据库状态机方法 和“ 不要懒”，“保持一致”），并且原型实现已显示出很大的希望。我们将同步数据库复制方面的经验与该领域的最新研究相结合，创建了Galera Replication Toolkit。<br/>
Galera复制是 用于应用程序集群的 高度透明和可扩展的同步复制解决方案，以实现高可用性和改进的性能。基于Galera的集群为：

* 高度可用
* 高度透明
* 高度可扩展（取决于应用程序，可以达到接近线性的可扩展性）

## 5. 一些use case
### 1. read master
但对于Galera而言，所有“从”节点始终都是有能力的主节点，只有应用程序将它们视为从属节点。Galera复制可以确保此类安装的从站延迟为0，并且由于并行应用从站，因此群集的吞吐量要好得多。

### 2. 写可扩展性
在群集中分布写入将利用从属节点中的CPU功能，以更好地用于处理客户端写入事务。由于基于行的复制方法，将仅复制在客户端事务期间进行的更改，并且在从属应用程序中应用此类事务比处理原始事务要快得多。因此，群集可以在许多主节点之间分配繁重的客户端事务处理，从而总体上产生更好的写入事务吞吐量。

### 3. 广域网集群
同步复制可以在WAN网络上正常工作。将存在与网络往返时间（RTT）成比例的延迟，但它只会影响提交操作

### 4. 灾难恢复
灾难恢复是WAN复制的子类。在这里，一个数据中心是被动的，仅接收复制事件，但不处理任何客户端事务。这样的远程数据中心将始终保持最新状态，并且不会丢失任何数据。在恢复过程中，备用站点仅被指定为主站点，并且应用程序可以正常运行，故障转移延迟最小。

### 5. 延迟橡皮擦
借助WAN复制拓扑，群集节点可以放置在靠近客户端的位置，因此在本地节点连接下所有读写操作都将非常快。与RTT相关的延迟将仅在提交时经历，并且即使那时它也可以被最终用户普遍接受，通常，最终用户体验的杀戮在于缓慢的浏览响应时间，并且读取操作尽可能快。

<br/>
<br/>
<br/>
<br/>
<br/>

源于: [https://galeracluster.com/products/](https://galeracluster.com/products/)

<br/>
<br/>
<br/>

原文作者： whx@Flora


