title: express提供的中间件
date: 2014-05-19 10:26:54
categories: Node
tags: 
    - express
    - middleware

---

## Express 框架中常用的一些中间件的使用方法

- basiAuth 访问控制

		// 帐号密码正确时才发回 true，才能继续访问
		var express =  require('express');
		var app = express();
		// 这里的用户名和密码应从数据库读取
		app.use(express.basicAuth('username', 'password'));
		app.get('/', function (req, res) {
			res. send('成功登录后才会看到这段内容。');
		});
		app.listen(1234, 'localhost');

- bodyParse 处理请求 body 的内容

		// 内部使用 JSON	编码、url 编码处理和文件的上传处理
		// 处理一个上传文件
        <body>
          <h1>使用 express.bodyParser 中间件上传文件</h1>
          <form id="myForm" action="upload.html" method="post" enctype="multipart/form-data">
            <input type="file" id="file" name="file">
            <input type="submit" onclick="uploadFile()" value="上传">
          </form>
          <div id="result">
            选择文件后，点击按钮上传.
          </div>
        </body>

		
    	// 帐号密码正确时才发回 true，才能继续访问
    	var express =  require('express');
        var connect = require('connect');
        var fs = require('fs');
    		var app = express();
        // 使用 bodyParser 处理上传
        // app.use(express.bodyParser());
        app.use(connect.urlencoded());
        app.use(connect.json());
        app.get('/upload.html', function (req, res) {
          res.sendFile(__dirname + '/upload.html');
        });
        // 开始处理
        app.post('/upload.html', function (req, res) {
          var file = req.files.myFile;
          // 读取文件
          fs.readFile(file.path, function (err, data) {
            if (err) throw err; //读取文件失败
            fs.writeFile(file.name, data, function (err) {
              if (err) throw err; // 写入文件失败
              res.send('文件上传成功！');
            })
          })
        })
    	app.listen(1234, 'localhost');
		
		
	 
- compress 压缩响应流数据

		// 在其他中间前调用才能保证全部数据流都压缩
		
- cookieParser 处理 cookie

        var express = require('express');
        var app = express();
        // 使用 cookieParser 处理 cookie
        app.use(express.cookieParser());
        // 提前在 cookie.html 中埋下 cookie
        app.get('/cookie.html', function (req, res) {
            res.sendFile(__dirname + '/cookie.html');
        });
        app.post('/cookie.html', function (req, res) {
          var cookies = req.cookies;
          // 开始处理
          for (var key in cookies) {
            res.write('名：', key);
            res.write('值：', cookies[key]);
            res.write('</br>');
          }
          res.end();
        });
        app.listen(1234, 'localhost');		
		
- csrf 防止跨站访问

		// 与 session 中间件 和 bodyParser 中间件配合使用
		
- directory 列出某目录下的文件列表

		// app.use(express.directory(path, [options]))
		// 与 express.static 配合使用
		app.use(express.static(__dirname));
		app.use(express.directory(__dirname), {icons: true}); // 显示文件icon
		// 此时会列出文件目录，且点击静态文件（如js/css）可直接访问


- errorHandler 捕获错误

- limit 限制请求提交数据的字节数

		// 限制为 1M，超出则报错
		var size = 1024*1024; // 1M
		app.use(express.limit(size))


- logger 输出日志到文件中

		// app.use(express.logger([options]))
		// options = {
		//	immediate: false, // 是否在接收到客户端请求时就输出日志,否则服务器端发送完响应数据才输出
		//	format: 'default', // 可选 default/short/tiny/dev，即输出日志的格式		//  stream: process.stdout, // 指定输出数据流的对象
		// 	buffer: undefined, // 整数毫秒则指定缓存区有效时间啊暖，或为 true 时，使用缓存区且有效时间为 1000ms
		// }
		var express = require('express');
		var app = express();
		app.use(express.logger({
			format: 'default',
			stream: process.stdout
		}))
		


- methodOverride 为 bodyParser 提供 HTTP 支持

- reponseTime 在响应头添加 X-Request-Time 字段

		// 使用后可在 浏览器控制台查看到响应头多了一个 X-Request-Time 
		app.use(express.responseTime());
		

- router 提供路由功能
		
		// express 3.x 隐式使用了 router 
		
- session 提供session功能

		// session 加密过的数据保存在 cookie 中
		app.use(express.session([options]))
		options = {
			key: connect.sid, // 字符串，指定保存这个 session 的 cookie 名
			store: , // 保存 session 的第三方存储对象
			fingerprint: function () {}, // 一个自定义指纹生成函数
			cookie: {path: '/', httpOnly: true, maxAge: 14400000}, // 指定保存 session 的设置 cookie 的对象
			secret: 'xiaolai', // 字符串，用于 session 的数据加密（加盐）
		}
		
		// 使用这个中间件后，就有了 req.session 属性
		var express = require('express');
		var app = express();
		app.use(express.cookieParser());
		app.use(express.session({secret: 'xiaolai'}));
		app.get('/', function (req, res) {
			res.sendFile(__dianema + '/index.html');
			// 开始设置
			req.session.username = 'xiaolai';
			req.session.password = 'password';
			
			// 重新生成
			// req.session.regenerate(function (err) {});
			
			// 销毁
			// req.session.destroy(function (err) {});
		})

- static 设置访问静态文件的功能

		// public 为存放静态文件的目录
		app.use(express.static('public'))
		
- 自定义错误处理中间件

		app.use(function (err, req, res, next) {
			// 输出错误到服务端
			console.log(err.stack);
			next();
		})	
		app.use(function (err, req, res, next) {
			// 输出错误到客户端
			res.send(500, err.message);
		})	

<!-- more -->	

## Express 框架中常用的一些配置的使用方法

		// env 指定环境， callback为无参数的回调
		// app.configure([env], callback)	

		// 设置
		app.set(name, value);
		// 将 布尔类型的内部变量设置为 true
		app.enable(name);
		// 或 false
		
		app.disable(name);
		// 判断是否为 true
		var isTure = app.enabled(name)
		// 或 false
		var isFalse = app.disabled(name)
		
		// 获取设置
		var value = app.get(name);
		

- 所有环境下的配置

		app.configure(function () {
			app.set('title', 'My title');
			// 模板文件存放的目录
			app.set('views', __dirname + '/views');
			// 模板引擎
			app.set('view engine', 'ejs');
			// 使用一些中间件
			app.use(express.bodyParser());
			app.use(express.cookieParser());
			app.use(express.static(__dirname + '/public'));
			app.use(app.router);
		});	
		
		// 相当于：
		app.set('title', 'My title');
		// ...
		
- 针对开发环境的配置		
	
		app.configure('development', function () {
			app.set('db uri', 'localhost/dev');
		});
		
		// 相当于：
		if (app.get('env') === 'development') {
			app.set('db uri', 'localhost/dev');
		}

- 针对生产环境的配置

		app.configure('production', function () {
			app.set('db uri', '222.201.132.xxx/prod');
		})	
		
		// 相当于：
		if (app.get('env') === 'production') {
			app.set('db uri', '222.201.132.xxx/prod');
		}
		
	







		
		
		
		
 