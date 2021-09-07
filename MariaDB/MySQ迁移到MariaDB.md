# MySQ迁移到MariaDB

从MySQL迁移到MariaDB非常简单：
1. 数据和表定义文件（.frm）是二进制兼容的
2. 所有客户端API、协议和结构都是完全一致的
3. 所有文件名、二进制、路径、端口等都是一致的
4. 所有的MySQL连接器，比如PHP、Perl、Python、Java、.NET、MyODBC、Ruby以及MySQL C connector等在MariaDB中都保持不变
5. mysql-client包在MariaDB服务器中也能够正常运行
6. 共享的客户端库与MySQL也是二进制兼容的
也就是说，在大多数情况下，你完全可以卸载MySQL然后安装MariaDB，然后就可以像之前一样正常的运行。

目前MariaDB是发展最快的MySQL分支版本，新版本发布速度已经超过了Oracle官方的MySQL版本。
