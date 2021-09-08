# 💨🏹🌀 2021年，史上最强的数据库资料集合 <br/> 12种数据库的全方位整理：mysql，mariaDB，Percona Server，MongoDB，Redis，RocksDB，TiDB，CouchDB，Cassandra，TokuDB，MemDB，Oceanbase


<br/>
<br/>

* [👀 数据库分类](#nav_sec1)
  * [数据库分类对比](#nav_sec1_chapter1)
  * [ACID规则](#nav_sec1_chapter2)
  * [CAP原理](#nav_sec1_chapter3)
* [🦩 关系型数据库](#nav_sec2)
  * [🦈 MySQL](#nav_sec2_chapter1)
  * [🐬 MariaDB](#nav_sec2_chapter2)
  * [🐋 Percona Server](#nav_sec2_chapter3)
* [🦉 NoSQL数据库](#nav_sec3)
  * [键值(Key-Value)存储数据库](#nav_sec3_chapter1)
    * [🐝 Redis](#nav_sec3_chapter1_01)
    * [🦗 RocksDB](#nav_sec3_chapter1_02)
  * [列存储数据库](#nav_sec3_chapter2)
    * [🦕 Cassandra](#nav_sec3_chapter2_01)
  * [文档型数据库](#nav_sec3_chapter3)
    * [🦜 CouchDB](#nav_sec3_chapter3_01)
    * [🦢 MongoDb](#nav_sec3_chapter3_02)
* [🦊 NewSQL数据库](#nav_sec4)
  * [SQL引擎](#nav_sec4_chapter2)
    * [🦅 TokuDB](#nav_sec4_chapter2_01)
  * [分布式数据库](#nav_sec4_chapter4)
    * [🐪 TiDB](#nav_sec4_chapter4_01)
    * [🐫 Oceanbase](#nav_sec4_chapter4_02)
    * [🐐 MemDB](#nav_sec4_chapter4_03)

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
 
# <h1 id="nav_sec2">🦩 关系型数据库</h1>

关系型数据库，是指采用了关系模型来组织数据的数据库

1. 关系模型指的就是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。
2. 通过SQL结构化查询语句存储数据。
3. 强调ACID规则, 保持数据一致性。

## <h2 id="nav_sec2_chapter1">🦈 MySQL</h2>

<div align=center>
  
知识体系|存储引擎|面试题|优化与集群架构|源码与配置参数|视频资源|文章|Paper|电子书籍|常见问题
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
<br/>
MySQL体系详解
 
</div>

#### <h4 id="nav_sec2_chp_01_2">MySQL架构图</h4>
<!--
![image](https://user-images.githubusercontent.com/87458342/132278098-083884ad-5554-41c9-93ed-9d2f2d131da1.png){:height="50%" width="50%"}
-->
<div align=center>
 
<img src="https://user-images.githubusercontent.com/87458342/132278098-083884ad-5554-41c9-93ed-9d2f2d131da1.png" width="70%" height="70%" />
<br/>
MySQL架构图
 
</div>

#### <h4 id="nav_sec2_chp_01_3">MySQL亿级订单数据分库分表设计架构图</h4>

<div align=center>
 
![image](https://user-images.githubusercontent.com/87458342/132295577-336360c2-3445-4fb2-b733-39477f416688.png)
<br/>
MySQL亿级订单数据分库分表设计架构图（鑫语人间）

</div>
 
#### <h4 id="nav_sec2_chp_01_4">MySQL亿级流量系统设计每秒十万查询的高并发架构图</h4>

<div align=center>

<img src="https://user-images.githubusercontent.com/87458342/132321585-61079a0a-9cb4-4557-a300-873891b2d8f9.png" width="85%" height="85%" />
<br/>
MySQL亿级流量系统设计每秒十万查询的高并发架构图（清幽梅）
 
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
 
### <h3 id="nav_sec2_chp_04">🛠 优化与集群架构</h3>
 
* [MySQL体系结构和组件](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/优化与架构/MySQL体系结构和组件.md)
* [MySQL架构体系](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/优化与架构/MySQL架构体系.md)
 
### <h3 id="nav_sec2_chp_05">📜 源码与配置参数</h3>

* MySQL5.7源码地址： [https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.18.tar.gz](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.18.tar.gz)
* MySQL8.0源码地址： [https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19.tar.gz)
* [MySQL源码文件结构及主要数据结构](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/源码与配置参数/MySQL源码文件结构及主要数据结构.md)
* [MySQL5.7配置文件 my.ini 说明](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/源码与配置参数/MySQL5.7配置文件%20my.ini%20说明.md)
* [MySQL8.0配置文件 my.ini 说明](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/源码与配置参数/MySQL8.0配置文件%20my.ini%20说明.md)
 
### <h3 id="nav_sec2_chp_06">🧿 视频资源</h3>

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
 
#### <h4 id="nav_sec2_chp_07_2">📙 电子书籍</h4>
 
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
 
### <h3 id="nav_sec2_chp_08">🍀 Paper</h3>

No.|Title|Translate|Company
:-------: | :--------------- | :------------|:---------------
1|[《MySQL V5 - Ready forPrime Time Business Intelligence》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/01_MySQL_Primetime_Business_Intelligence.pdf) |MySQL V5 - 为黄金时段商业智能做好准备|
2|[《TDSQL for MySQL Security White Paper Product Documentation》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/02_TDSQL_for_MySQL_Security_White_Paper_Product_Documentation.pdf) |TDSQL for MySQL安全白皮书产品文档 |
3|[《MONGODB VS MYSQL: A COMPARATIVE STUDY OF PERFORMANCE IN SUPER MARKET MANAGEMENT SYSTEM》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/03_MONGODB_VS_MYSQL.pdf) | MONGODB与MYSQL：超市管理系统性能的比较研究| 
4|[《Optimizing MySQL Database System on Information Systems Research , Publications and Community Service》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/04_Optimizing_MySQL_Database_System_on_Information_Systems_Research_Publications_and_Community_Service.pdf) | 在信息系统研究、出版物和社区服务方面优化MySQL数据库系统|
5|[《A Comparative Study: MongoDB vs MySQL》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/05_A-Comparative-Study-MongoDB-vs-MySQL.pdf) |MongoDB与MySQL的比较研究 |
6|[《Best Practices for Migrating MySQL Databases to Amazon Aurora》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/06_Best-Practices-for-Migrating-MySQL-Databases-to-Amazon-Aurora.pdf) |将MySQL数据库迁移到Amazon Aurora的最佳实践 |
7|[《Comparative Performance Analysis of MySQL and SQL Server Relational Database Management Systems in Windows Environment》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/07_Comparative_Performance_Analysis_of_MySQL_and_SQL_Server_Relational_Database_Management_Systems_in_Windows_Environment.pdf) |Windows环境下MySQL与sqlserver关系数据库管理系统性能比较分析 |
8|[《MySQL Cluster Internal Architecture》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/08_MySQL_Cluster_Internal_Architecture_MariaDB_White_Paper.pdf) |MySQL集群内部架构 |
9|[《MySQL Database Service with HeatWave》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/09_mysql-database-service-technical-paper.pdf) |MySQL数据库服务与HeatWave |
10|[《Amazon Aurora: Design Considerations for High Throughput Cloud-Native Relational Databases》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/10_Design_Considerations_for_High_Throughput_Cloud-Native_Relational_Databases.pdf) |Amazon Aurora：高通量云本地关系数据库的设计考虑 |
11|[《Recovery Principles of MySQL Cluster 5.1》](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/paper/11_Recovery_Principles_of_MySQL_Cluster.pdf) |MySQL集群的恢复原理 |

### <h3 id="nav_sec2_chp_09">⚒ 集群架构</h3>


### <h3 id="nav_sec2_chp_10">🧲 常见问题</h3>

* [远程客户端连接问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/远程客户端连接问题.md)
* [库表设计问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/库表设计问题.md)
* [慢SQL问题](https://github.com/0voice/newsql_nosql_library/blob/main/mysql/常见问题/慢SQL问题.md)

<br/>
 
## <h2 id="nav_sec2_chapter2">🐬 MariaDB</h2>

MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可 MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。<br/>
在存储引擎方面，使用XtraDB来代替MySQL的InnoDB。<br/>
MariaDB基于事务的Maria存储引擎，替换了MySQL的MyISAM存储引擎。<br/>
MariaDB直到5.5版本，均依照MySQL的版本。<br/>

### MariaDB项目
* [MariaDB项目地址](https://github.com/MariaDB)。
* [MySQ迁移到MariaDB](https://github.com/0voice/newsql_nosql_library/blob/main/MariaDB/MySQ迁移到MariaDB.md)。
* [MariaDB优势](https://github.com/0voice/newsql_nosql_library/blob/main/MariaDB/MariaDB优势.md)。
* [MariaDB是MySQL的二进制替代品](https://github.com/0voice/newsql_nosql_library/blob/main/MariaDB/MariaDB是MySQL的二进制替代品.md)

### MariaDB与MySQL比较
* [MariaDB与MySQL特性比较](https://mariadb.com/kb/en/mariadb-vs-mysql-features/)
* [MariaDB与MySQL兼容性比较](https://mariadb.com/kb/en/mariadb-vs-mysql-compatibility/)
* [MariaDB各个发行版的异同](https://mariadb.com/kb/en/mariadb-releases/)
* [MariaDB发行注记](https://mariadb.com/kb/en/release-notes/)

### MariaDB第三方工具
* [DBEdit](http://dbedit2.sourceforge.net/) 一个免费的MariaDB数据库和其他数据库管理应用程序。
* [Navicat](https://www.navicat.com.cn/) 一系列Windows、Mac OS X、Linux下专有数据库管理应用程序。
* [HeidiSQL](https://www.heidisql.com/) 一个Windows上自由和开放源码的MySQL客户端。它支持MariaDB的5.2.7版本和以后的版本。[5][6]
* [phpMyAdmin](https://www.phpmyadmin.net/) 一个基于网络的MySQL数据库管理应用程序
 
<br/>
 
## <h2 id="nav_sec2_chapter3">🐋 Percona Server</h2>

### Percona Server项目
Percona Server由领先的MySQL咨询公司Percona发布。<br/>
Percona Server是一款独立的数据库产品，其可以完全与MySQL兼容，可以在不更改代码的情况了下将存储引擎更换成XtraDB。是最接近官方MySQL Enterprise发行版的版本。<br/>
Percona提供了高性能XtraDB引擎，还提供PXC高可用解决方案，并且附带了percona-toolkit等DBA管理工具箱。<br/>
Percona Server 只包含 MySQL 的服务器版，并没有提供相应对 MySQL 的 Connector 和 GUI 工具进行改进。<br/>
Percona Server 使用了一些 google-mysql-tools, Proven Scaling, Open Query 对 MySQL 进行改造。<br/>
 * [Percona Server项目地址](https://github.com/percona/percona-server)
 * [Percona XtraDB群集](https://github.com/0voice/newsql_nosql_library/blob/main/Percona_Server/Percona_XtraDB群集.md)
 * [Percona XtraDB Cluster安装](https://www.percona.com/doc/percona-xtradb-cluster/LATEST/install/yum.html#yum)
 * [Galera Cluster](https://github.com/0voice/newsql_nosql_library/blob/main/Percona_Server/Galera_Cluster.md)
 
### MySQL，MariaDB，Percona Server如何选择
综合多年使用经验和性能对比，首选Percona分支，其次是MariaDB，如果你不想冒一点风险，那就选择MYSQL官方版本。
 
<br/>
<br/>

# <h1 id="nav_sec3">🦉 NoSQL数据库</h1>

## <h2 id="nav_sec3_chapter1">键值(Key-Value)存储数据库</h2>

* 相关产品： Redis、RocksDB
* 典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
* 数据模型： 一系列键值对
* 优势： 快速查询
* 劣势： 存储的数据缺少结构化

### <h3 id="nav_sec3_chapter1_01">🐝 Redis</h3>

<div align=center>
  
知识体系|数据类型|面试题|优化与集群架构|源码与配置参数|视频资源|文章|Paper|电子书籍|常见问题
:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:
[🌴](#nav_sec3_chp_01)|[♨](#nav_sec3_chp_02)|[⭕](#nav_sec3_chp_03) |[🛠](#nav_sec3_chp_04)|[📜](#nav_sec3_chp_05)|[🧿](#nav_sec3_chp_06)|[📄](#nav_sec3_chp_07)|[🍀](#nav_sec3_chp_08)|[📙](#nav_sec3_chp_09)|[🧲](#nav_sec3_chp_10)

</div>
 
### <h3 id="nav_sec3_chp_01">🌴 知识体系</h3>
 
#### <h4 id="nav_sec3_chp_01_1">Redis知识体系图</h4>

<div align=center>
 
<img src="https://user-images.githubusercontent.com/87458342/132483897-2fe31078-529a-43ec-9684-59b5ad484275.png" width="85%" height="85%" />
<br/>
Redis知识体系图

</div>

#### <h4 id="nav_sec3_chp_01_2">Redis Cluster方案图</h4>
<div align=center>

![image](https://user-images.githubusercontent.com/87458342/132462677-6866a14c-cb5f-4c43-9798-644d342632f2.png)
<br/>
官方Redis Cluster 方案(服务端路由查询)
 
</div>

#### <h4 id="nav_sec3_chp_01_3">Redis集群方案（单副本）</h4>
<div align=center>

<img src="https://user-images.githubusercontent.com/87458342/132483021-f21ed53c-cb8c-470a-97cc-e5a2b8535f34.png" width="80%" height="80%" />
<br/>
Redis集群方案（单副本）

</div>

 
### <h3 id="nav_sec3_chp_02">♨ 数据类型</h3>
 

 
### <h3 id="nav_sec3_chp_03">⭕ 面试题</h3>
 
* [01. Redis相比memcached有哪些优势？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_001)
* [02. Redis支持哪几种数据类型？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_002)
* [03. Redis主要消耗什么物理资源?](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_003)
* [04. Redis的全称是什么？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_004)
* [05. Redis有哪几种数据淘汰策略？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_005)
* [06. Redis官方为什么不提供Windows版本？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_006)
* [07. 一个字符串类型的值能存储最大容量是多少？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_007)
* [08. 为什么Redis需要把所有数据放到内存中？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_008)
* [09. Redis集群方案应该怎么做？都有哪些方案？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_009)
* [10. Redis集群方案什么情况下会导致整个集群不可用？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_010)
* [11. MySQL里有2000w数据，redis中只存20w的数据，如何保证redis中的数据都是热点数据？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_011)
* [12. Redis有哪些适合的场景？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_012)
* [13. Redis支持的Java客户端都有哪些？官方推荐用哪个？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_013)
* [14. Redis和Redisson有什么关系？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_014)
* [15. Jedis与Redisson对比有什么优缺点？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_015)
* [16. Redis如何设置密码及验证密码？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_016)
* [17. 说说Redis哈希槽的概念？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_017)
* [18. Redis集群的主从复制模型是怎样的？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_018)
* [19. Redis集群会有写操作丢失吗？为什么？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_019)
* [20. Redis集群之间是如何复制的？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_020)
* [21. Redis集群最大节点个数是多少？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_021)
* [22. Redis集群如何选择数据库？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_022)
* [23. 怎么测试Redis的连通性？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_023)
* [24. Redis中的管道有什么用？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_024)
* [25. 怎么理解Redis事务？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_025)
* [26. Redis事务相关的命令有哪几个？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_026)
* [27. Redis事务相关的命令有哪几个？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_027)
* [28. ](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_028)
* [29. Redis如何做内存优化？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_029)
* [30. Redis回收进程如何工作的？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_030)
* [31. Redis回收使用的是什么算法？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_032)
* [32. Redis如何做大量数据插入？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_032)
* [33. 为什么要做Redis分区？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_033)
* [34. 你知道有哪些Redis分区实现方案？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_034)
* [35. Redis分区有什么缺点？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_035)
* [36. Redis持久化数据和缓存怎么做扩容？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_036)
* [37. 分布式Redis是前期做还是后期规模上来了再做好？为什么？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_037)
* [38. Twemproxy是什么？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_038)
* [39. 支持一致性哈希的客户端有哪些？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_039)
* [40. Redis与其他key-value存储有什么不同？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_040)
* [41. Redis的内存占用情况怎么样？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_041)
* [42. 都有哪些办法可以降低Redis的内存使用情况呢？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_042)
* [43. 一个Redis实例最多能存放多少的keys？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_043)
* [44. ](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_044)
* [45. ](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_045)
* [46. 一个Redis实例最多能存放多少的keys？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_046)
* [47. Redis常见性能问题和解决方案？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_047)
* [48. Redis提供了哪几种持久化方式？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_048)
* [49. 如何选择合适的持久化方式？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_049)
* [50. 修改配置不重启Redis会实时生效吗？](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/面试题/面试题_01.md#subject_050)
 
### <h3 id="nav_sec3_chp_04">🛠 优化与集群架构</h3>
 
#### <h4 id="nav_sec3_chp_04_1">Redis集群方式</h4>
 
* [Redis主从复制](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/集群架构/Redis主从复制.md)
* [Redis哨兵模式](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/集群架构/Redis哨兵模式.md)
* [Redis分片模式](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/集群架构/Redis分片模式.md)
 
### <h3 id="nav_sec3_chp_05">📜 源码与配置参数</h3>
 
* [Redis-6.2.5.tar.gz](https://download.redis.io/releases/redis-6.2.5.tar.gz)
* [Redis参数解释](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/源码与配置参数/Redis参数解释.md)
 
### <h3 id="nav_sec3_chp_06">🧿 视频资源</h3>

* [Redis有序集合，跳表 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_011)
* [性能优化的方法论,异步的原理与实现，mysqlredis,dns, http，服务器并发 ]( https://github.com/0voice/backend_video#nav_1_high_performance_network_020)
* [epoll的网络模型,从redis, memcached到nginx ]( https://github.com/0voice/backend_video#nav_1_high_performance_network_025)
* [如何在开发中使用redis ]( https://github.com/0voice/backend_video#nav_1_middleware_development_002)
* [Redis训练营一 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_007)
* [Redis训练营二 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_008)
* [Redis实战场景分析 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_009)
* [Redis使用场景实战 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_010)
* [Redis主从复制 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_012)
* [—起实现Redis驱动与Redis协议深度剖析 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_013)
* [网络服务器模型, Redis, Memcached, Nginx对比 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_020)
* [Redis, Nginx以及Skynet源码分析探究 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_023)
* [TCP网络服务模型,Redis, Nginx，Memcached一起搞定 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_024)
* [10年大厂程序员是如何学习使用Redis ]( https://github.com/0voice/backend_video#nav_1_middleware_development_030)
* [秃顶大佬程序员如何制定Redis学习路线 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_035 )
* [90分钟搞定Redis面试 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_037 )
* [90分钟搞懂Redis存储结构 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_043 )
* [Redis,Skynet,Nginx,Memcached网络模块对比分析 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_051 )
* [Redis的rehash，布隆过滤器，redis持久化一节课搞定 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_052 )
* [Redis如何实现分布式锁延时队列以及限流应用 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_053 )
* [大厂Redis面试，你能get到几个点 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_058 )
* [大厂秋招面试必备-从Redis应用以及原理说起 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_059 )
* [多维度了解Redis以及原理实现 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_063 )
* [手把手带你看Redis， skynet网络模块实现 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_073 )
* [—场Redis线上事故引发的思考 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_078 )
* [醍醐灌顶搞懂分布式多播以及消息队列的Redis解决方案 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_081 )
* [茅塞顿开搞懂开源框架(Redis, Nginx, Skynet)中锁的使用 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_082)
* [Redis，Memcached到Nginx，底层网络io中剥离精髓 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_125)
* [高并发场景下,Mysql与Redis的数据同步方案 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_143)
* [Redis是什么，用来做什么，要掌握到什么程度 ]( https://github.com/0voice/backend_video#nav_1_middleware_development_185 )

### <h3 id="nav_sec3_chp_07">📄 文章</h3>
 
### <h3 id="nav_sec3_chp_08">🍀 Paper</h3>
 
No.|Title|Translate|Company
:-------: | :--------------- | :------------|:---------------
1|[《Implementing Information System Using MongoDB and Redis》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/001-Implementing-Information-System-Using-MongoDB-and-Redis.pdf)|利用MongoDB和Redis实现信息系统|
2|[《15 Reasons to use Redis as an Application Cache》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/002-15-Reasons-Caching-is-best-with-Redis-RedisLabs-1.pdf) | 使用Redis作为应用程序缓存的15个原因| 
3|[《AMAZON ELASTICACHE》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/003-AMAZON-ELASTICACHE.pdf) |弹性评估 | 
4|[《PLeveraging Django and Redis using Web Scraping》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/004-PLeveraging-Django-and-Redis-using-Web-Scraping.pdf) |使用网页抓取对Django和Redis进行编辑 | 
5|[《Cloud Native at the Edge: High-performance Edge Inference with RedisAI》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/005-cloud-native-at-the-edge.pdf) |边缘原生云：使用RedisAI进行高性能边缘推断 | 
6|[《Contrail Architecture》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/006-contrail-architecture.pdf) |轨迹结构 | 
7|[《Database Caching Strategies Using Redis AWS Whitepaper》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/007-database-caching-strategies-using-redis.pdf) |使用Redis AWS白皮书的数据库缓存策略 | 
8|[《Deploying a Highly Available Distributed Caching Layer on Oracle Cloud Infrastructure using Memcached & Redis》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/008-deploying-memcached-and-redis-on-oci.pdf) |使用Memcached和Redis在Oracle云基础架构上部署高可用的分布式缓存层 | 
9|[《Scaling Memcache at Facebook》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/009-Scaling-Memcache-at-Facebook.pdf ) |在Facebook上扩展Memcache | 
10|[《From advert to action: behavioural insights into the advertising of financial products》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/010-behavioural-insights-into-the-advertising-of-financial-products.pdf ) |从广告到行动：金融产品广告的行为洞察 | 
11|[《Persistent Memory Performance in vSphere 6.7》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/011-optane-dc-pmem-vsphere67-perf.pdf) |vSphere 6.7中的持久内存性能 | 
12|[《Performance at Scale with Amazon ElastiCache》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/012-performance-at-scale-with-amazon-elasticache.pdf) |使用Amazon ElastiCache实现大规模性能 | 
13|[《Redis Enterprise For Financial Services》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/013-Redis-Enterprise-For-Financial-Services.pdf) |Redis金融服务企业 | 
14|[《Persisting Objects in Redis Key-Value Database》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/014-Persisting-Objects-in-Redis-Key-Value-Database.pdf) |在Redis键值数据库中持久化对象 | 
15|[《Break the Cost and Capacity Barrier with Intel® Optane™ DC Persistent Memory》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/015-redis-enterprise-brief.pdf) |使用Intel®Optane打破成本和容量壁垒™ DC持久存储器 | 
16|[《Redistribution, Inequality,and Growth》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/016-Redistribution-Inequality-and-Growth.pdf) |再分配、不平等和增长 | 
17|[《In-Memory Data processing using Redis Database》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/017-In-Memory-Data-processing-using-Redis-Database.pdf) |使用Redis数据库进行内存数据处理 | 
18|[《USING REDIS WITH SOURCEPRO DB》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/018-white-paper-sourcepro-db-redis.pdf) | 将REDIS与SOURCEPRO DB一起使用| 
19|[《Redis on Samsung NVMe Benchmarking Results》](https://github.com/0voice/newsql_nosql_library/blob/main/Redis/paper/019-WP-RedisLabs-Samsung-NVMe-Benchmark-103-Web-1.pdf) | 三星NVMe基准测试结果的Redis| 
 
### <h3 id="nav_sec3_chp_09">📙 电子书籍</h3>
 
* [《Redis的主从复制》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis的主从复制.pdf)
* [《Redis存储结构》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/redis-存储结构.pdf) 
* [《Redis实现分析》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis%20实现分析.pdf) 
* [《Redis与网站架构》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis与网站架构.pdf) 
* [《Redis入门指南(第2版)》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis入门指南(第2版).pdf) 
* [《Redis在京东到家的订单中的使用》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis在京东到家的订单中的使用.pdf) 
* [《Redis实战》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis实战.pdf) 
* [《Redis原代码分析》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis原代码分析.pdf) 
* [《Redis实现分析》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis实现分析.pdf) 
* [《Redis集群》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis集群.pdf) 
* [《Redis大数据之路》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/redis大数据之路.pdf) 
* [《深入了解Redis》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/深入了解redis.pdf) 
* [《大厂Redis面试，你能get到几个点》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/大厂redis面试，你能get到几个点.pdf) 
* [《新浪微博Redis优化历程》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/新浪微博redis优化历程.pdf) 
* [《Redis 多机特性工作原理简介》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis%20多机特性工作原理简介.pdf) 
* [《Redis设计与实现(第二版)》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis设计与实现(第二版).pdf) 
* [《Redis编程发布与订阅》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis编程_发布与订阅.pdf) 
* [《Redis cluster Specification (work in progress)》](https://github.com/0voice/newsql_nosql_library/tree/main/Redis/电子书/Redis%20cluster%20Specification%20(work%20in%20progress)%20–%20Redis.pdf) 
 
### <h3 id="nav_sec3_chp_10">🧲 常见问题</h3>


 
<br/>

### <h3 id="nav_sec3_chapter1_02">🦗 RocksDB</h3>
 
#### 特性：
* 高性能： RocksDB使用一套日志结构的数据库引擎，为了更好的性能，这套引擎是用C++编写的。 Key和value是任意大小的字节流。
* 为快速存储而优化： RocksDB为快速而又低延迟的存储设备（例如闪存或者高速硬盘）而特殊优化处理。 RocksDB将最大限度的发挥闪存和RAM的高度率读写性能。
* 可适配性： RocksDB适合于多种不同工作量类型。 从像MyRocks这样的数据存储引擎， 到应用数据缓存, 甚至是一些嵌入式工作量，RocksDB都可以从容面对这些不同的数据工作量需求。
* 基础和高级的数据库操作： RocksDB提供了一些基础的操作，例如打开和关闭数据库。 对于合并和压缩过滤等高级操作，也提供了读写支持。
 
* [RocksDB官方网址](https://rocksdb.org.cn/)
 
<br/>
 
## <h2 id="nav_sec3_chapter2">列存储数据库</h2>

* 相关产品：Cassandra, HBase
* 典型应用：分布式的文件系统
* 数据模型：以列簇式存储，将同一列数据存在一起
* 优势：查找速度快，可扩展性强，更容易进行分布式扩展
* 劣势：功能相对局限
                    
### <h3 id="nav_sec3_chapter2_01">🦕 Cassandra</h3>
 
Cassandra是开源分布式NoSQL数据库系统。用于储存收件箱等简单格式数据，集GoogleBigTable的数据模型与Amazon Dynamo的完全分布式的架构于一身，由于Cassandra良好的可扩展性，现在成为了一种流行的分布式结构化数据存储方案。是一个**网络社交云**计算方面理想的数据库。
 
![image](https://user-images.githubusercontent.com/87458342/132357265-4792209b-b01c-4910-9a40-8fa210a47531.png)
 
#### 特征
* 分布式
* 基于column的结构化
* 高伸展性

<br/>
 
## <h2 id="nav_sec3_chapter3">文档型数据库</h2>

* 相关产品：CouchDB、MongoDB、SequoiaDB
* 典型应用：Web应用（与Key-Value类似，Value是结构化的）
* 数据模型： 一系列键值对
* 优势：数据结构要求不严格
* 劣势： 查询性能不高，而且缺乏统一的查询语法

### <h3 id="nav_sec3_chapter3_01">🦜 CouchDB</h3>
 
<br/>
 
### <h3 id="nav_sec3_chapter3_02">🦢 MongoDb</h3>
 
<br/>
<br/>

# <h1 id="nav_sec4">🦊 NewSQL数据库</h1>
 
## <h2 id="nav_sec4_chapter2">SQL引擎</h2>

### <h3 id="nav_sec4_chapter2_01">🦅 TokuDB</h3>

<br/>
 
## <h2 id="nav_sec4_chapter4">分布式数据库</h2>

### <h3 id="nav_sec4_chapter4_01">🐪 TiDB</h3>
 
<br/>
 
### <h3 id="nav_sec4_chapter4_02">🐫 Oceanbase</h3>
 
<br/>
 
### <h3 id="nav_sec4_chapter4_03">🐐 MemDB</h3>



<br/>
<br/>
<br/>

## <h2 id="nav_9">🤝 鸣谢</h2>

##### 为了让我们的repo内容更加的丰富，更加的专业。欢迎大家贡献patch，希望大家在issue里面出谋划策，我们期待你的加入。

<a href="https://github.com/wangbojing">
    <img src="https://avatars.githubusercontent.com/u/18027560?v=4" width="40px">
</a> 

<a href="https://github.com/ls-Brynn">
    <img src="https://avatars.githubusercontent.com/u/87458342?v=4" width="40px">
</a> 

## 联系专栏

#### 零声教育，专注于c/c++Linux后台服务器开发架构技术学习提升。<br>
每天晚上8点【免费技术直播】：[分享Linux，Nginx，ZeroMQ，MySQL，Redis，fastdfs，MongoDB，ZK，流媒体，CDN，P2P，K8S，Docker，TCP/IP，协程，DPDK等技术内容，立即学习。](https://ke.qq.com/course/417774?flowToken=1037127)

#### 关注微信公众号【后台服务架构师】——【联系我们】，获取本repo最全PDF学习文档！

<img width="65%" height="65%" src="https://user-images.githubusercontent.com/87457873/130796999-03af3f54-3719-47b4-8e41-2e762ab1c68b.png"/>
