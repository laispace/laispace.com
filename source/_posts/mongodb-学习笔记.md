title: MongoDB-学习笔记

date: 2014-05-14 19:03:55

categories: Database


tags: 
	- mongodb 
	- mongoose

---

## MongoDB 支持的几钟数据类型

- String

		// 字符串
		var mySite = 'laispace.com';			

- Array

		// 数组
		var myFriends = ['小赖', '小清', '大花', '大黄'];

- Boolean

		// 布尔类型，true 或 false 		
		var IloveU = true;

- Code

		// 代码，可在数据库内运行
		var myCode = new BSON.Code('function (name) {
			return 'My name is ' + name;
		}');

- Date

		// 日期
		var myDate = new Date();
		

		
- Integer

		// 整数
		var myAge = 18; // 让我年轻一次嘛~

<!--more-->

- Long

		// 长整数
		var myMoney = new BSON.Long('999999999999999999999');
		
- Hash

		// 数据字典
		var myInfo = {
			name: '小赖',
			age: '18,
			sex: 'male'
		};

- Null 

		// null 值
		var myBadFriend = null;

- ObjectId

		// 索引ID，12字节，24位16进制串，用于唯一标识
		var myId = new BSON.ObjectId()

- DBRef

		// 数据库引用
		var bestFriendId = new BSON.DBRef('users', friendObjectId);

<!-- more -->

## 使用MongoDB


- 连接数据库

		var mongodb = require('mongodb');
		
	创建 Server对象 实例		

		var server = new mongodb.server(host, port, [options]);

	options 可选，默认参数为：
 
		options = {
			ssl: false, // 是否启用ssl安全协议
			sslValidate: false, // 是否验证服务器提交的证书
			sslCA: null, // 数组，一组供服务器验证的证书
			sslCert: null, // 数组，一组服务器验证时使用的证书
			sslKey: null, // Buffer 或 String, 一个供服务器验证时使用的私钥
			sslPass: null, // Buffer 或 String, 一个供服务器验证时使用的证书密码
			poolSize: 5, // 整数，连接池中最大连接数
			socketOptions: null, // 对象，指定与服务器连接时端口设置的选项
			// socketOptions: {
			//	keepAlive: [Number], // 整数毫秒，指定客户端向服务器发送 keepAlive探测包的时间间隔
			//	connectTimeMS: [Number], // 整数毫秒，指定客户端连接超时时间
			//	socketTimeoutMS: [Number] // 整数毫秒，指定客户端端口超时时间
			// }
			logger: null, // 对象，用于记录日志
			auto_reconnect: false, // 是否在客户端与服务器连接出错时自动重连
			disableDriverBSONSizeCheck: false // 是否在 BSON对象过大时抛出错误
		}
	
	创建 Db对象 实例

	创建了 server服务器对象后，使用它创建代表 MongoDB数据库的 Db对象：

		var Db = new mongodb.Db(databaseName, server, [options]);
		
	options 可选，默认参数为：
	
		options = {
			safe: false // 是否使用  getLastError命令 执行数据操作，该命令返回数据操作的执行结果
			w: [Number lt -1], // 大于-1d的整数或字符串，用于设置 write concern机制，该值大于等于1或为字符串时，才承认数据被写入
			wtimeout: [Number], // 整数毫秒，指定数据操作的超时时间
			fsync: false, // 写入数据的方法返回前是否等待数据库内部的 fsync操作
			journal: false, // 写入数据的方法返回前是否等待数据库内部的 journal操作 
			native_parse: false, // 是否使用C++ BSON解析器
			forceServerObjectId: false, 是否强制在服务器端而不是客户端创建 BSON对象ID
			pkFactory: {}, // 用于重载数据库内部生成的对象ID主键的对象
			serializeFunctions: false, // 是否序列化Javascript函数
			raw: false, // 是否使用二进制BSON数据缓存区来执行数据操作
			recordQueryStats: false, // 查询数据时是否在数据库内部执行查询统计
			retryMiliSeconds: 5000, // 整数毫秒，指定连接数据库失败时建个多长时间重连数据库
			numberOfRetries: 5, // 重连数据库的次数
			logger: null, // 对象，用于记录日志
			slaveOk: null, // 整数，查询时使用的SlaveOk值
		}
	
	创建好 Db对象后，则开始打开数据库进行操作：
		
		// 连接失败时 db 为 null
		Db.open(function (err, db) {
			// db operations here.
			
			// 关闭数据库， forceClose 是否强制关闭，强制关闭后不可再用 open() 方法打开 
			// db.close([forceClose], function (err) {
				if (err) throw err;
				// operations after closing db.
			})		
		})				
	
	实例：

		var mongodb = require('mongodb');
		var host = 'localhost';
		var port = mongodb.Connection.DEFAULT_PORT || 1234;
		// 创建服务器实例
		var server = new mongodb.Server(host, port, {auto_connect: true});
		// 创建数据库实例
		var Db = new mongodb.Db('testDbName', server, {safe: true});
		// 打开数据库
		Db.open(function (err, db) {
			if (err) throw err;
			console.log('连接数据库成功');
			// 记得操作完要关闭数据库
			db.close(function (err) {
				if (err) throw err;
				// console.log('关闭数据库成功');
			});
		});

