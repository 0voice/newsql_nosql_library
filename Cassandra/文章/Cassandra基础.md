# Cassandra基础

（1）概念
Cassandra 是高度可扩展的、高性能的分布式 NoSQL 数据库。 Cassandra 旨在处理许多商品服务器上的大量数据，提供高可用性而无需担心单点故障。

Cassandra 具有能够处理大量数据的分布式架构。 数据放置在具有多个复制因子的不同机器上， 以获得高可用性，而无需担心单点故障。

（2）数据模型
Key Space（对应 SQL 数据库中的 database）

一个Key Space中可包含若干个CF，如同SQL数据库中一个database可包含多个table。

Key（对应 SQL 数据库中的主键）

在Cassandra中，每一行数据记录是以key/value的形式存储的，其中key是唯一标识。

column（对应 SQL 数据库中的列）

Cassandra中每个key/value对中的value又称为column，它是一个三元组，即：name， value和timestamp，其中 name需要是唯一的。

super column（SQL 数据库不支持）

cassandra允许key/value中的value是一个map(key/value_list)，即某个column有多个子列。

Standard Column Family（相对应 SQL 数据库中的 table）

每个CF由一系列row组成，每个row包含一个key以及其对应的若干column。

Super Column Family（SQL 数据库不支持）

每个SCF由一系列row组成，每个row包含一个key以及其对应的若干super column。

（3）Cassandra 一致 Hash 和虚拟节点
一致性 Hash（多米诺 down 机） 为每个节点分配一个 token，根据这个 token 值来决定节点在集群中的位置以及这个节点所存储 的数据范围。

虚拟节点（down 机多节点托管） 由于这种方式会造成数据分布不均的问题，在 Cassandra1.2 以后采用了虚拟节点的思想：不需要为每个节点分配token，把圆环分成更多部分，让每个节点负责多个部分的数据，这样一个节点移除后，它所负责的多个token会托管给多个节点处理，这种思想解决了数据分布不均的问题。


