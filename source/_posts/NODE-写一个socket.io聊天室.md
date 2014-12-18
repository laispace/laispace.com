title: NODE-写一个socket.io聊天室
tags:
  - socket
categories:
  - Node
date: 2013-12-02 19:16:51
---

前几天接触了WebSocket，感觉到了socket通信的强大，见《[HTML5-WebSocket API 学习](http://www.laispace.com/?p=532)》。

小赖决定自己动手写一个基于nodejs+express+socket.io的聊天室应用，用来做node入门的小项目吧。

项目地址戳[这里](https://github.com/laispace/laiChat)。

今天实现的部分是：

- 客户端与服务器通信

- 多个客户端同时通信

- 保存聊天记录和在线用户

安装方法：

1.  下载到本地，安装需要的模块：$ npm install
2.  打开服务器：$ node app.js
3.  打开多个浏览器页面，分别输入昵称
4.  可以开始聊天啦！
这算是我试水学习node的第一个项目，代码托管到github上，慢慢捣鼓出一些东西来！