- 增查改删操作

	先获取数据库对象的文档集合
	
		// options 参数与上文的大致相同，增加
		// optiosn.strict, 默认为false，指定是否在访问的集合不存在时抛出错误
		db.collection(collectionName, [options], function (err, collection) {
			if (err) throw err;
			console.log('获取到的文档集合是：\n', collection);
			// 在这里进行【增查改删】操作
			// 增
			collection.insert(docs, [options], [callback]);
			// 查
			collection.find(selector, [options]).toArray(callback);
			// 改
			collection.update(selector, document, [options], [callback]);
			// 查并改
			collection.findAndModify(selector, sort, document, [options], [callback]);
			// 删
			collection.remove([selector], [options], [callback]);
			// 查并删
			collection.findAndRemove(selector, sort, [options], [callback]);
		});	
	
	collection.insert(docs, [options], [callback])中的options与上文的大致相同，增加：
	
		options = {	
			continueOnError: false, // 若一个文档插入失败，是否继续插入剩余文档
			checkKeys: true, // 插入数据时是否取消检查该数据文档的主键是否已存在的处理
		}	

	collection.update(selector, document, [options], [callback]);中的options与上文的大致相同，增加：
	
		options = {	
			upsert: false, // 是否在更新时执行upsert操作：数据不存在则创建
			multi: false, // 是否更新所有符合查询条件的数据文档，默认为false即更新第一条
		}	
	
	collection.remove([selector], [options], [callback]);中的options与上文的大致相同，增加：
	
		options = {	
			single: false, // 是否只删除符合条件的第一条数据文档
		}						
		
	【增查改删】操作的实例：
	
		var mongodb = require('mongodb');
		var host = 'localhost';
		var port = mongodb.Connection.DEFAULT_PORT || 1234;
		var server = new mongodb.Server(host, port, {auto_connect: true});
		var Db = new mongodb.Db('testDbName', server, {safe: true});
		Db.open(function (err, db) {
			if (err) throw err;
			console.log('连接数据库成功');
			
			// 先获取文档集合
			db.collection('users', function (err, collection) {
				// 【增】
				collection.insert({
					name: '小赖',
					password: 'pws123456',
					email: 'laixiaolai@foxmail.com',
					age: 18,
					sex: 'male'
				}, function (err, docs) {
					// docs 是成功插入后的文档集合
					console.log('\n【增】插入数据成功，刚插入的数据是：\n', docs);
					// 记得关闭数据库！
					// db.close();
				});
				
				// 【查】, {} 表示查询所有
				collection.find({}).toArray(function (err, docs) {
					if (err) throw err;
					console.log('\n【查】所有文档集合是：\n', docs);
				});
				// 【查】，{name: '小赖'} 为限定条件
				collection.find({name: '小赖'}).toArray(function (err, docs) {
					if (err) throw err;
					console.log('\n【查】名字叫小赖的文档是：\n', docs);
				});
				
				// 【改】
				var xiaoqing = {
					name: '小清',
					password: 'newPws123456',
					email: 'abcd@laispace.com',
					age: 180,
					sex: 'male'
				};
				// 把名字为小赖的用户改为小清
				collection.update({name: '小赖'}, xiaoqing, {upsert: true}, function (err, result) {
					if (err) throw err;
					console.log('\n更新数据成功，刚更新的文档数是：\n', result);
				});
				
				// 【删】
				collection.remove({name: '小清'}, function (err, result) {
					if (err) throw err;
					console.log('\n删除数据成功，刚删除的文档数是：\n', result);
				
					// 记得最后一次操作要关闭数据库！！！
					// db.close();
				});			
			});
		});	
		Db.on('close', function (err) {
			if (err) throw err;
			console.log('关闭数据库成功');
		})

	在查询操作 collection.find(selector, [options]).toArray(callback) 中可以设置一些限定：
	
		// 查找全部，缺省参数{} 即 db.users.find({});
		$ db.users.find();
		// 指定范围查找
		$ db.users.find({"name": "小清", "email": "abcd@laispace.com"});
		// 查找全部，但只返回指定的键，1表示true， 注意 _id 总会被返回
		$ db.users.find({}, {"name": 1, "email": 1});
		// 查找全部，但不要返回指定的键，0表示false
		$ db.users.find({}, {"password": 0});
		// 大于小于
		$ db.users.find({"age": {"$gte": 18, "$lte": 30}}) // 大于等于18小于等于30岁
		db.users.find({"registered": {"$lt": new Date("01/01/2014")}}); // 在2014/01/01前注册
		// 不等于
		$ db.users.find({"name": {"$ne": "小清"}}); // 用户名不是『小清』
		// 包含于
		db.users.find({"name": {"$in": ["小清", "小赖"]}}); // 用户名是『小清』或『小赖』
		// 不包含于
		db.users.find({"name": {"$nin": ["小清", "小赖"]}}); // 用户名不是『小清』和『小赖』
		// 或
		db.users.find({"$or": [{"name": "小清"}, {"email": "laixiaolai@foxmail.com"}]}); // 用户名是『小清』或 邮箱是 "123@example.com"

	也可在更新操作 collection.update(selector, document, [options], [callback]); 中可以设置一些限定：
	
		// 原子修改器
		db.users.update({"name": "小清"}, {
			"$inc": {
			"age": 1 // 年龄加一
			}
		})
		"$inc" // 增加
		"$set" // 修改，无则创建
		"$unset" // 删除
 
		// 数组修改器
		// "$push" // 添加
		// "$pop": {key: 1} // 数组末删除一个元素
		// "$pop": {key: -1} // 数组头删除一个元素
		// "$pull": {"foo": "bar"} // 删除数组foo中的bar
 
		// 函数 update(query , obj , upsert , multi) 参数说明：
		$ db.users.update({"name": "小清"}, xiaoqing);
		// 若指定第三个参数upsert为true,即：
		$ db.users.update({"name": "小清"}, xiaoqing, true);
		// 则表示：
		// - 若找到匹配文档，正常更新；
		// - 若没有文档符合更新条件，则以这个条件和更新文档为基础建立一个新的文档。
		// 若指定第四个参数multi为true, 即：
		$ db.users.update({"name": "小清"}, xiaoqing, true, true);
		// 则表示：
		// 匹配到的所有文档都得到更新（为false则只匹配第一个）。	

