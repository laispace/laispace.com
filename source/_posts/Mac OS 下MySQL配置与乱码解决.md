title: 'Mac OS 下MySQL配置与乱码解决'
tags:
  - mysql
  - python
id: 469
categories:
  - 后台编程
date: 2013-09-24 20:29:50
---

用惯了windows下xampp,打开Apache打开mysql就可以有一个本地服务器做测试了，在mac上发现：

1.xampp上的MySQL开启后，在终端输入『mysql』无反应，故在MySQL官网重装了个，终端就可以操作MySQL了；

2.MySQL默认端口是3306，两个MySQL是无法同时打开的，于是修改xampp中mysql的端口为3307（只要不冲突）就可以了：
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130924-1.png "QQ20130924-1")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130924-1.png)[
](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130924-1.png)

3.初学Python，用Python连接数据库，却发现无法连接xampp中mysql的数据库，这就纳闷了，显示错误：

OperationalError: (2002, "Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)")

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130924-2.png "QQ20130924-2")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130924-2.png)[
](http://www.laispace.com/?p=469)
修改了端口还不行么？查了好久了资料都没搞懂，stackoverflow上有类似的问题，真是个神奇的地方啊，修改xampp的配置文件：[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130924-3.png "QQ20130924-3")](http://www.laispace.com/?p=469)

让它能找到正确的socket,重新连接数据库，成功！

PS:除此之外，还可以在连接数据库时指定一个unix_socket使程序招到正确的sock:

[python]
conn=MySQLdb.connect(host=&quot;localhost&quot;,
                     user=&quot;root&quot;,passwd=&quot;&quot;,
                     db=&quot;wegroup&quot;,
                     unix_socket=&quot;/Applications/XAMPP/xamppfiles/var/mysql/mysql.sock&quot;)
                     # unix_socket 
[/python]

&nbsp;

4.因mac默认编码不是utf-8，用python操纵mysql输出数据库的中文信息显示乱码[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130927-2.png "QQ20130927-2")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130927-2.png)

解决办法:
在配置文件的［client］后添加default-character-set=utf8；
在［mysqld］后添加：
default-storage-engine=INNODB
character-set-server=utf8
collation-server=utf8_general_ci；
终端mysql直接查询是不出现问题了，Python文件头也声明了utf-8编码，还是没用，只好直接在MySQLdb.connect 参数中指定编码为utf8：

[python]
conn=MySQLdb.connect(host=&quot;localhost&quot;,
                     user=&quot;root&quot;,passwd=&quot;&quot;,
                     db=&quot;wegroup&quot;,
                     charset = &quot;utf8&quot;,
                     unix_socket=&quot;/Applications/XAMPP/xamppfiles/var/mysql/mysql.sock&quot;)
                     # unix_socket 
[/python]

如此一来，输出中文乱码的问题就解决啦：[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130927-1.png "QQ20130927-1")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/QQ20130927-1.png)