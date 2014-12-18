title: 'HTML5-WebSocket API 学习'
tags:
  - html5
  - socket
  - websocket
id: 532
categories:
  - HTML
date: 2013-11-25 18:44:27
---

使用nodejs的socket.io和现代浏览器的WebSocket来建立一个聊天室。

# 服务器端 编写serverfile.js文件，建立http服务器和socket连接：

[javascript]
 var http = require('http');
 var io = require('socket.io');

 // 创建一个服务器
 var server = http.createServer(function(request, response){
     response.writeHead(200, {'Content-type': 'text/html'});
     response.end('小赖的WebSocket服务器启动啦！');
 });
 // 监听端口
 server.listen(9999);

 // 创建一个WebSocket
 var socket = io.listen(server).set('log', 1);

 // 监听连接
 server.on('connection', function(client) {
     // 监听信息
     client.on('message', function(data){
         console.log('收到客户端发来信息：', data);
         var curTime = new Date().getTime();
         client.emit('服务器返回信息：', data + '-&gt;' + curTime);

         client.on('disconnect', function(){
             console.log('连接已断开');
         });
     });
 });
 [/javascript]

创建http服务器，运行http服务器成功：

# 浏览器端 编写 webSocket.html ，建立与服务器的连接：

[html]
 &lt;!DOCTYPE html&gt;
 &lt;html&gt;
 &lt;head&gt;
     &lt;title&gt;WebSocket API&lt;/title&gt;
     &lt;meta charset=&quot;utf-8&quot;&gt;
     &lt;script src=&quot;&lt;span style=&quot;text-decoration: underline;&quot;&gt;http://localhost:9999/socket.io/socket.io.js&lt;/span&gt;&quot;&gt;&lt;/script&gt;
 &lt;/head&gt;
 &lt;body&gt;
     &lt;div id=&quot;log&quot;&gt;显示log信息...&lt;/div&gt;
     &lt;input id=&quot;msg&quot; type=&quot;text&quot; placeholder=&quot;请输入信息&quot; /&gt;
     &lt;button id=&quot;send-btn&quot;&gt;发送！&lt;/button&gt;

 &lt;script&gt;
     var myWebSocket = {};
     myWebSocket.socketio = {
         mysocket: null,
         initialize: function(){
             // 建立连接
             myWebSocket.socketio.mysocket = io.connect('&lt;span style=&quot;text-decoration: underline;&quot;&gt;http://localhost:9999&lt;/span&gt;');
             // 监听连接
             myWebSocket.socketio.mysocket.on('connect', function(){
                 myWebSocket.socketio.log('成功连接到服务器\n');
             });
             // 监听信息
             myWebSocket.socketio.mysocket.on('message', function(data){
                 myWebSocket.socketio.log('服务器返回数据：' + data + '\n');
             });
             // 监听断开连接
             myWebSocket.socketio.mysocket.on('disconnect', function(){
                 myWebSocket.socketio.log('已断开连接\n');
             })

             // 点击发送按钮
             document.querySelector('#send-btn').onclick = function(){
                 // 发送信息到服务器
                 var msg = document.querySelector('#msg').value;
                 myWebSocket.socketio.sendMessageToServer(msg);
                 document.querySelector('#msg').value = '';
             };
         },
         sendMessageToServer: function(data){
             myWebSocket.socketio.mysocket.secd(data);
             myWebSocket.socketio.log('已发送信息到服务器：' + data +'\n');
         },
         log: function(msg) {
             document.querySelector('#log').innerHTML += msg;
         }
     }

     myWebSocket.socketio.initialize();
     &lt;/script&gt;

 &lt;/body&gt;
 &lt;/html&gt;
 [/html]

然后在浏览器打开这个网页，终端显示：

![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-1.png)

这样客户端和服务器就可以通信啦！

输入信息后，点击发送，终端显示：

![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-2.png)

浏览器则显示：

![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-3.png)

然后关闭服务器的话，则显示「已断开连接」：

![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-4.png)

实现广播功能，即一个客户端发送消息，所以和服务器建立了连接的其他客户端都能看到这个消息：
修改serverfile.js文件，注意有『修改』字眼的部分：

