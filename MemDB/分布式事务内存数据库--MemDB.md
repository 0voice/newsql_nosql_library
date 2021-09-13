## 高性能和可伸缩
* 快速的内存数据访问，高达 25,000 ops (single doc read/write) /碎片 (each shard take one CPU core).
* 系统可水平伸缩
* 没有单点瓶颈
## 真正的分布式 ACID 事务
* 在分布式环境真正支持 ACID (Stands for Atomicity, Consistency, Isolation, Durability) 事务
* MemDB 让 MongoDB 支持 ACID 事务
* 兼容 MongoDB 和 Mongoose
* 直接使用 MongoDB 的查询 API
* 内置 Mongoose 支持
## 高可用性
* 每个碎片都有一个或者多个 slaves，不会发生单点故障
## 链接
* 主页：http://memdb.org
* GitHub：https://github.com/rain1017/memdb
* GitHub权威资料：https://github.com/0voice/newsql_nosql_library
* API参考：https://github.com/rain1017/memdb/wiki/API-Reference

## 架构
![image.png](https://pic.leetcode-cn.com/1631523232-yuNMjm-image.png)


## 特点
MemDB 是一个key/value平台系统。
MemDB定位于【memcache、redis】与【mysql】间的一个key/value持久存储平台。
如下特点：
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

## 配置文件
```C
[common]
home=/usr/local/memdb #运行目录
thread_num=4 #线程数量
store_type=bdb #存储engine类型
sock_buf_kbyte=8192 #数据同步的socket buf
max_chunk_kbyte=1024 #数据同步最大的chunk大小
store_flush_num=10000 #多少条数据必须sync 存储
host=172.168.1.8 #主机标示，为内部同步的host ip地址
idc=yf #所属的idc名字
group=1 #所属的分组名字
trans_conn_num=20 #内部消息转发的连接数量
sync_conn_num=20 #内部数据同步的连接树立
write_cache_mbyte=256 #写cache的大小
read_cache_mbyte=2048 #读cache的大
read_cache_max_key_num=20000000 #读cache的最大key数量
master_lost_binlog=100000 #slave变为master，运行丢失的最大binlog数量。
max_write_queue_messge=500000 #写队列排队消息的最大数量，超过则写失败。
max_trans_message=100000 #并发master转发消息的最大数量，超过则失败。
monitor=:9900 #监控端口
enable_expire=yes/no #是否支持key的expire，yes：支持；no：不支持。
expire_default=3600 #若没有指定expire，缺省的expire值，单位为s。
expire_concurrent = 100 #expire检查的并发消息数量。

[inner_dispatch]
user=async #内部分发的连接用户名
passwd=async_passwd #内部分发的用户口令
listen=:9903 #内部分发的监听地址

[outer_dispatch]
user=async #外部分发的连接用户名
passwd=async_passwd #外部分发的用户口令
listen=:9903 #外部分发的监听地址

[zk]
server=172.16.42.1:2181 #zookeeper的连接
auth= #zk的认证
root=/memdb #zookeeper配置信息的root目录

[binlog]
path=/data2/kv_data/data/binlog #binlog文件的路径
file_prefix=binlog #binlog文件的前缀
file_max_mbyte=1024 #binlog文件的最大大小
max_file_num=24 #维护的binlog文件的数量
del_out_file=yes #是否删除不维护的binlong文件
cache=no #对接受的到binlog是否cache，cache的话，若程序以外down会丢失
flush_log_num=100000 #多少条日志flush binlog文件
flush_log_second=100 #多少秒必须flsuh binlog文件

[bdb] #bdb存储engine的配置
env_home=/data3/bdb_home #bdb的env home目录
db_path=/data4/bdb_data #bdb的db目录
compress=yes #bdb是否压缩
cache_msize=3000 #bdb的cache大小
page_ksize=32 #bdb的page大小
[bdbc] #海量、高效、动态可配多计数值的计数器引擎配置
env_home=/data3/bdb_home #bdb的env home目录
db_path=/data4/bdb_data #bdb的db目录
compress=yes #bdb是否压缩
cache_msize=3000 #bdb的cache大小
page_ksize=32 #bdb的page大小
key_type=int64 #key的类型。可为int32/int64/int128/int256/char
int32_hex=no/yes #对于int32的key，返回时是否以16进制表示
int64_hex=yes/no #对于int64的key，返回时是否以16进制表示
hex_upper=yes/no #对于返回的16进制的key，是否大写a-f
group_start_time_bit =0 #bdb文件分组的key的group bit32值的开始bit位
group_end_time_bit=5 #bdb文件分组的key的group bit32值的结束bit位
counter_def_file=/usr/home//unistor/bin/counter_def.dat #key的计数器名字配置文件
```

Mdbgoose：
```C
var memdb = require('memdb-client');
var P = memdb.Promise;
var mdbgoose = memdb.goose;
// Define player schema
var playerSchema = new mdbgoose.Schema({
    _id : String,
    name : String,
    areaId : Number,
    deviceType : Number,
    deviceId : String,
    items : [mdbgoose.SchemaTypes.Mixed],
}, {collection : 'player'});
// Define player model
var Player = mdbgoose.model('player', playerSchema);
var main = P.coroutine(function*(){
    // Connect to memdb
    yield mdbgoose.connectAsync({
        shards : { // specify all shards here
            s1 : {host : '127.0.0.1', port: 31017},
            s2 : {host : '127.0.0.1', port: 31018},
        }
    });
    // Make a transaction in s1
    yield mdbgoose.transactionAsync(P.coroutine(function*(){
        var player = new Player({
            _id : 'p1',
            name: 'rain',
            areaId : 1,
            deviceType : 1,
            deviceId : 'id1',
            items : [],
        });
        // insert a player
        yield player.saveAsync();
        // find player by id
        var doc = yield Player.findByIdAsync('p1');
        console.log('%j', doc);
        // find player by areaId, return array of players
        var docs = yield Player.findAsync({areaId : 1});
        console.log('%j', docs);
        // find player by deviceType and deviceId
        player = yield Player.findOneAsync({deviceType : 1, deviceId : 'id1'});
        // update player
        player.areaId = 2;
        yield player.saveAsync();
        // remove the player
        yield player.removeAsync();
    }), 's1');
});
if (require.main === module) {
    main().finally(process.exit);
}
```
