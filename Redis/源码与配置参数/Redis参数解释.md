# Redis的参数解释

* include /path/to/local.conf
  * 当有公用配置时,可以采用独立出公共配置文件然后引入的方式达到公共配置unixsocket
* /tmp/redis.sock
  * 通过socket文件进行通信,比通过TCP模型通信效率更高,但是需要客户端和服务端都在同一个机器上
* timeout 0
  * 当连接空闲超过一定时间后,就会关闭此链接,0表示禁止关闭连接
* tcp-keepalive 0
  * 用于TCP的socket连接活跃性检测,0表示关闭检测功能,如果开启会增加网络流量
* syslog-enabled no
  * 是否开启系统日志,即采用linux的syslog模块来记录日志,默认不开启!
* repl-diskless-sync no
  * 全量同步时,是否把快照先存于磁盘,如果存于磁盘,会有10性能,但是可以复用RDB文件
  * 如果不存于磁盘,就会直接把RDB文件流发送给从节点,复用性降低,在从节点比较少的时候,建议开启
* repl-diskless-sync-delay 5
  * 与上一个参数相关,当上一个设置为yes ,采用不存储磁盘时, master等待一定时间,等待更多的从节点要求增量复制,然后进行并行复制
* maxclients 10000
  * 最大客户端连接数,需要同时修改linux的最大线程数和文件句柄数
* maxmemory
  * 设置内存的最大值,要小于物理内存,如果有从服务器或者其它应用也适用此物理服务器,则要预留更多的空间,避免出现内存溢出,或者性能变慢,可以占用物理内存的3/4
  * 当内存使用达到此限制后,将会触发内存淘汰或者返回错误给客户端,需要根据maxmemory-policy属性的配置来做决定
* maxmemory-policy noeviction
  * 内存满了后的淘汰策略,默认为noeviction ,即没有淘汰策略,直接返回错误给客户端
  * volatile-lru :利用lru算法淘汰设置过过期时间的key
  * allkeys-lru :利用lru算法淘汰所有的keyv
  * olatile-random :随机的淘汰设置过过期时间的key
  * allkeys-random :随机的淘汰所有的keyvolatile-ttl :淘汰即将到达过期时间的key
  * noeviction :没有淘汰算法
  * slowlog-log-slower-than 10000
  * 设置慢日志的时间,即操作花费的时间大于此时间就会记录日志,单位为微妙
* client-output-buffer-limit
  * 当某些客户端由于处理速度不够,服务器端会缓冲一部分数据,当超过缓冲硬缓冲的限制,则会把客户端的连接关闭
  * 当超过软限制( soft limit) ,并且达到一定的时间soft seconds ,则也会关闭连接
  * client-output-buffer-limit normal 0 0 0 //normal代表常规客户端,包括监控类型的客户端
  * client-output-buffer-limit slave 256mb 64mb 60 //slave代表主从结构的客户端
  * client-output-buffer-limit pubsub 32mb 8mb 60 //订阅类型的客户端

# info命令

* 通过cli合令行执行info或者info all可以查看更详细的监控信息，# server为服务器端信息

### 客户端信息

* client-longestoutput list :由于客户端处理比较慢,服务器端会缓冲一部分数据,如果客户端处理及时，一般为0, 比如:当slave同步滞后比较严重时,此值就比较大,会导致redis内存耗尽
* clientlongest-outputlist :由于客户端处理比较慢,服务器端会缓冲一部分数据,如果客户端处理及时一般为0,比如:当slave同步滞后比较严重时,此值就比较大,会导致redis内存耗尽
* clientbiggestinputbuf:服务器端处理比较慢,缓冲客户端发送过来的数据blocked_clients :阻塞的客户端数,通过调用阻塞命令导致( BLPOP BRPOP BRPOPLPUSH )
* 内存使用情况
  * used_memory: redis分配的内存使用情况
  * used_memory_human:以可读的方式显示上一个值
  * used-memory_rss: linux系统的rss值,与top命令看见的一样,实际使用物理内存(包含共享库占用的内存)used_memory_peak:最高峰值
  * mem fragmentation_ratio: used_memory_rss/ used_memorymem_allocator:分配器
* Persistence
  * Loading :表示是否在加载dump的文件
  * rdb_changes-since-last-save :上次dump后,发生了多少变化rdb-bgsavein-progress:是否在做bgsave操作
  * rdb lastsave-time :上一次rdb save成功的时间
  * rdb_last-bgsave_status :上一次rdb save操作的状态
  * rdb lastbgsavetime-sec :上一次bqsave花费的时间
  * rdb_currentbgsave-time-sec:当前bqsave操作花费的时间aofenabled :是否开启aof
  * aofrewrite-in-progress: aof的rewrite是否正在执行
  * aof rewrite-scheduled:是否有aof的rewrite操作等待执行aoflast rewrite-time-sec:最后一次aof的rewrite消耗的时间
  * aof_current - rewritetime-sec:当前正在执行的aof的rewrite时间
  * aof-current-size :当前aof文件的大小
  * aof-base-size:上一次操作或者rewrite后的aof文件大小
  * aof_pending_rewrite:是否有aof的rewrite操作等待执行,当rdb的save操作执行完成后
  * aof-bufferlength : aof的缓冲大小
  * aof-rewritebufferlength : aof的rewrite的缓冲大小
  * aof-delayed_fsync :延迟刷新磁盘的计数器
* Stats
  * total_connections-received:服务器接受的所有连接数
  * total_commands_processed: redis服务器处理过的所有命令数
  * instantaneous-_ops-per-sec :每秒处理的操作数
  * total_net-input-bytes :网络输入数据总量
  * total_net-output-bytes :网络输出数据总量
  * instantaneous-input-kbps :网络输入的速度
  * instantaneous-outputkbps :网络输出速率rejected-connections :由于maxclients现在被拒绝的连接数
  * sync_partial_ok
  * sync_full
  * sync_partial err
  * expired_keys :运行以来过期的key的数量
  * evicted-keys:运行以来删除过的key的数量
  * keyspace_misses :在主字典中没有找到key的次数
  * latest fork-_usec :最后一次fork操作的耗时
* monitor
  * 通过monitor命令,可以查看到服务器所有接收到被执行的命令只可以用于调试时候用,不能一直用于生产环境
