title: MongoDB入门-CRUD简单操作
tags:
  - mongodb
categories:
  - Node
date: 2014-03-09 14:22:10

---

按着 [mongodb官网](http://www.mongodb.org/) 教程安装好后，练习一下CRUD（增加、读取、更新、删除）操作。

// 我把数据库保存在了 ~/nosql/mongodb下

# 第一步-启动mongodb：

$ cd nosql/mongodb/bin

$ sudo ./mongod

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.47.04-PM.png "Screen Shot 2014-03-09 at 1.47.04 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.47.04-PM.png)

这样，mongodb就启动了，接着是创建数据库。进行简单的CRUD。

# 第二步-启动 mongo shell

$ ./mongo

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.51.09-PM.png "Screen Shot 2014-03-09 at 1.51.09 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.51.09-PM.png)

# 第三步-创建数据库laispace

// $ help

// 显示已有的数据库

$ show dbs

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.55.25-PM.png "Screen Shot 2014-03-09 at 1.55.25 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.55.25-PM.png)

// 切换数据库，这里用了新的数据库laispace，则自动创建

$ use laispace

// 注意这时候没有插入数据，但实际上已经创建了

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.57.12-PM.png "Screen Shot 2014-03-09 at 1.57.12 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-1.57.12-PM.png)

# 第四步-向数据库中添加数据

// db即表示laispace数据库，先建立users文档并插入一个新用户

$ var newUser = { "name": "赖小赖", "email": "example@gmail.com"};

$ db.users.insert(newUser);

// 继续插入

$ var anotherNewUser = { "name": "小清", "email": "123456@gmail.com"};

$ db.users.insert(anotherNewUser);

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.03.43-PM.png "Screen Shot 2014-03-09 at 2.03.43 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.03.43-PM.png)
<div></div>
<div># 第五步-查找数据</div>
<div>// 查看laispace是否创建</div>
<div>$ show dbs</div>
<div>// 切换到laispace</div>
<div>$ use laispace</div>
<div>// 查看laispace下有哪些文档</div>
<div>$ show collections</div>
<div>// 查看laispace.users下有哪些数据</div>
<div>$ db.users.find(); // 或 $ db.getCollection('users').find();</div>
<div>[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.07.18-PM.png "Screen Shot 2014-03-09 at 2.07.18 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.07.18-PM.png)</div>
<div>// 查看其中一个</div>
<div>$ db.users.findOne({"name": "小清"});</div>
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.11.10-PM.png "Screen Shot 2014-03-09 at 2.11.10 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.11.10-PM.png)

# 第六步-更新数据

var xiaoqing = {

"name": "小清",
"email": "abcd@laispace.com",
"password": "myLatestPassword"
};

$ db.users.update({"name": "小清"}, xiaoqing);

$ db.users.findOne({"name": "小清"});

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.15.05-PM.png "Screen Shot 2014-03-09 at 2.15.05 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.15.05-PM.png)

# 第七步-删除数据

// 看看已有的数据

$ db.users.find();

$ db.users.find({"name": "小清"});

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.16.37-PM.png "Screen Shot 2014-03-09 at 2.16.37 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.16.37-PM.png)

// 删除 小清

$ db.users.remove({"name": "小清"});

$ db.users.findOne({"name": "小清"});

$ db.users.find();

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.18.37-PM.png "Screen Shot 2014-03-09 at 2.18.37 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.18.37-PM.png)

// 删除全部

$ db.users.remove();
$ db.users.find()

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.20.42-PM.png "Screen Shot 2014-03-09 at 2.20.42 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-2.20.42-PM.png)

# 第八步-继续学习

$ console.log("待补充！");
<!--more-->

# 其他笔记

- mongodb 默认占用了系统的27017端口，而打开 http://localhost:28017 则可以进入管理界面：

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-3.18.30-PM-1024x821.png "Screen Shot 2014-03-09 at 3.18.30 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-09-at-3.18.30-PM.png)

# 修改器

[javascript]

// 原子修改器
db.users.update({&quot;name&quot;: &quot;小清&quot;}, {
&quot;$inc&quot;: {
&quot;age&quot;: 1 // 年龄加一
}
})
&quot;$inc&quot; // 增加
&quot;$set&quot; // 修改，无则创建
&quot;$unset&quot; // 删除

// 数组修改器
// &quot;$push&quot; // 添加
// &quot;$pop&quot;: {key: 1} // 数组末删除一个元素
// &quot;$pop&quot;: {key: -1} // 数组头删除一个元素
// &quot;$pull&quot;: {&quot;foo&quot;: &quot;bar&quot;} // 删除数组foo中的bar

// 函数 update(query , obj , upsert , multi) 参数说明：
$ db.users.update({&quot;name&quot;: &quot;小清&quot;}, xiaoqing);
// 若指定第三个参数upsert为true,即：
$ db.users.update({&quot;name&quot;: &quot;小清&quot;}, xiaoqing, true);
// 则表示：
// - 若找到匹配文档，正常更新；
// - 若没有文档符合更新条件，则以这个条件和更新文档为基础建立一个新的文档。
// 若指定第四个参数multi为true, 即：
$ db.users.update({&quot;name&quot;: &quot;小清&quot;}, xiaoqing, true, true);
// 则表示：
// 匹配到的所有文档都得到更新（为false则只匹配第一个）。

[/javascript]

&nbsp;

# 查询操作

[javascript]

// 查找全部，缺省参数{} 即 db.users.find({});
$ db.users.find();
// 指定范围查找
$ db.users.find({&quot;name&quot;: &quot;小清&quot;, &quot;email&quot;: &quot;abcd@laispace.com&quot;});
// 查找全部，但只返回指定的键，1表示true， 注意 _id 总会被返回
$ db.users.find({}, {&quot;name&quot;: 1, &quot;email&quot;: 1});
// 查找全部，但不要返回指定的键，0表示false
$ db.users.find({}, {&quot;password&quot;: 0});
// 大于小于
$ db.users.find({&quot;age&quot;: {&quot;$gte&quot;: 18, &quot;$lte&quot;: 30}}) // 大于等于18小于等于30岁
db.users.find({&quot;registered&quot;: {&quot;$lt&quot;: new Date(&quot;01/01/2014&quot;)}}); // 在2014/01/01前注册
// 不等于
$ db.users.find({&quot;name&quot;: {&quot;$ne&quot;: &quot;小清&quot;}}); // 用户名不是『小清』
// 包含于
db.users.find({&quot;name&quot;: {&quot;$in&quot;: [&quot;小清&quot;, &quot;小赖&quot;]}}); // 用户名是『小清』或『小赖』
// 不包含于
db.users.find({&quot;name&quot;: {&quot;$nin&quot;: [&quot;小清&quot;, &quot;小赖&quot;]}}); // 用户名不是『小清』和『小赖』
// 或
db.users.find({&quot;$or&quot;: [{&quot;name&quot;: &quot;小清&quot;}, {&quot;email&quot;: &quot;123@example.com&quot;}]}); // 用户名是『小清』或 邮箱是 &quot;123@example.com&quot;

[/javascript]