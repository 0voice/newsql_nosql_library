# MySQL源码文件结构及主要数据结构

* BUILD: 内含在各个平台、各种编译器下进行编译的脚本。如compile-pentium-debug表示在pentium架构上进行编译的脚本。
* Client: 客户端工具，如mysql, mysqladmin之类。
* Cmd-line-utils: readline, libedit工具。
* Config: 给aclocal使用的配置文件。
* Dbug: 提供一些调试用的宏定义。
* Extra: 提供innochecksum，resolveip等额外的小工具。
* Include: 包含的头文件
* Libmysql: 库文件，生产libmysqlclient.so。
* Libmysql_r: 线程安全的库文件，生成libmysqlclient_r.so。
* Libservices: 5.5.0中新加的目录，实现了打印功能。
* Man: 手册页。
* Mysql-test: mysqld的测试工具一套。
* Mysys:包含了对于系统调用的封装，用以方便实现跨平台。MySQL自己实现了一套常用的数据结构和算法，如string, hash等。
* Netware: 在netware平台上进行编译时需要的工具和库。
* Plugin:插件的目录，目前有一个全文搜索插件（只能用在myisam存储引擎）。
* Pstack: 异步栈追踪工具。
* Regex: 正则表达式工具。
* Scripts: 提供脚本工具，如mysql_install_db等
* Sql: 
mysql主要代码，数据库主程序mysqld所在的地方。大部分的系统流程都发生在这里。你还能看到sql_insert.cc, sql_update.cc, sql_select.cc，等等，分别实现了对应的SQL命令。后面我们还要经常提到这个目录下的文件。
大概有如下及部分:
  * SQL解析器代码: sql_lex.cc, sql_yacc.yy, sql_yacc.cc, sql_parse.cc等，实现了对SQL语句的解析操作。
  * "handler"代码: handle.cc, handler.h，定义了存储引擎的接口。
  * "item"代码：item_func.cc, item_create.cc，定义了SQL解析后的各个部分。
  * SQL语句执行代码: sql_update.cc, sql_insert.cc sql_select.cc, sql_show.cc, sql_load.cc，执行SQL对应的语句。当你要看"SELECT ..."的执行的时候，直接到sql_select.cc去看就OK了。
  * 辅助代码: net_serv.cc实现网络操作
  * 还有其他很多代码。
    * Sql-bench: 一些评测代码。
    * Sql-common: 存放部分服务器端和客户端都会用到的代码。

* Storage：这个目录包含了所谓的Mysql存储引擎 (storage engine)。存储引擎是数据库系统的核心，封装了数据库文件的操作，是数据库系统是否强大最重要的因素。Mysql实现了一个抽象接口层，叫做handler(sql/handler.h)，其中定义了接口函数，比如：ha_open, ha_index_end, ha_create等等，存储引擎需要实现这些接口才能被系统使用。这个接口定义超级复杂，有900多行 :-(，不过我们暂时知道它是干什么的就好了，没必要深究每行代码。对于具体每种引擎的特点，我推荐大家去看mysql的在线文档: http://dev.mysql.com/doc/refman/5.1/en/storage-engines.html
应该能看到如下的目录:
  * innobase, innodb的目录，当前最流行的存储引擎
  * myisam, 最早的Mysql存储引擎,一直到innodb出现以前，使用最广的引擎。
  * heap, 基于内存的存储引擎
  * federated, 一个比较新的存储引擎
  * example, csv，这几个大家可以作为自己写存储引擎时的参考实现，比较容易读懂
* Strings: string库。
* Support-files: my.cnf示例配置文件。
* Tests: 测试文件所在目录。
* Unittest: 单元测试。
* Vio:封装了virtual IO接口，主要是封装了各种协议的网络操作。
* Win: 给windows平台提供的编译环境。
* Zip: zip库工具
