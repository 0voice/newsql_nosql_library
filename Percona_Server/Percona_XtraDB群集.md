# Percona XtraDB群集

## 1. Percona XtraDB群集的功能包括：

### 1) 同步复制
数据同时写入所有节点，或者即使在单个节点上失败也不会写入。

### 2) Multi-master复制
任何节点都可以触发数据更新。

### 3) 真正的并行复制
从属服务器上的多个线程在行级别执行复制。

### 4) 自动节点配置
您只需添加一个节点，它就会自动同步。

### 5) 资料一致性
不再有未同步的节点。

### 6) PXC严格模式
避免使用实验性和不受支持的功能。

### 7) ProxySQL的配置脚本
Percona为ProxySQL软件包提供了proxysql-admin自动配置Percona XtraDB Cluster节点的工具。

### 8) 自动配置SSL加密
Percona XtraDB Cluster包含pxc-encrypt-cluster-traffic启用S​​SL加密自动配置的变量。

### 9) 优化性能
Percona XtraDB群集性能经过优化，可以随着不断增加的生产工作量进行扩展。

## 2. 关于Percona XtraDB群集
集群节点，其中每个节点包含相同的一组节点同步的数据。推荐的配置是至少有3个节点，但是您也可以有2个节点。每个节点都是常规的MySQL Server实例（例如，Percona Server）。您可以将现有的MySQL Server实例转换为一个节点，并以该节点为基础运行集群。您还可以从群集中分离任何节点，并将其用作常规MySQL Server实例。
![image](https://user-images.githubusercontent.com/87458342/132347950-3457d142-90a7-4cae-87ab-41bcacc8f6a3.png)

### 好处：
* 执行查询时，它在本地节点上执行。所有数据都在本地可用，无需远程访问。
* **没有中央管理。**您可以在任何时间点松开任何节点，群集将继续运行而不会丢失任何数据。
* 扩展读取工作负载的好解决方案。您可以将读取查询放入任何节点。

### 缺点：
* 设置新节点的开销。添加新节点时，它必须从一个现有节点中复制完整的数据集。如果是100GB，它将复制100GB。
* 这不能用作有效的写扩展解决方案。当您运行到2个节点的写流量相对于所有到1个节点的写流量时，写吞吐量可能会有所改善，但是期望不高。所有写入仍必须在所有节点上进行。
* 您有几个数据重复项，对于3个节点，您有3个重复项。

## 3. 组件
Percona XtraDB群集基于与XtraDB存储引擎一起运行的Percona Server。它使用Galera库，该库是Codership Oy开发的写集复制（wsrep）API的实现。推荐的默认数据传输方法是通过Percona XtraBackup，其他的传输方式还有mysqldump、rsync。

## 4. Percona XtraDB群集限制

以下限制适用于Percona XtraDB群集：

* 1) 复制仅适用于InnoDB存储引擎。mysql.*不会复制对其他类型的表（包括系统（）表）的任何写操作。但是，DDL语句是在语句级别（statement）对mysql.*复制的，对表的更改将以这种方式复制。因此，您可以放心发行，但不会重复发行。您可以使用变量启用实验性MyISAM复制支持。CREATE USER…INSERT INTO mysql.user…wsrep_replicate_myisam
* 2) 不支持的查询：

#### 表锁 ：LOCK TABLES并且在多主机设置中不受支持UNLOCK TABLES
锁定功能，如GET_LOCK()，RELEASE_LOCK()等
查询日志无法定向到表。如果启用查询日志记录，则必须将日志转发到文件：log_output = FILE
使用general_log和general_log_file选择查询日志记录和日志文件名。

#### 允许的最大交易规模由wsrep_max_ws_rows和wsrep_max_ws_size变量定义 。 处理将每10000行提交一次。因此，由于要 进行的大笔交易将分成一系列小笔交易。LOAD DATA INFILELOAD DATA

**–由于群集级别的乐观并发控制，COMMIT在此阶段可能仍中止事务发布。**可以有两个事务写入同一行并在不同的Percona XtraDB Cluster节点中提交，而其中只有一个可以成功提交。失败的将被中止。对于集群级中止，Percona XtraDB集群返回死锁错误代码：
（错误： 1213 SQLSTATE ： 40001 （ER_LOCK_DEADLOCK ）
不支持XA事务，因为可能会在提交时回滚。

#### 整个群集的写入吞吐量受最弱节点的限制。如果一个节点变慢，则整个群集会变慢。如果您需要稳定的高性能，则应由相应的硬件支持。
#### 建议的最小群集大小为3个节点。第三节点可以是仲裁器。
#### 不支持InnoDB伪造更改功能。

enforce_storage_engine=InnoDB与wsrep_replicate_myisam=OFF（默认）不兼容 。

在群集模式下运行Percona XtraDB群集时，请避免工作量。如果未在所有节点上同步执行，则可能导致节点不一致。ALTER TABLE … IMPORT/EXPORT

#### 所有表必须具有主键。 这样可以确保相同的行以相同的顺序出现在不同的节点上。DELETE没有主键的表不支持该语句。


原文作者： whx@Flora
