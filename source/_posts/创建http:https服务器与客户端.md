title: 创建HTTP/HTTPS服务器与客户端

date: 2014-05-12 09:36:45

categories: Node

tags: [HTTP, HTTPS] 

---

- 创建HTTP服务器

	方法1：
	
		var server = http.createServer([requestListener])
		// requestListener = function (request, response) {
			// request 是一个 http.IncomingMessage对象
			// response 是一个 http.ServerResponse对象
		// }
	
	方法2：
	
		var server = http.createServer();
		// 监听请求
		server.on('request', function (request, response) {
			// callback code here.
		})		
		// 监听端口
		// port 若为0则分配随机端口号
		// host 缺省则监听来自任何ipv4地址的客户端连接
		// backlog 默认为511，设定等待队列中最大的连接数，超过则拒绝
		server.listen(port, [host], [backlog], [callback])
		
		// 或
		server.on('listening', function (request, response) {
			// callback code here.
		})
		
<!--more-->
		
	示例：
	
		var http = require('http');
		var server = http.createServer(function (req, res) {
			// console.log('客户端请求信息为：', req);
			console.log('客户端请求方法为：', req.method);
			console.log('客户端请求url为：', req.url);
			console.log('客户端请求头为：', req.headers);
			console.log('客户端请求HTTP版本为：', req.httpVersion);
			
			// 监听客户端发来的数据
			req.on('data', function (data) {
				console.log('服务器接收到数据：', data);
			});
			req.on('end', function () {
				console.log(				'服务器接收数据完毕');
			})
			
			// 设置超时
			res.setTimeout(1000);
			res.on('timeout', function () {
				console.log('服务器响应超时');
			})
			// 监听关闭
			res.on('close', function () {
				console.log('连接被中断');
			})
			
			
			// 发送服务器端的响应数据
			// res.writeHead(statusCode, [reasonPhase], [headers])
			// starusCode 为HTTP状态码
			// reasonPhase 为状态码的描述信息
			// headers 指定响应头对象，或使用 res.setHeader(name, value) 单独设置
			res.writeHead(200, {'Content-Type': 'text/plain'});
			res.write('hello 赖小赖！\n');
			res.write('hello 我是继续写入的赖小赖！\n');
			res.write('hello 我是调皮的赖小赖！\n');
			// 必须使用 end() 结束响应
			res.end('好吧，我不玩了，掰掰！', function () {
				// console.log('已结束响应');
			});
		});
		// 监听端口
		server.listen(1234, 'localhost', function () {
			console.log('服务器正在监听端口1234');
	
			// 关闭服务器
			// server.close();
		});
		// 设置超时, 默认为2分钟，这里设定为1分钟
		server.setTimeout(60*1000, function (socket) {
			// 超时后执行回调
			// console.log('服务器超时：', socket);
		})
		
		// 监听连接
		server.on('connection', function (socket) {
			console.log('客户端连接已建立');
		})
		
		
		// 监听关闭
		server.on('close', function () {
			console.log('服务器已被关闭');
		});
		// 监听错误
		server.on('error', function(e) {
			// 端口被占用
			if (e.code == 'EADDRINUSE') {
				console.log('error: 端口已被占用');
			}
		});
		
		
	使用 querystring模块 处理查询字符串

		- querystring.parse() 处理查询字符串
	
		// str为查询字符串
		// sep 设定查询字符串中的分隔符，默认为 &
		// eq 设定查询字符串中的分配符，默认为 = 
		// querystring.parse(str, [sep], [eq], [options])
		var str = 'name=xiaolai&age=18&sex=male';
		querystring.parse(str);
		// 将输出 {
		//	name: 'xiaolai',
		//	age: 18,
		//	sex: 'male'
		// }
		
		- querystring.stringify() 转换对象为查询字符串
		// querystring.stringify(obj, [sep], [eq])
		var obj = {
			name: 'xiaolai',
			age: 18,	// 好吧 就让我再年轻一次！！！
			sex: 'male'
		};
		var str = querystring.stringify(obj);
		// 将输出 name=xiaolai&age=18&sex=male
		
	使用 url模块 处理完整的URL字符串
		
		- url.parse() 处理字符串
		
		// parseQueryString 默认为false, 为true时，将字符串中的查询字符串转化为对象
		// url.parse(str, [parseQueryString])
		var str = 'http://user:pwd@laispace.com:80/user/xiaolai?age=18&sex=male#section1';
		var strObj1 = url.parse(str);
		var strObj2 = url.parse(str, true);
		console.log(strObj1, strObj2);
		// strObj1.query 为 'age=18&sex=male'
		// strObj2.query 为 {age: '18', sex: 'male'}
		
		- url.format() 转换对象为字符串
		
		var str = url.format(strObj1);
		console.log(str);
		
		- url.resolve() 合并路径

		// url.resolve(form, to);
		var str = url.resolve('http://laispace.com', 'user/xiaolai')
		console.log(str)/ =>'http://laispace.com/user/xiaolai'
		

