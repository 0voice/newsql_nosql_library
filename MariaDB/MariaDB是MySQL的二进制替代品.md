# MariaDB是MySQL的二进制替代品

MariaDB是同一MySQL版本的二进制替代品(例如MySQL 5.1->MariaDB 5.1, MariaDB5.2和MariaDB 5.3是兼容的。MySQL 5.5将会和MariaDB 5.5保持兼容)。这意味着：
* 数据和表定义文件(.frm)文件是二进制兼容的。
* 所有客户端APIs，协议和结构都是相同的。
* 所有的文件名、二进制文件的路径、端口、套接字等等……应该是相同的。
* 所有MySQL的连接器(PHP Python Perl Java .NET MyODBC Ruby MySQL C连接器等) 和MariaDB的不变。有一些和PHP5的安装问题要注意(一个和老的PHP5如何检查库兼容性的bug)。
* mysql-client包还可以与MariaDB服务器一起工作。
可以卸载MySQL再安装MariaDB，可以工作很好。(不需要转换成任何数据文件，如果使用同一主版本,比如5.1)。
在脚本升级方面也做了大量的工作，从MySQL 5.0升级到MariaDB 5.1比从MySQL 5.0到MySQL 5.1更容易。
