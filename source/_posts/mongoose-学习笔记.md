title: Mongoose-学习笔记

date: 2014-05-21 18:22:53

categories: Database

tags: 
  - node
  - mongo 
  - mongoose

---

定义模式 Schema -> 定义模型 model -> 定义实例 -> 增查删改这个实例

在模式中可以直接定义一些方法，使得使用模型生成的实例都继承了这些方法，可直接调用。
	
- 快速开始

        var mongoose = require('mongoose');

        // 【模式】
        var UserSchema = mongoose.Schema({
          name: String,
          age: Number
        })

        // 【方法】在模式中添加一个方法，则生成的实例中将可以直接调用这个方法
        UserSchema.methods.say = function () {
          var name = this.name;
          if (name) {
            console.log('我的名字是：', name);
          } else {
            console.log('我还没有名字');
          }
        };

        // 【模型】使用模式定义一个模型
        var User = mongoose.model('User', UserSchema);

        // 【实例】使用模型定义一个实例
        var newUser = new User({
          name: '小赖',
          age: 18  // 我年年十八
        })
        // 【实例】使用模型定义一个实例
        var r = parseInt(Math.random() * 10 + 20);
        var newUserRandom = new User({
          name: '用户'+r,
          age: r  //
        })
        // 实例中调用模式中定义好的方法
        newUser.say(); // => '我的名字是：小赖'
        newUserRandom.say();

        // 【增】将实例保存到数据库
        newUser.save(function (err, newUser) {
          if (err) throw err;
          console.log('【增】保存数据成功, 成功保存的数据是：\n', newUser);
        });
        // 【增】将实例保存到数据库
        newUserRandom.save(function (err, newUser) {
          if (err) throw err;
          console.log('【增】保存数据成功, 成功保存的数据是：\n', newUserRandom);
        });

        // 【查】查询所有文档
        User.find(function (err, users) {
          if (err) throw err;
          console.log('【查】当前数据库的所有用户是：\n', users);
        });
        // 【查】限定条件查询
        User.find({age: 20}, function (err, users) {
          if (err) throw err;
          console.log('查询到年龄为20的用户有：\n', users);
        })

        // 连接数据库
        mongoose.connect('mongodb://localhost/test');

        // 获取连接
        var db = mongoose.connection;

        // 连接错误
        db.on('error', function (err) {
          console.log('连接失败：', err);
        });

        // 连接成功
        db.on('open', function () {
          console.log('连接成功！');
        })
        
<!-- more -->	
                
- 定义模式 Schema
	
		// new Schema(properties, options);

options 中的key为autoIndex/capped/collection/id/_id/read/safe/shardKey/strict/toJSON/toObject/versionKey

模式中的数据类型，可以为：String/Number/Date/Buffer/Boolean/Mixed/ObjectId/Array
        
        var mongoose = require('mongoose');
        var Schema = mongoose.Schema;
		
		// 定义一篇博文的模式
        var articleSchema = new Schema({
          title:  String,
          author: String,
          content:	String,
          comments: [{ author: String, content: String, date: Date }],
          date: { type: Date, default: Date.now },
          meta: {
            votes: Number,
            favs:  Number
          }
        }); 
        
        // 后期添加模式属性
        articleSchema.add({visited: 'string'})
        
        // 在模式中添加静态方法-供模型调用
        articlesSchema.statics.findByAuthor = function (name, callback) {
        	this.find({author: name}, callback)
        };
        
        // 在模式中添加实例方法-供实例调用
        articleSchema.methods.getInfo = function () {
        	console.log('文章标题是：', this.title);
        	console.log('文章作者是：', this.author);
        	// ... 
        };
        
        // 在模式中添加虚拟化方法-供实例调用
        // 相当于实例中会存在一个新的description属性: 
        articleSchema.virtual('description').get(function () {
        	return '作者'+ this.author + '写了一篇名为『' + this.title + '』的文章';
        })
        
        
- 定义模型 model

使用模式定义模型
	
		var Article = mongoose.model('Article', articleSchema);
		
		// 调用静态方法
		Article.findByAuthor('小赖', function (err, articles) {
			if (err) throw err;
			console.log('找到所有作者为小赖的文章：', articles);
		})
		
- 生成实例

使用模型生成实例，实例中可调用模式中定义好的方法

		var newArticle = new Article({
			title: 'hello Mongoose',
			author: '小赖',
			... 
		})	
		
		// 调用实例方法
		newArticle.getInfo();
		
		// 调用虚拟化方法
		var desc = newArticle.description;
		console.log(desc); // => '小赖写了一篇名为『hello Mongoose』的文章'

- 增查删改数据操作
		
		// 【增】
		newArticle.save(function (err, newArticle) {
			if (err) throw err;
			console.log('保存数据成功, 成功保存的数据是：\n', newArticle);
		})
		// 想当于：
		// var newArc = {
		// 	 title: 'hello Mongoose',
		//	 author: '小赖',
		//	 ... 
		//}
		//Article.create(newArc, function (err, newArc) {
		//	 if (err) throw err;
		//	 console.log('保存数据成功, 成功保存的数据是：\n', newArc);
		//})
		
		// 【删】
		Article.remove({author: '小赖'}, function (err) {
			if (err) throw err;
			console.log('删除数据成功');
		})
		
		// 【查】
		Article.find({ahthor: '小赖'}, function (err, articles) {
			if (err) throw err;
			console.log('小赖写的文章有：', articles);
		})
		// 更多查询方法，见 http://mongoosejs.com/docs/api.html#model_Model.find
		// Model.find(conditions, [fields], [options], [callback])
		// Model.findById(id, [fields], [options], [callback])
		// Model.findByIdAndRemove(id, [options], [callback])
		// Model.findByIdAndUpdate(id, [update], [options], [callback])
		// Model.findOne(conditions, [fields], [options], [callback])
		// Model.findOneAndRemove(conditions, [options], [callback])
		// Model.findOneAndUpdate([conditions], [update], [options], [callback])
		


 注意：静态方法和实例方法都定义在模式上，但前者由模型调用，后者由实例调用。		
	
### 参考资料

- [Mongoo manual](http://docs.mongodb.org/manual/)

- [Mongoose guide](http://mongoosejs.com/docs/guide.html)

- [Mongoose aip](http://mongoosejs.com/docs/api.html)

 
       
       