- 创建HTTP客户端

		// options 为对象，若为地址字符串则自动parse为对象
		// options.host 指定IP地址，默认为 localhost
		// options.hostname 默认为 localhost
		// options.port 指定端口
		// localAddress 指定专用于网络连接的本地端口
		// socketPath 指定目标Unix域端口
		// method 指定HTTP请求方法，默认为 GET
		// path 指定请求路径和查询字符串
		// headers 指定请求头对象
		// auth 指定认证信息，如 "user: password"
		// agent 指定代理，是一个 http.Agent对象	

		var req = http.request(options, function (response) {
			// response code here.
		})	
		// 发送数据
		req.write(chunk, [encoding]);
		// 结束请求
		req.end([chunk], [encoding]);
		
	示例：
	
		var http = require('http');
		var options = {
			hostname: 'www.laispace.com',
			post: 80,
			path: '/',
			method: 'GET'
		};
		var req = http.request(options, function (res) {
			// console.log('响应信息：', res);
			console.log('响应状态码：', res.statusCode);
			console.log('响应头：');
			// 设定编码
			res.setEncoding('utf8');
			var body = '';
			// 监听响应数据
			res.on('data', function (chunk) {
				console.log('接收到响应数据：', chunk);
				body += chunk;
			})
			res.on('end', function () {
				// console.log('响应数据已全部接受：', body);
			})
		})
		// 写入请求数据
		// req.write('Hello 我是小赖');
		// 发起请求
		req.end();
		
		// 监听错误，如访问一个不存在的地址时
		req.on('error', function (err) {
			if (err.code === 'ECONNRESET') {
				console.log('socket 端口超时')
			} else {
				console.log('请求发生错误：', err);
			}
			
		});
		// 设定超时
		req.setTimeout(1000, function () {
			// 终止请求
			// req.abort()		
		})	
	
	除了使用 http.request(options, callback) 外，
	
	也可以使用简化的 http.get(options, callback) ，
	
	其默认使用 GET 并会自动调用 end() 方法发起请求
	

- 创建HTTP代理服务器	

		var http = require('http');
		var url = require('url');
		// 建立代理服务器
		var server = http.createServer(function (clientReq, clientRes) {
			var url_parts = url.parse(clientReq.url);
			var options = {
				host: 'www.laispace.com', // =>真正访问的网站host
				port: 80,
				path: url_parts.pathname,
				headers: clientReq.headers
			};
			// 服务器代理客户端发起请求
			var serverReq = http.get(options, function (serverRes) {
				// 代理服务器得到的响应返回给客户端
				clientRes.writeHead(serverRes.statusCode, serverRes.headers);
				// 代理服务器请求到的数据返回给客户端
				serverRes.pipe(clientRes);
			});			
			// 将客户端请求
			clientReq.pipe(serverReq);
		});
		server.listen(1234, 'localhost');
		
	保存这段代码到 test.js
	
	执行 $ node test.js
	
	浏览器访问 http://localhost:1234 则会访问到 www.laispace.com 
	
- 创建HTTPS服务器	

	HTTPS 相比于 HTTP：
		
		- HTTPS服务器向CA申请证书
		
		- HTTPS传输的是经过SSL加密后的数据
		
		- HTTPS常用443端口，而HTTP常用80端口
		
	SSL简单介绍：
		
		- 私钥和公钥保存在服务器
		
		- 公钥发送到客户端，客户端端发送 消息msg1 给服务器
		
		- 服务器将 消息msg1 进行哈希运算得到 hash1字符串 并用私钥加密后发送 消息msg2 传回客户端
		
		- 客户端使用公钥解密 消息msg2 ，将 消息msg1 进行哈希运算得到 hash2字符串，与hash1字符串 进行比较
		
		- 若 hash1 与 hash2 相等则握手成功
		
		- 客户端选择加密算法和相应密钥，用公钥加密后发送给服务器
		
		- 服务器收到加密算法和响应密钥，开始与客户端传输数据		
	1. 创建私钥：
	
			$ openssl genrsa -out privatekey.pem 1024
	
	2. 创建证书签名请求(Certificate Signing Request)文件：
	
			$ openssl req -new -key privatekey.pem -out certrequest.csr

	3. 获取证书：（这里是使用 openssl 创建的测试用的证书-访问网站会被警告，真正的证书要向CA申请）
		
			$ openssl x509 -req -in certrequest.csr -signkey privatekey.pem -out certificate.pem
	
	4. 创建pfx文件（为存储私钥、公钥和证书的一种格式）：
	
			$ openssl pkcs12 -export -in certificate.pem -inkey privatekey.pem -out certificate.pfx
			
	具备以上条件后，使用 https.createServer(options, [requestListener]) 创建HTTPS服务器：
	
		var https = require('https');
		var fs = require('fs');

		var pk = fs.readFileSync('./privatekey.pem');
		var pc = fs.readFileSync('./certificate.pem');

		var options = {
			key: pk,
			cert: pc
		};
		var server = https.createServer(options, function (req, res) {
			res.write('Hello 赖小赖！');
			res.end();
		});
		server.listen(1234, 'localhost', function () {
			console.log('HTTPS服务器已开启，正在监听端口1234');
		})

- 创建HTTPS客户端

		var https = require('https');

		var options = {
			hostname: 'npmjs.org',
			port: 443,
			path: '/',
			method: 'GET',
			agent: false // 设置为false表示自动选择代理
		};
		var req = https.request(options, function (res) {
			console.log('响应状态码：', res.statusCode);
			console.log('响应头：', res.headers);
			// 接收数据
			res.on('data', function (chunk) {
				console.log('响应内容：', chunk.toString());
			});
		});
		// 发送请求
		req.end();
		// 设置超时
		req.setTimeout(1000, function () {
			console.log('连接超时');
			// 终止请求
			req.abort();
		})
		req.on('error', function (err) {
			console.log('出错啦！')
		})

	除以上提出的区别外，HTTPS服务器的编写（如错误监听、设置超时、关闭服务器等）与HTTP服务器的编写方法基本相同，详见官方文档。		
	
	默认是GET方法时，也可以使用 https.get(options, callback)，自动调用 end() 方法发起请求，这时HTTPS服务器编写也与HTTP服务器的编写方式相同，参见上文。

						
		
	
														




### 参考资料：

1. [SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)


		
	
		
		
		
	
				
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		

					