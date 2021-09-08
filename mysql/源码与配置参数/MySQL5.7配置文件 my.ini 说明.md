# MySQL 8.0 配置参数参考
```C
[mysqld]
 
#####non innodb options (fixed)
max_connections = 800
table_cache = 2400
sort_buffer_size = 32K
join_buffer_size = 32K
query_cache_size = 0
query_cache_type = 0
 
#####MyISAM Specific options
key_buffer_size = 8M
read_buffer_size = 256K
read_rnd_buffer_size = 256K
 
#####fixed innodb options
innodb_file_per_table = true
innodb_data_file_path = ibdata1:50M:autoextend
innodb_flush_log_at_trx_commit = 2
innodb_flush_method = O_DIRECT
innodb_log_buffer_size = 256M
innodb_log_file_size = 2048M
innodb_log_files_in_group = 2
innodb_log_block_size=4096
innodb_read_io_threads = 16
innodb_write_io_threads = 16
innodb_buffer_pool_size = 16G
 
#####I/O Tuning for SSD
innodb_io_capacity = 16000
innodb_flush_neighbor_pages = none
innodb_adaptive_flushing_method = keep_average
innodb_read_ahead = 0
 
##### fusion io
innodb_use_atomic_write = 0
innodb_doublewrite = 1
innodb_checksums = 1
innodb_fast_checksum = 0
 
##### fusion io
innodb_use_atomic_write = 1
innodb_doublewrite = 0
innodb_checksums = 1
innodb_fast_checksum = 0
##### fusion io
innodb_use_atomic_write = 1
innodb_doublewrite = 0
innodb_checksums = 1
innodb_fast_checksum = 1
 
##### fusion io
innodb_use_atomic_write = 1
innodb_doublewrite = 0
innodb_checksums = 0
 
 
 
docker pull 172.16.1.50:50000/library/percona:5.7
 
## 初始化参考指令：
## ./bin/mysqld --defaults-file=./my.cnf --initialize --user=mysql --basedir=/usr/local/mysql57 --datadir=/data/mysql57
#
[client]
port    = 3306
socket  = /tmp/mysql.sock
 
[mysql]
prompt="\u@oceanbase06 \R:\m:\s [\d]> "
no-auto-rehash  # MySQL开启命令自动补全功能(auto-rehash)
 
[mysqld]
user    = mysql
port    = 3306
basedir = /usr/local/mysql
datadir = /data/mysql/datadir
socket  = /tmp/mysql.sock
pid-file = oceanbase06.pid
character-set-server = utf8mb4
skip_name_resolve = 1
open_files_limit    = 65535
back_log = 1024
max_connections = 512
max_connect_errors = 1000000
table_open_cache = 1024
table_definition_cache = 1024
table_open_cache_instances = 64
thread_stack = 512K
external-locking = FALSE
max_allowed_packet = 32M
sort_buffer_size = 4M
join_buffer_size = 4M
thread_cache_size = 768
query_cache_size = 0
query_cache_type = 0
interactive_timeout = 600
wait_timeout = 600
tmp_table_size = 32M
max_heap_table_size = 32M
slow_query_log = 1
slow_query_log_file = /data/mysql/datadir/slow.log
log-error = /data/mysql/datadir/error.log
long_query_time = 0.1
log_queries_not_using_indexes =1
log_throttle_queries_not_using_indexes = 60
min_examined_row_limit = 100
log_slow_admin_statements = 1
log_slow_slave_statements = 1
server-id = 192156
log-bin = /data/mysql/logs/mybinlog
sync_binlog = 1
binlog_cache_size = 4M
max_binlog_cache_size = 2G
max_binlog_size = 1G
expire_logs_days = 7
master_info_repository = TABLE
relay_log_info_repository = TABLE
gtid_mode = on
enforce_gtid_consistency = 1
log_slave_updates
binlog_format = row
binlog_checksum = 1
relay_log_recovery = 1
relay-log-purge = 1
key_buffer_size = 32M
read_buffer_size = 8M
read_rnd_buffer_size = 4M
bulk_insert_buffer_size = 64M
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
lock_wait_timeout = 3600
explicit_defaults_for_timestamp = 1
innodb_thread_concurrency = 0
innodb_sync_spin_loops = 100
innodb_spin_wait_delay = 30
 
transaction_isolation = REPEATABLE-READ
#innodb_additional_mem_pool_size = 16M
innodb_buffer_pool_size = 50176M
innodb_buffer_pool_instances = 8
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_data_file_path = ibdata1:1G:autoextend
innodb_flush_log_at_trx_commit = 1
innodb_log_buffer_size = 32M
innodb_log_file_size = 2G
innodb_log_files_in_group = 2
innodb_max_undo_log_size = 4G
 
# 根据您的服务器IOPS能力适当调整
# 一般配普通SSD盘的话，可以调整到 10000 - 20000
# 配置高端PCIe SSD卡的话，则可以调整的更高，比如 50000 - 80000
innodb_io_capacity = 4000
innodb_io_capacity_max = 8000
innodb_flush_neighbors = 0
innodb_write_io_threads = 8
innodb_read_io_threads = 8
innodb_purge_threads = 4
innodb_page_cleaners = 4
innodb_open_files = 65535
innodb_max_dirty_pages_pct = 50
innodb_flush_method = O_DIRECT
innodb_lru_scan_depth = 4000
innodb_checksums = 1
innodb_checksum_algorithm = crc32
#innodb_file_format = Barracuda
#innodb_file_format_max = Barracuda
innodb_lock_wait_timeout = 10
innodb_rollback_on_timeout = 1
innodb_print_all_deadlocks = 1
innodb_file_per_table = 1
innodb_online_alter_log_max_size = 4G
internal_tmp_disk_storage_engine = InnoDB
innodb_stats_on_metadata = 0
 
innodb_status_file = 1
# 注意: 开启 innodb_status_output & innodb_status_output_locks 后, 可能会导致log-error文件增长较快
innodb_status_output = 0
innodb_status_output_locks = 0
 
#performance_schema
performance_schema = 1
performance_schema_instrument = '%=on'
 
#innodb monitor
innodb_monitor_enable="module_innodb"
innodb_monitor_enable="module_server"
innodb_monitor_enable="module_dml"
innodb_monitor_enable="module_ddl"
innodb_monitor_enable="module_trx"
innodb_monitor_enable="module_os"
innodb_monitor_enable="module_purge"
innodb_monitor_enable="module_log"
innodb_monitor_enable="module_lock"
innodb_monitor_enable="module_buffer"
innodb_monitor_enable="module_index"
innodb_monitor_enable="module_ibuf_system"
innodb_monitor_enable="module_buffer_page"
innodb_monitor_enable="module_adaptive_hash"
 
[mysqldump]
quick
max_allowed_packet = 32M
 
 
INSTALL PLUGIN group_replication SONAME 'group_replication.so';
plugin-load=group_replication.so
 
保证mysql实例重启生效可以写到配置文件my.cnf
 
```
