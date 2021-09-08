# MySQL 8.0 配置参数参考
```C
[mysqld]
lower_case_table_names          = 1
user                            = mysql
server_id                       = 1
port                            = 3306
 
default-time-zone = '+08:00'
enforce_gtid_consistency        = ON
gtid_mode                       = ON
binlog_checksum                 = none
default_authentication_plugin   = mysql_native_password
datadir                         = /data/mysql/data
pid-file                        = /data/mysql/tmp/mysqld.pid
socket                          = /var/lib/mysql/mysql.sock
tmpdir                          = /data/mysql/tmp/
skip-name-resolve               = ON
open_files_limit                = 65535
table_open_cache                = 2000
bind-address                    = 0.0.0.0
 
#################innodb########################
innodb_data_home_dir            = /data/mysql/data
innodb_data_file_path           = ibdata1:512M;ibdata2:512M:autoextend
#innodb_buffer_pool_size = 12000M
innodb_flush_log_at_trx_commit = 1
innodb_io_capacity = 600
innodb_lock_wait_timeout = 120
innodb_log_buffer_size = 8M
innodb_log_file_size = 200M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 85
innodb_read_io_threads = 8
innodb_write_io_threads = 8
innodb_thread_concurrency = 32
innodb_file_per_table
innodb_rollback_on_timeout
 
innodb_undo_directory           = /data/mysql/data
innodb_log_group_home_dir       = /data/mysql/data
 
###################session###########################
join_buffer_size = 8M
key_buffer_size = 256M
bulk_insert_buffer_size = 8M
max_heap_table_size = 96M
tmp_table_size = 96M
read_buffer_size = 8M
sort_buffer_size = 2M
max_allowed_packet = 64M
read_rnd_buffer_size = 32M
innodb_buffer_pool_size = 4G 
############log set###################
log-error                       = /data/mysql/log/mysqld.err
log-bin                         = /data/mysql/binlog/binlog
log_bin_index                   = /data/mysql/binlog/binlog.index
max_binlog_size                 = 500M
slow_query_log_file             = /data/mysql/log/slow.log
slow_query_log                  = 1
long_query_time                 = 10
log_queries_not_using_indexes   = ON
log_throttle_queries_not_using_indexes  = 10
log_slow_admin_statements       = ON
log_output                      = FILE,TABLE
master_info_file                = /data/mysql/binlog/master.info
```
