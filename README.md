# 整理12种数据库相关资料，mysql，mariaDB，Percona Server，MongoDB，Redis，RocksDB，TiDB，CouchDB，Cassandra，TokuDB，MemDB，Oceanbase

<br/>
<br/>

* [👀 数据库分类](#nav_sec1)
  * [数据库分类对比](#nav_sec1_chapter1)
  * [ACID规则](#nav_sec1_chapter2)
  * [CAP原理](#nav_sec1_chapter3)
* [🦈 关系型数据库](#nav_sec2)
  * [MySQL](#nav_sec2_chapter1)
  * [MariaDB](#nav_sec2_chapter2)
  * [Percona Server](#nav_sec2_chapter3)
* [🦉 NoSQL数据库](#nav_sec3)
  * [键值(Key-Value)存储数据库](#nav_sec3_chapter1)
    * [Redis](#nav_sec3_chapter1_01)
    * [RocksDB](#nav_sec3_chapter1_01)
  * [列存储数据库](#nav_sec3_chapter2)
    * [Cassandra](#nav_sec3_chapter2_01)
    * [HBase](#nav_sec3_chapter2_02)
  * [文档型数据库](#nav_sec3_chapter3)
    * [CouchDB](#nav_sec3_chapter3_01)
    * [MongoDb](#nav_sec3_chapter3_02)
    * [SequoiaDB](#nav_sec3_chapter3_03)
  * [图形(Graph)数据库](#nav_sec3_chapter4)
    * [Neo4J](#nav_sec3_chapter4_01)
    * [Infinite Graph](#nav_sec3_chapter4_02)
* [🦊 NewSQL数据库](#nav_sec4)
  * [新架构](#nav_sec4_chapter1)
    * [Google Spanner](#nav_sec4_chapter1_01)
    * [VoltDB](#nav_sec4_chapter1_01)
  * [SQL引擎](#nav_sec4_chapter2)
    * [TokuDB](#nav_sec4_chapter2_01)
  * [透明分片](#nav_sec4_chapter3)
    * [ScaleBase](#nav_sec4_chapter3_01)
  * [分布式数据库](#nav_sec4_chapter4)
    * [TiDB](#nav_sec4_chapter4_01)
    * [Oceanbase](#nav_sec4_chapter4_02)
    * [MemDB](#nav_sec4_chapter4_03)

<br/>
<br/>
<br/>


# <h1 id="nav_sec1">👀 数据库分类</h1>

## <h2 id="nav_sec1_chapter1">数据库分类对比</h2>

英文名              |中文名       |定义                                     |存储方式                   | ACID规则支持情况    | CAP原理支持情况
:------------------|:------------|:----------------------------------------|:-------------------------|:-------------------|:------------
Relational database|关系型数据库  |采用了关系模型来组织数据的数据库            |表格                      |支持ACID规则         |满足CP,但A不完美
NoSQL              |非关系型数据库|泛指非关系型的数据库,不保证关系数据的ACID特性|键值存储、列存储、文档存储等|不一定完全支持ACID规则|满足AP,但C不完美
NewSQL             |NewSQL       |NewSQL是对各种新的可扩展/高性能数据库的简称 |多种存储方式               |支持ACID规则         |满足CAP

## <h2 id="nav_sec1_chapter2">ACID规则</h2>
* 原子性（A）
一个事务的所有系列操作步骤被看成一个动作，所有的步骤要么全部完成，要么一个也不会完成。如果在事务过程中发生错误，则会回滚到事务开始前的状态，将要被改变的数据库记录不会被改变。
* 一致性（C）
一致性是指在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏，即数据库事务不能破坏关系数据的完整性及业务逻辑上的一致性。
* 隔离性（I）
主要用于实现并发控制，隔离能够确保并发执行的事务按顺序一个接一个地执行。通过隔离，一个未完成事务不会影响另外一个未完成事务。
* 持久性（D）
一旦一个事务被提交，它应该持久保存，不会因为与其他操作冲突而取消这个事务。

## <h2 id="nav_sec1_chapter3">CAP原理</h2>

* Consistency(一致性)： 数据一致更新，所有数据变动都是同步的
* Availability(可用性)： 好的响应性能
* Partition tolerance(分区耐受性)： 可靠性

举例来说在高可用的网站架构中，对于数据基础提出了以下的要求：
* 分区耐受性
保证数据可持久存储，在各种情况下都不会出现数据丢失的问题。为了实现数据的持久性，不但需要在写入的时候保证数据能够持久存储，还需要能够将数据备份一个或多个副本，存放在不同的物理设备上，防止某个存储设备发生故障时，数据不会丢失。
* 数据一致性
在数据有多份副本的情况下，如果网络、服务器、软件出现了故障，会导致部分副本写入失败。这就造成了多个副本之间的数据不一致，数据内容冲突。
* 数据可用性
多个副本分别存储于不同的物理设备的情况下，如果某个设备损坏，就需要从另一个数据存储设备上访问数据。如果这个过程不能很快完成，或者在完成的过程中需要停止终端用户访问数据，那么在切换存储设备的这段时间内，数据就是不可访问的。

<br/>
<br/>

# <h1 id="nav_sec2">🦈 关系型数据库</h1>

关系型数据库，是指采用了关系模型来组织数据的数据库

1. 关系模型指的就是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。
2. 通过SQL结构化查询语句存储数据。
3. 强调ACID规则, 保持数据一致性。

## <h2 id="nav_sec2_chapter1">MySQL</h2>

<div align=center>
  
知识体系|存储引擎|面试题|常见问题|配置文件参数|视频教程|文章|Paper|电子书籍
:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:
[🌴](#nav_sec2_chp_01)|[🔱](#nav_sec2_chp_02)|[⭕](#nav_sec2_chp_03) |[🛠](#nav_sec2_chp_04)|[📜](#nav_sec2_chp_05)|[🧿](#nav_sec2_chp_06)|[📄](#nav_sec2_chp_07)|[🍀](#nav_sec2_chp_08)|[📙](#nav_sec2_chp_09)
 
</div>

### <h3 id="nav_sec2_chp_01">知识体系</h3>

![image](https://user-images.githubusercontent.com/87458342/132217787-7570ad77-fb5a-499d-a0e3-70c69b08e506.png)

### <h3 id="nav_sec2_chp_02">存储引擎</h3>

#### <h4 id="nav_sec2_chp_02">MyISAM引擎</h4>

1. 不支持事务
2. 表级锁定 数据更新时锁定整个表：其锁定机制是表级锁定，也就是对表中的一个数据进行操作都会将这个表锁定，其他人不能操作这个表，这虽然可以让锁定的实现成本很小但是也同时大大降低了其并发性能。
3. 读写互相阻塞 不仅会在写入的时候阻塞读取，MyISAM还会再读取的时候阻塞写入，但读本身并不会阻塞另外的读。
4. 只会缓存索引 MyISAM可以通过key_buffer_size的值来提高缓存索引，以大大提高访问性能减少磁盘IO，但是这个缓存区只会缓存索引，而不会缓存数据。
5. 读取速度较快 占用资源相对较少
6. 不支持外键约束，但只是全文索引
7. MyISAM引擎是MySQL5.5版本之前的默认引擎，是对最初的ISAM引擎优化的产物。

#### <h4 id="nav_sec2_chp_02">InnoDB引擎</h4>

1. 支持事务
2. 行级锁定对高并发有很好的适应能力，但需要确保查询是通过索引完成。
3. 数据更新较为频繁的场景，如：BBS(论坛)、SNS(社交平台)、微博等
4. 数据一致性要求较高的业务，例如：充值转账，银行卡转账。
5. 硬件设备内存较大，可以利用InnoDB较好的缓存能力来提高内存利用率，尽可能减少磁盘IO，可以通过一些参数来设置
6. 相比MyISAM引擎，Innodb引擎更消耗资源，速度没有MyISAM引擎快

#### <h4 id="nav_sec2_chp_02">其他引擎</h4>

* MEMORY存储引擎提供"内存中"表。MERGE存储引擎允许集合将被处理同样的MyISAM表作为一个单独的表。就像MyISAM一样，MEMORY和MERGE存储引擎处理非事务表，这两个引擎也都被默认包含在MySQL中。
注释：MEMORY存储引擎正式地被确定为HEAP引擎。

* EXAMPLE存储引擎是一个"存根"引擎，它不做什么。你可以用这个引擎创建表，但没有数据被存储于其中或从其中检索。这个引擎的目的是服务，在 MySQL源代码中的一个例子，它演示说明如何开始编写新存储引擎。同样，它的主要兴趣是对开发者。

* NDB Cluster是被MySQL Cluster用来实现分割到多台计算机上的表的存储引擎。它在MySQL-Max 5.1二进制分发版里提供。这个存储引擎当前只被Linux, Solaris, 和Mac OS X 支持。在未来的MySQL分发版中，我们想要添加其它平台对这个引擎的支持，包括Windows。

* ARCHIVE存储引擎被用来无索引地，非常小地覆盖存储的大量数据。

* CSV存储引擎把数据以逗号分隔的格式存储在文本文件中。

* BLACKHOLE存储引擎接受但不存储数据，并且检索总是返回一个空集。

* FEDERATED存储引擎把数据存在远程数据库中。在MySQL 5.1中，它只和MySQL一起工作，使用MySQL C Client API。在未来的分发版中，我们想要让它使用其它驱动器或客户端连接方法连接到另外的数据源。

### <h3 id="nav_sec2_chp_03">面试题</h3>

### <h3 id="nav_sec2_chp_04">常见问题</h3>

* [远程客户端连接问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/远程客户端连接问题.md)
* [库表设计问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/库表设计问题.md)
* [慢SQL问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/慢SQL问题.md)

### <h3 id="nav_sec2_chp_05">配置文件参数</h3>

* [mysql5.7配置文件 my.ini 说明]()
* [mysql8.0配置文件 my.ini 说明]()
* Mysql5.7源码地址： [https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz)
* Mysql8.0源码地址： [https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz)

### <h3 id="nav_sec2_chp_06">视频教程</h3>

* [MySQL不了解这些,好意思说搞懂了Mysql ](https://github.com/0voice/backend_video#nav_1_interview_009)
* [性能优化的方法论,异步的原理与实现，mysqlredis,dns, http，服务器并发 ]( https://github.com/0voice/backend_video#nav_1_high_performance_network_020)
* [大厂面试，Mysql必问的问题 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_015)
* [MySQL的块数据操作 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_021 )
* [mysql索引 myisam,innodb，b树b+树 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_022)
* [一节课搞懂 MySQL 索引和事务 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_028)
* [90分析搞懂mysql索引及其优化 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_036 )
* [90分钟搞懂MySQL InnoDB索引以及事务 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_040 )
* [90分钟搞懂mysql缓存问题的解决方案 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_041)
* [大厂如何解决mysql读写效率问题 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_060 )
* [你所需要掌握的MySQL基本原理:索引和事务 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_069 )
* [—节课搞懂MySQL索引和事务 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_080 )
* [—节课详尽讲解提升MySQL读写性能的方案 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_083 )
* [高并发场景下,mysql与redis的数据同步方案 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_143)

### <h3 id="nav_sec2_chp_07">文章</h3>

### <h3 id="nav_sec2_chp_08">Paper</h3>

### <h3 id="nav_sec2_chp_09">电子书籍</h3>

## <h2 id="nav_sec2_chapter2">MariaDB</h2>

## <h2 id="nav_sec2_chapter3">Percona Server</h2>

<br/>
<br/>

# <h1 id="nav_sec3">🦉 NoSQL数据库</h1>

## <h2 id="nav_sec3_chapter1">键值(Key-Value)存储数据库</h2>

### <h3 id="nav_sec3_chapter1_01">Redis</h3>
### <h3 id="nav_sec3_chapter1_01">RocksDB</h3>

## <h2 id="nav_sec3_chapter2">列存储数据库</h2>

### <h3 id="nav_sec3_chapter2_01">Cassandra</h3>
### <h3 id="nav_sec3_chapter2_02">HBase</h3>

## <h2 id="nav_sec3_chapter3">文档型数据库</h2>

### <h3 id="nav_sec3_chapter3_01">CouchDB</h3>
### <h3 id="nav_sec3_chapter3_02">MongoDb</h3>
### <h3 id="nav_sec3_chapter3_03">SequoiaDB</h3>

## <h2 id="nav_sec3_chapter4">图形(Graph)数据库</h2>

### <h3 id="nav_sec3_chapter4_01">Neo4J</h3>
### <h3 id="nav_sec3_chapter4_02">Infinite Graph</h3>

<br/>
<br/>

# <h1 id="nav_sec4">🦊 NewSQL数据库</h1>

## <h2 id="nav_sec4_chapter1">新架构</h2>

### <h3 id="nav_sec4_chapter1_01">Google Spanner</h3>
### <h3 id="nav_sec4_chapter1_02">VoltDB</h3>

## <h2 id="nav_sec4_chapter2">SQL引擎</h2>

### <h3 id="nav_sec4_chapter2_01">TokuDB</h3>

## <h2 id="nav_sec4_chapter3">透明分片</h2>

### <h3 id="nav_sec4_chapter3_01">ScaleBase</h3>

## <h2 id="nav_sec4_chapter4">分布式数据库</h2>

### <h3 id="nav_sec4_chapter4_01">TiDB</h3>
### <h3 id="nav_sec4_chapter4_02">Oceanbase</h3>
### <h3 id="nav_sec4_chapter4_03">MemDB</h3>

