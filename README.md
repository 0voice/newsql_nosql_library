# 💨🏹🌀整理12种数据库相关资料，mysql，mariaDB，Percona Server，MongoDB，Redis，RocksDB，TiDB，CouchDB，Cassandra，TokuDB，MemDB，Oceanbase

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

<!--
  * [表格存储数据库](#nav_sec2_chapter1)
    * [MySQL](#nav_sec2_chapter1_01)
    * [MariaDB](#nav_sec2_chapter1_02)
    * [Percona Server](#nav_sec2_chapter1_03)
-->

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
  
知识体系|存储引擎|面试题|优化与架构|源码与配置参数|视频教程|文章|Paper|电子书籍|常见问题
:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:
[🌴](#nav_sec2_chp_01)|[🔱](#nav_sec2_chp_02)|[⭕](#nav_sec2_chp_03) |[🛠](#nav_sec2_chp_04)|[📜](#nav_sec2_chp_05)|[🧿](#nav_sec2_chp_06)|[📄](#nav_sec2_chp_07)|[🍀](#nav_sec2_chp_08)|[📙](#nav_sec2_chp_09)|[🧲](#nav_sec2_chp_10)
 
</div>

### <h3 id="nav_sec2_chp_01">🌴 知识体系</h3>

#### <h4 id="nav_sec2_chp_01_1">MySQL体系详解</h4>

<!--
![image](https://user-images.githubusercontent.com/87458342/132217787-7570ad77-fb5a-499d-a0e3-70c69b08e506.png){:height="65%" width="65%"}
-->
<div align=center>
 
<img src="https://user-images.githubusercontent.com/87458342/132217787-7570ad77-fb5a-499d-a0e3-70c69b08e506.png" width="85%" height="85%" />
 
</div>

#### <h4 id="nav_sec2_chp_01_2">MySQL架构图</h4>
<!--
![image](https://user-images.githubusercontent.com/87458342/132278098-083884ad-5554-41c9-93ed-9d2f2d131da1.png){:height="50%" width="50%"}
-->
<div align=center>
 
<img src="https://user-images.githubusercontent.com/87458342/132278098-083884ad-5554-41c9-93ed-9d2f2d131da1.png" width="70%" height="70%" />
 
</div>

#### <h4 id="nav_sec2_chp_01_3">MySQL亿级订单数据分库分表设计架构图</h4>

<div align=center>
 
![image](https://user-images.githubusercontent.com/87458342/132295577-336360c2-3445-4fb2-b733-39477f416688.png)

</div>
 
#### <h4 id="nav_sec2_chp_01_3">MySQL亿级订单数据分库分表设计架构图</h4>

<div align=center>

<img src="https://user-images.githubusercontent.com/87458342/132321585-61079a0a-9cb4-4557-a300-873891b2d8f9.png" width="85%" height="85%" />
 
</div>
 
 

### <h3 id="nav_sec2_chp_02">🔱 存储引擎</h3>

#### <h4 id="nav_sec2_chp_02">MyISAM引擎</h4>

##### <h5>MyISAM引擎特点</h5>

1. 不支持事务
2. 表级锁定 数据更新时锁定整个表：其锁定机制是表级锁定，也就是对表中的一个数据进行操作都会将这个表锁定，其他人不能操作这个表，这虽然可以让锁定的实现成本很小但是也同时大大降低了其并发性能。
3. 读写互相阻塞 不仅会在写入的时候阻塞读取，MyISAM还会再读取的时候阻塞写入，但读本身并不会阻塞另外的读。
4. 只会缓存索引 MyISAM可以通过key_buffer_size的值来提高缓存索引，以大大提高访问性能减少磁盘IO，但是这个缓存区只会缓存索引，而不会缓存数据。
5. 读取速度较快 占用资源相对较少
6. 不支持外键约束，但只是全文索引
7. MyISAM引擎是MySQL5.5版本之前的默认引擎，是对最初的ISAM引擎优化的产物。

##### <h5>MyISAM引擎适用的生产业务场景</h5>

1. 不需要事务支持的业务(例如转账就不行，充值也不行)
2. 一般为读数据比较多的应用，读写都频繁场景不适合，读多或者写多的都适合。
3. 读写并发访问都相对较低的业务(纯读纯写高并发也可以)(锁定机制问题)
4. 数据修改相对较少的业务(阻塞问题)
5. 以读为主的业务，例如：blog,图片信息数据库，用户数据库，商品库等业务
6. 对数据一致性要求不是很高的业务。
7. 中小型的网站部分业务会用。
8. 小结：单一对数据库的操作都可以示用MyISAM，所谓单一就是尽量纯读，或纯写(insert,update,delete)等。

##### <h5>MyISAM引擎调优精要</h5>

1. 设置合适的索引(缓存机制)(where、join后面的列建立索引，重复值比较少的建索引等)
2. 调整读写优先级，根据实际需求确保重要操作更优先执行，读写的时候可以通过参数设置优先级。
3. 启用延迟插入改善大批量写入性能(降低写入频率，尽可能多条数据一次性写入)。
4. 尽量顺序操作让insert数据都写入到尾部，较少阻塞。
5. 分解大的操作，降低单个操作的阻塞时间，就像操作系统控制cpu分片一样。
6. 降低并发数(减少对MySQL访问)，某些高并发场景通过应用进行排队队列机制Q队列。
7. 对于相对静态(更改不频繁)的数据库数据，充分利用Query Cache(可以通过配置文件配置)或memcached缓存服务可以极大的提高访问频率。
8. MyISAM的Count只有在全表扫描的时候特别高效，带有其他条件的count都需要进行实际的数据访问。

#### <h4 id="nav_sec2_chp_02">InnoDB引擎</h4>

##### <h5>InnoDB引擎适用的生产业务场景</h5>

1. 支持事务
2. 行级锁定对高并发有很好的适应能力，但需要确保查询是通过索引完成。
3. 数据更新较为频繁的场景，如：BBS(论坛)、SNS(社交平台)、微博等
4. 数据一致性要求较高的业务，例如：充值转账，银行卡转账。
5. 硬件设备内存较大，可以利用InnoDB较好的缓存能力来提高内存利用率，尽可能减少磁盘IO，可以通过一些参数来设置
6. 相比MyISAM引擎，Innodb引擎更消耗资源，速度没有MyISAM引擎快

##### <h5>InnoDB引擎调优精要

1. 主键尽可能小，避免给Secondery index带来过大的空间负担。
2. 避免全表扫描，因为会使用表锁。
3. 尽可能缓存所有的索引和数据，提高响应速度，较少磁盘IO消耗。
4. 在大批量小插入的时候，尽量自己控制事务而不要使用autocommit自动提交，有开关可以控制提交方式。
5. 合理设置innodb_flush_log_at_trx_commit参数值，不要过度追求安全性。 如果innodb_flush_log_at_trx_commit的值为0，log buffer每秒就会被刷写日志文件到磁盘，提交事务的时候不做任何操作。
6. 避免主键更新，因为这会带来大量的数据移动。

#### <h4 id="nav_sec2_chp_02">其他引擎</h4>

* MEMORY存储引擎提供"内存中"表。MERGE存储引擎允许集合将被处理同样的MyISAM表作为一个单独的表。就像MyISAM一样，MEMORY和MERGE存储引擎处理非事务表，这两个引擎也都被默认包含在MySQL中。
注释：MEMORY存储引擎正式地被确定为HEAP引擎。

* EXAMPLE存储引擎是一个"存根"引擎，它不做什么。你可以用这个引擎创建表，但没有数据被存储于其中或从其中检索。这个引擎的目的是服务，在 MySQL源代码中的一个例子，它演示说明如何开始编写新存储引擎。同样，它的主要兴趣是对开发者。

* NDB Cluster是被MySQL Cluster用来实现分割到多台计算机上的表的存储引擎。它在MySQL-Max 5.1二进制分发版里提供。这个存储引擎当前只被Linux, Solaris, 和Mac OS X 支持。在未来的MySQL分发版中，我们想要添加其它平台对这个引擎的支持，包括Windows。

* ARCHIVE存储引擎被用来无索引地，非常小地覆盖存储的大量数据。

* CSV存储引擎把数据以逗号分隔的格式存储在文本文件中。

* BLACKHOLE存储引擎接受但不存储数据，并且检索总是返回一个空集。

* FEDERATED存储引擎把数据存在远程数据库中。在MySQL 5.1中，它只和MySQL一起工作，使用MySQL C Client API。在未来的分发版中，我们想要让它使用其它驱动器或客户端连接方法连接到另外的数据源。

### <h3 id="nav_sec2_chp_03">⭕ 面试题</h3>

* [1. 能说下myisam 和 innodb的区别吗？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_001)
* [2. 说下mysql的索引有哪些吧，聚簇和非聚簇索引又是什么？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_002)
* [3. 那你知道什么是覆盖索引和回表吗？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_003)
* [4. 锁的类型有哪些呢?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_004)
* [5. 你能说下事务的基本特性和隔离级别吗？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_005)
* [6. 那ACID靠什么保证的呢？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_006)
* [7. 那你说说什么是幻读，什么是MVCC？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_007)
* [8. 那你知道什么是间隙锁吗？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_008)
* [9. 你们数据量级多大？分库分表怎么做的？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_009)
* [10. 那分表后的ID怎么保证唯一性的呢？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_010)
* [11. 分表后非sharding_key的查询怎么处理呢？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_011)
* [12. 说说mysql主从同步怎么做的吧？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_012)
* [13. 那主从的延迟怎么解决呢？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_013)

* [14. 为什么用自增列作为主键?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_014)
* [15. 为什么使用数据索引能提高效率?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_015)
* [16. B+树索引和哈希索引的区别?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_016)
* [17. 哈希索引的优势?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_017)
* [18. 哈希索引不适用的场景?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_018)
* [19. B树和B+树的区别?](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_019)
* [20. 为什么说B+比B树更适合实际应用中操作系统的文件索引和数据库索引？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_020)
* [21. MySQL联合索引？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_021)
* [22. 什么情况下应不建或少建索引？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_022)
* [23. 什么是表分区？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_023)
* [24. 表分区与分表的区别？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_024)
* [25. 表分区有什么好处？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_025)
* [26. 分区表的限制因素？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_026)
* [27. 如何判断当前MySQL是否支持分区？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_027)
* [28. MySQL支持的分区类型有哪些？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_028)
* [29. 四种隔离级别？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_029)
* [30. 关于MVVC的运行原理介绍？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_030)
* [31. 在MVCC并发控制中，读操作可以分成两类？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_031)
* [32. 行级锁定的优点有哪些？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_032)
* [33. 行级锁定的缺点有哪些？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_033)
* [34. 你对MySQL优化了解多少？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_034)
* [35. key和index的区别？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_035)
* [36. Mysql 中 MyISAM 和 InnoDB 的区别有哪些？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_036)
* [37. 数据库表创建注意事项？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_037)
* [38. MyISAM和InnoDB存储引擎使用的锁？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_038)
* [39. 数据库一个大表如何优化？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_039)
* [40. 如何理解字符集及校对规则？](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/面试题/面试题_01.md#subject_040)
 
### <h3 id="nav_sec2_chp_04">🛠 优化与架构</h3>
 
* [MySQL体系结构和组件](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/优化与架构/MySQL体系结构和组件.md)
* [MySQL架构体系](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/优化与架构/MySQL架构体系.md)
 
### <h3 id="nav_sec2_chp_05">📜 源码与配置参数</h3>

* MySQL5.7源码地址： [https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.18.tar.gz](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.18.tar.gz)
* MySQL8.0源码地址： [https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz)
* [MySQL源码文件结构及主要数据结构](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/源码与配置参数/MySQL源码文件结构及主要数据结构.md)
* [MySQL5.7配置文件 my.ini 说明](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/源码与配置参数/MySQL5.7配置文件%20my.ini%20说明.md)
* [MySQL8.0配置文件 my.ini 说明](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/源码与配置参数/MySQL8.0配置文件%20my.ini%20说明.md)
 
### <h3 id="nav_sec2_chp_06">🧿 视频教程</h3>

* [MySQL不了解这些,好意思说搞懂了MySQL](https://github.com/0voice/backend_video#nav_1_interview_009)
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

### <h3 id="nav_sec2_chp_07">📄 文章</h3>

No.|Article|Author
:------- |:--------------- |:---------------
1|[MySQL远程客户端连接问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/远程客户端连接问题.md)|milo
2|[亿级订单数据分库分表设计方案](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/亿级订单数据分库分表设计方案.md)|鑫语人间
3|[数据库扩展性架构设计](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/数据库扩展性架构设计.md)|
4|[分库分表需要考虑的问题及方案](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/分库分表需要考虑的问题及方案.md)|
5|[无限容量数据库架构设计](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/无限容量数据库架构设计.md)|
6|[MQ消息可达性+幂等性+延时性架构设计](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/MQ消息可达性+幂等性+延时性架构设计.md)|
7|[高可用+高并发+负载均衡架构设计](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/高可用+高并发+负载均衡架构设计.md)|
8|[数据库秒级平滑扩容架构方案](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/数据库秒级平滑扩容架构方案.md)|
9|[100亿数据平滑数据迁移,不影响服务](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/100亿数据平滑数据迁移,不影响服务.md)|
10|[一分钟掌握数据库垂直拆分](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/一分钟掌握数据库垂直拆分.md)|
11|[5kw数据量,如何为表增加一列](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/5kw数据量,如何为表增加一列.md)|
12|[互联网在线表结构变更](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/互联网在线表结构变更.md)|
13|[58同城,1万属性100亿数据量数据库架构设计](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/文章/58同城,1万属性100亿数据量数据库架构设计.md)|
 
### <h3 id="nav_sec2_chp_08">🍀 Paper</h3>

No.|Title|Translate|Company
:-------: | :--------------- | :------------|:---------------
1|[《MySQL V5 - Ready forPrime Time Business Intelligence》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/01_MySQL_Primetime_Business_Intelligence.pdf)|MySQL V5 - 为黄金时段商业智能做好准备|
2|[《TDSQL for MySQL Security White Paper Product Documentation》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/02_TDSQL_for_MySQL_Security_White_Paper_Product_Documentation.pdf)|TDSQL for MySQL安全白皮书产品文档 |
3|[《MONGODB VS MYSQL: A COMPARATIVE STUDY OF PERFORMANCE IN SUPER MARKET MANAGEMENT SYSTEM》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/03_MONGODB_VS_MYSQL.pdf)| MONGODB与MYSQL：超市管理系统性能的比较研究| 
4|[《Optimizing MySQL Database System on Information Systems Research , Publications and Community Service》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/04_Optimizing_MySQL_Database_System_on_Information_Systems_Research_Publications_and_Community_Service.pdf)| 在信息系统研究、出版物和社区服务方面优化MySQL数据库系统|
5|[《A Comparative Study: MongoDB vs MySQL》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/05_A-Comparative-Study-MongoDB-vs-MySQL.pdf)|MongoDB与MySQL的比较研究 |
6|[《Best Practices for Migrating MySQL Databases to Amazon Aurora》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/06_Best-Practices-for-Migrating-MySQL-Databases-to-Amazon-Aurora.pdf)|将MySQL数据库迁移到Amazon Aurora的最佳实践 |
7|[《Comparative Performance Analysis of MySQL and SQL Server Relational Database Management Systems in Windows Environment》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/07_Comparative_Performance_Analysis_of_MySQL_and_SQL_Server_Relational_Database_Management_Systems_in_Windows_Environment.pdf)|Windows环境下MySQL与sqlserver关系数据库管理系统性能比较分析 |
8|[《MySQL Cluster Internal Architecture》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/08_MySQL_Cluster_Internal_Architecture_MariaDB_White_Paper.pdf)|MySQL集群内部架构 |
9|[《MySQL Database Service with HeatWave》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/09_mysql-database-service-technical-paper.pdf)|MySQL数据库服务与HeatWave |
10|[《Amazon Aurora: Design Considerations for High Throughput Cloud-Native Relational Databases》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/10_Design_Considerations_for_High_Throughput_Cloud-Native_Relational_Databases.pdf)|Amazon Aurora：高通量云本地关系数据库的设计考虑 |
11|[《Recovery Principles of MySQL Cluster 5.1》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/11_Recovery_Principles_of_MySQL_Cluster.pdf)|MySQL集群的恢复原理 |

### <h3 id="nav_sec2_chp_09">📙 电子书籍</h3>

书籍|翻译       
:-----|:----------
[《MySQL 5.7 Reference Manual》](https://docs.oracle.com/cd/E17952_01/mysql-5.7-en/)|《MySQL 5.7参考手册》
[《MySQL 8.0 Reference Manual》](https://docs.oracle.com/cd/E17952_01/mysql-8.0-en/)|《MySQL 8.0参考手册》
[《MySQL》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/MySQL.pdf)|《MySQL》
[《MySQL Notes For Professionals》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/MySQL_Notes_For_Professionals.pdf)|《MySQL专业指南》
[《Intrusion Detection with SNORT: Using SNORT, Apache, MySQL, PHP, and ACID》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/Intrusion_Detection_with_SNORT_Using_SNORT_Apache_MySQL_PHP_and_ACID.pdf)|《Snort入侵检测：使用Snort、Apache、MySQL、PHP和ACID》
[《MySQL 从入门到精通》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/Mysql%20V2.3最终版.pdf)|《MySQL 从入门到精通》
[《MySQL Workbench教程》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/MySQL%20workbench教程.pdf)|《MySQL Workbench教程》
[《高性能MySQL_中文版》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/高性能MySQL_英文版.pdf)|《高性能MySQL(中文版)》
[《高性能MySQL(英文版)》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/高性能MySQL_英文版.pdf)|《高性能MySQL(英文版)》
[《MySQL技术内幕：SQL编程》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/高性能MySQL_英文版.pdf)|《MySQL技术内幕：SQL编程》
[《MySQL技术内幕：InnoDB存储引擎 第二版》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/电子书/高性能MySQL_英文版.pdf)|《MySQL技术内幕：InnoDB存储引擎 第二版》
 
### <h3 id="nav_sec2_chp_10">🧲 常见问题</h3>

* [远程客户端连接问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/远程客户端连接问题.md)
* [库表设计问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/库表设计问题.md)
* [慢SQL问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/慢SQL问题.md)

<br/>
 
## <h2 id="nav_sec2_chapter2">MariaDB</h2>
 
<br/>
 
## <h2 id="nav_sec2_chapter3">Percona Server</h2>

<br/>
<br/>

# <h1 id="nav_sec3">🦉 NoSQL数据库</h1>

## <h2 id="nav_sec3_chapter1">键值(Key-Value)存储数据库</h2>

* 相关产品： Redis、RocksDB
* 典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
* 数据模型： 一系列键值对
* 优势： 快速查询
* 劣势： 存储的数据缺少结构化

### <h3 id="nav_sec3_chapter1_01">Redis</h3>
### <h3 id="nav_sec3_chapter1_01">RocksDB</h3>

## <h2 id="nav_sec3_chapter2">列存储数据库</h2>

* 相关产品：Cassandra, HBase
* 典型应用：分布式的文件系统
* 数据模型：以列簇式存储，将同一列数据存在一起
* 优势：查找速度快，可扩展性强，更容易进行分布式扩展
* 劣势：功能相对局限
                    
### <h3 id="nav_sec3_chapter2_01">Cassandra</h3>
### <h3 id="nav_sec3_chapter2_02">HBase</h3>

## <h2 id="nav_sec3_chapter3">文档型数据库</h2>

* 相关产品：CouchDB、MongoDB、SequoiaDB
* 典型应用：Web应用（与Key-Value类似，Value是结构化的）
* 数据模型： 一系列键值对
* 优势：数据结构要求不严格
* 劣势： 查询性能不高，而且缺乏统一的查询语法

### <h3 id="nav_sec3_chapter3_01">CouchDB</h3>
### <h3 id="nav_sec3_chapter3_02">MongoDb</h3>
### <h3 id="nav_sec3_chapter3_03">SequoiaDB</h3>
 
#### 产品架构
 
![image](https://user-images.githubusercontent.com/87458342/132276817-dced5957-6d83-4439-be36-d44f2fe07003.png)


## <h2 id="nav_sec3_chapter4">图形(Graph)数据库</h2>

* 相关数据库：Neo4J、Infinite Graph
* 典型应用：社交网络
* 数据模型：图结构
* 优势：利用图结构相关算法。
* 劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

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

