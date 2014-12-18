title: 使用net模块创建TCP服务器

date: 2014-05-11 12:43:20

categories: Node

tags: [net, tcp] 

---

- 创建TCP服务器

		var server = net.createServer([options], [connectionListener])
		// 相当于: 
		// var server = net.createServer([options]);
		// server.on('connection', connectionListener);
	
	方法1：		
	
		// 监听端口
		// port 若为0则分配随机端口号
		// host 缺省则监听来自任何ipv4地址的客户端连接
		// backlog 默认为511，设定等待队列中最大的连接数，超过则拒绝
		server.listen(port, [host], [backlog], [callback])
		
	方法2：	
		
		// 监听指定路径
		server.listen(path, [callback])
		
	方法3：
	
		// 监听socket句柄
		server.listen(handle, [callback])
		
	以上三种方法的 callback 可改写为：
	
		server.on('listening', function () {
			// callback code here
		})	
				
<!--more-->
		
	示例：
	
		var net = require('net');
		var server = net.createServer(function(socket) {
			console.log('客户端与服务器的连接已建立');
			
			console.log('socket信息是：', socket.address())
			
			// 获取客户端与服务器的连接数
			server.getConnections(function(err, count) {
				console.log('当前连接数为：', count);
				// 设置最大连接数，超过这个连接数后，客户端将无法得到响应
				server.maxConnections = 2;
				console.log('最大连接数为：', server.maxConnections);
			})
			
			// 关闭服务器，不再接收所有连接
			// server.close(function() {
			//	console.log('服务器已关闭');
			// })
			
		});
		server.listen(1234, 'localhost', function() {
			console.log('正在监听端口1234');
			console.log('server信息是：', server.address())
		});
		server.on('error', function(e) {
			// 端口被占用
			if (e.code == 'EADDRINUSE') {
				console.log('error: 端口已被占用');
			}
			
		})
		
	
	处理socket连接

		var net =require('net');
		var server = net.createServer();
		server.on('connection', function(socket) {
			console.log('socket信息是：', socket.address());

			// 监听socket连接
			socket.on('data', function(data) {
				console.log('共接收到%d字节的数据：', socket.bytesRead);
				// data 默认是 buffer 流
				// 设定编码
				// socket.setEncoding('utf8');
				// 或使用 data.toString()
				console.log(data.toString());
			});

			// 监听关闭连接
			socket.on('end', function () {
				console.log('连接被客户端关闭！');
			})
			//向客户端发送数据
			socket.write('哈喽！这是来自服务器的数据！')
		});

		server.listen(1234, 'localhost');


	将这段脚本存到 server.js 启动这段脚本 
	
		$ node server.js
	
	运行telnet并输入任意数据试试：
		
		telnet localhost 1234

		

- 创建TCP客户端

		
		var socket = new net.Socket([options])
		// options.fd 文佳描述符
		// options.type 可选tcp4/tcp6/unix 指定协议
		

	方法1：
	
		socket.on('connect', function(){
			// callback code here.
		})

	方法2：		

		sockec.connect(path, [connectionListener])
		
		socket.remoteAddress 远程地址
		socket.remotePort 远程端口
		socket.localAddress 本地地址
		localPort 本地端口
		
	写入数据：
	
		socket.write(data, [encoding], [callback])
		
	示例：
	

		var net = require('net');
		var client = new net.Socket();
		client.setEncoding('utf8');

		client.connect(1234, 'localhost', function(){
			console.log('客户端已连接到服务器');
			// 向服务器发送数据
			client.write('你好，我是来自客户端的消息！');
			console.log('已发送%d字节的数据', client.bytesWritten);
			// 关闭连接
			// client.end([data], [encoding])
			// client.end('客户端已关闭连接！')
		});

		// 监听数据接收
		client.on('data', function (data) {
			console.log('客户端接收到来自服务器的数据：', data);
		});

		// 监听错误
		client.on('error', function (e) {
			console.log('error: ', e);
			// 销毁这个错误的socket，确保不会被使用
			client.destroy();
		})

	
	将这段代码另存为 client.js
	
	先启动上面的 server.js
	
		$ node server.js
		
	再新建终端窗口启动 client.js 进行通信
		
		$ node client.js
		
		
- net模块判断IP地址

		// 判断输入是否为IP
		var type = net.isIP(ip);	
		switch (type) {
			case 0: 
				console.log('不是一个IP');
				break;
			case 4: 
				console.log('是一个IPV4地址');
				break;
			case 6: 
				console.log('是一个IPV6地址');
		}
		
		// 判断是否为 IPV4地址
		net.isIPV4(ip)
		
		// 判断是否为 IPV6地址
		net.isIPV6(ip)
		
- dgram 模块实现UDP通信

	服务器代码 udpServer.js ：
	
			var dgram = require('dgram');
			var server = dgram.createSocket('udp4');
			server.on('message', function (msg, info) {
				console.log('收到客户端信息：', msg);
				console.log('客户端地址信息：', info);
				// 往客户端发送信息
				var buff = new Buffer('已收到这条信息：', msg);
				server.send(buf, 0, buf.length, info.port, info.address);
			})	
			// 监听
			server.on('listening', function () {
				console.log('正在监听：', server.address());
			});
			// 绑定端口
			server.bind(12345, 'localhost');
			
	客户端代码 udpClient.js：
	
			var dgram = require('dgram');
			var message = new Buffer('哈喽！这是来自客户端的消息，我是小赖呦');
			var client = dgram.createSocket('udp4');
			// 发送消息
			client.send(message, 0, message.length, 12345, 'localhost', function (err, bytes) {
				if (err) {
					console.log('发送失败');
				}
				console.log('已发送%d字节的数据', butes);
			});			
			// 监听
			client.on('message', function (msg, info) {
				console.log('收到服务器信息：', msg);
				console.log('服务器地址信息：', info);
			})
			
		
					
				
		
		
		
	
	
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		