## 使用 Mongoose类库

使用mongoose可以让MongoDB在NodeJS中得到更好的支持。

直接上实例吧：
	
  	var mongoose =require('mongoose');
  	// 通过 Schema 来定义数据架构
  	var Schema = mongoose.Schema;
  	// 连接数据库, 27017 是MongoDB的默认端口
  	mongoose.connect('mongodb://localhost:27017/testDbName', function (err) {
  		if (err) {
  			console.log('连接数据库失败');
  			throw err;
  		}
  		// 定义数据架构
  		var userSchema = new Schema({
  			name: String, // name 为字符串
  			age: Number   // age 为整数
  		});
  		
  		var user1 = {name: '小赖', age: 18};
  		var user2 = {name: '小清', age: 19};
  		var user3 = {name: '大花', age: 20};
  		var user4 = {name: '大黄', age: 21};
  		var docs = [user1, user2, user3, user4];
  		var Users = mongoose.model('users', userSchema);
  		Users.create(docs, function (err, docs) {
  			if (err) {
  				console.log('保存数据失败');
  				throw err;
  			}
  			Users.find(function (err, docs) {
  				if (err) throw err;
  				console.log(docs);
  				// 断开数据库连接
  				mongoose.disconnect();
  			});
  		});
  	})

### 参考资料：

1. [MongoDB入门-CRUD简单操作](http://laispace.github.io/MongoDB%E5%85%A5%E9%97%A8-CRUD%E7%AE%80%E5%8D%95%E6%93%8D%E4%BD%9C%20/)
2. [the-little-mongodb-book-cn](https://github.com/justinyhuang/the-little-mongodb-book-cn/blob/master/mongodb.md)