[javascript]
 var http = require('http');
 var io = require('&lt;span style=&quot;text-decoration: underline;&quot;&gt;socket.io&lt;/span&gt;');

 // 创建一个服务器
 var server = http.createServer(function(request, response){
     response.writeHead(200, {'Content-type': 'text/html'});
     response.end('小赖的WebSocket服务器启动啦！');
 });
 // 监听端口
 server.listen(9999);

 // 创建一个WebSocket

 var socket = io.listen(server).set('log', 1);

 // 监听连接
 socket.on('connection', function(client) {
     // 修改：监听信息
     client.on('customMessage', function(data){
         console.log('收到客户端发来信息：', data);
         var curTime = new Date().getTime();

         // 修改：使用广播方法
         client.broadcast.emit('服务器返回customMessage信息：', data + '广播-&gt;' + curTime);

         client.on('disconnect', function(){
             console.log('连接已断开');
         });
     });
 });
 [/javascript]

修改webSocket.html文件，注意有『修改』字眼的部分：

[html]
 &lt;!DOCTYPE html&gt;
 &lt;html&gt;
 &lt;head&gt;
     &lt;title&gt;WebSocket API&lt;/title&gt;
     &lt;meta charset=&quot;utf-8&quot;&gt;
     &lt;script src=&quot;&lt;span style=&quot;text-decoration: underline;&quot;&gt;http://localhost:9999/socket.io/socket.io.js&lt;/span&gt;&quot;&gt;&lt;/script&gt;
 &lt;/head&gt;
 &lt;body&gt;
     &lt;div id=&quot;log&quot;&gt;显示log信息...&lt;/div&gt;
     &lt;input id=&quot;msg&quot; type=&quot;text&quot; placeholder=&quot;请输入信息&quot; /&gt;
     &lt;button id=&quot;send-btn&quot;&gt;发送！&lt;/button&gt;

 &lt;script&gt;
     var myWebSocket = {};
     myWebSocket.socketio = {
         mysocket: null,
         initialize: function(){
             // 建立连接
             myWebSocket.socketio.mysocket = io.connect('&lt;span style=&quot;text-decoration: underline;&quot;&gt;http://localhost:9999&lt;/span&gt;');
             // 监听连接
             myWebSocket.socketio.mysocket.on('connect', function(){
                 myWebSocket.socketio.log('成功连接到服务器&lt;br /&gt;');
             });
             //修改： 监听customMessage信息
             myWebSocket.socketio.mysocket.on('broadcastMessage', function(data){
                 myWebSocket.socketio.log('收到广播信息：' + data + '&lt;br /&gt;');
             });
             // 监听断开连接
             myWebSocket.socketio.mysocket.on('disconnect', function(){
                 myWebSocket.socketio.log('已断开连接\n');
             })

             // 点击发送按钮
             document.querySelector('#send-btn').onclick = function(){
                 // 发送信息到服务器
                 var msg = document.querySelector('#msg').value;
                 myWebSocket.socketio.sendMessageToServer(msg);
                 document.querySelector('#msg').value = '';
             };
         },
         sendMessageToServer: function(data){
             // 修改
             myWebSocket.socketio.mysocket.emit('customMessage', data);
             myWebSocket.socketio.log('已发送信息到服务器：' + data +'&lt;br /&gt;');
         },
         log: function(msg) {
             document.querySelector('#log').innerHTML += msg;
         }
     }

     myWebSocket.socketio.initialize();
     &lt;/script&gt;

 &lt;/body&gt;
 &lt;/html&gt;
 [/html]

然后用多个页面打开webSocket.html,在第一个页面输入消息：

![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-5.png)

点击发送，其他页面立即收到了消息：
![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-6.png)

在第二个页面输入信息：
![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-7.png)
在其他页面收到广播：

![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-8.png)
哇！WebSocket够强大，可以实现客户端和服务端的通信，而node的socket.io更是封装了它的一系列方法，实现一个web端通信就轻而易举了，但！是！HTML5的这个新特性，你敢用嘛？！

# 浏览器对WebSocket的支持性
在caniuse.com查询可知，WebSocket在IE10+和其他现代浏览器才支持，低版本的IE不支持WebSocket  -  -！
![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-10.png)
不过好消息是，socket.io对不支持WebSocket的浏览器启用了其他策略，使得socket.io甚至能在IE6下运行！
![](http://laispace.u.qiniudn.com/HTML5-WebSocket%20API%20%E5%AD%A6%E4%B9%A0-11.png)

学好node后再回来拓展这个小小聊天室呗^_^
[参考资料](<span style="text-decoration: underline;">http://www.ibm.com/developerworks/cn/web/1112_huangxa_websocket/</span> )
[参考资料](<span style="text-decoration: underline;">http://developer.51cto.com/art/201308/407192_all.htm</span> )
[参考资料](<span style="text-decoration: underline;">http://socket.io/</span> )