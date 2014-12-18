title: Ubuntu下部署nodejs+mongodb

date: 2014-05-25 15:46:36

categories: Node

tags: [Node, Mongo, Ubuntu]

---

> 捣鼓阿里云上的VPS，入门Linux，记录一下部署 NodeJs+MongoDB 的过程

1. [Ubuntu下安装NodeJS](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint-elementary-os)

	在 Ubuntu 12.04 ~ 13.04 中，默认安装的的 Node 版本是 0.6.x的

		$ sudo apt-get install nodejs
		$ sudo apt-get install npm
		$ node -v
		
	而在 Ubuntu 13.10 ~ 14.04 中则是 0.10.x 版本
	
	我的版本是旧版，所以需要使用这种方式来安装：
	
		$ sudo apt-get install software-properties-common
		$ sudo apt-get install python-software-properties
		// 安装以上两个包后才会有下面这个 add-apt-repository 命令：
		$ sudo add-apt-repository ppa:chris-lea/node.js
		$ sudo apt-get update
		$ sudo apt-get install python-software-properties python g++ make nodejs
		

2. [Linux下安装MongoDB](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-linux/)

<!-- more -->

	需要先导入mongodb.org的公钥到Ubuntu的包管理系统
	
		$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
	
	接着创建 /etc/apt/sources.list.d/mongodb.list 列表文件
	
		$ echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
		
	然后先更新包管理系统
	
		$ sudp apt-get update
		
	更新完毕就可以安装 Mongodb-org 这个包了
	
		$ sudo apt-get install mongodb-org
	
	建立存放数据库的目录
		
		$ mkdir -p /data/db
	
	启动MongoDB	
		
		$ sudo /usr/bin/mongod --dbpath /data/db
	
	启动 mongo shell 
	
		$ /usr/bin/mongo
	
		// 建立一个新的数据库
		$ use newDb
		// 创建一个 users 集合并往集合中插入数据
		$ db.users.insert({name: '小赖', age: 18});
		// 查看是否插入成功
		$ db.users.find()
		// 显示 { "_id" : ObjectId("5381a71b30f9768d2d0a53e2"), "name" : "小赖", "age" : 18 }
		// 成功！
	
		
		
			
					
