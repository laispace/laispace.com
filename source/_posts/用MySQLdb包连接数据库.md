title: 用MySQLdb包连接数据库
tags:
  - mysql
  - python
id: 489
categories:
  - 后台编程
date: 2013-09-27 15:11:40
---

捣鼓捣鼓，跨过好多坑终于在Mac上配置好了开发环境，开始学Python啦！
今天学习的是Python连接数据库，安装了python-mysql包后，使用它自带的一些方法就可以连接了：
[python]
#!/usr/bin/python
# -*- coding: utf-8 -*- 
import sys
import MySQLdb

db = None

try:
    # 连接数据库
    db=MySQLdb.connect(host=&quot;localhost&quot;,
                     user=&quot;root&quot;,passwd=&quot;&quot;,
                     db=&quot;wegroup&quot;,
                     charset = &quot;utf8&quot;,
                     unix_socket=&quot;/Applications/XAMPP/xamppfiles/var/mysql/mysql.sock&quot;)
                     # unix_socket 
    cursor = db.cursor()

    # 执行一个查询
    cursor.execute(&quot;SELECT VERSION()&quot;)
    data = cursor.fetchone()
    # 显示数据库的版本
    print &quot;Database version: %s&quot; % data

    # 创建表user(id,name) 并插入数据
    cursor.execute(&quot;CREATE TABLE IF NOT EXISTS user(id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(30))&quot;)
    cursor.execute(&quot;INSERT INTO user(name) VALUES('user1')&quot;)
    cursor.execute(&quot;INSERT INTO user(name) VALUES('user2')&quot;)
    # commit 后数据才会真正添加到数据表中！
    db.commit()

    # 更新数据
    cursor.execute(&quot;UPDATE user SET name = %s WHERE id = %s&quot;,(&quot;xiaolai&quot; , 2));

    # 获取数据并遍历
    cursor.execute(&quot;SELECT * FROM user&quot;);
    # 获取描述信息
    desc = cursor.description
    print &quot;描述信息：&quot;, desc
    # 打印表头
    print &quot;%s %s&quot; % (desc[0][0],desc[1][0])

    # 获取结果数
    count = cursor.rowcount
    for i in range(count):
        row = cursor.fetchone()
        # 每一个都是一个元组
        print row[0], row[1]

finally:
    # 记得要关闭！
    if db:
        db.close()
[/python]
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/屏幕快照-2013-09-27-下午3.08.46.png "屏幕快照 2013-09-27 下午3.08.46")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2013/09/屏幕快照-2013-09-27-下午3.08.46.png)