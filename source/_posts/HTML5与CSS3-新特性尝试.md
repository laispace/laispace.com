title: HTML5与CSS3-新特性尝试
tags:
  - flex
id: 621
categories:
  - HTML
date: 2014-03-07 22:09:57
---

HTML5一些特性不断在变化，一年前的语法跟现在的已经大有不同，像用来布局的 flex 属性也有新旧版本的语法了。

处理兼容性真是个蛋疼的事情 。

这里放一些闲时做的DEMO吧（含CSS3），测试在chrome下，主要目的是了解基本用法让自己一目了然，在项目需要的时候再加上兼容方案吧：）

- Flex 多栏响应式布局

1.设置父容器为 display 属性为 'flex'

2.设置子容器的 width、flex 和 order 属性
<iframe src="http://jsfiddle.net/laiqs2011/SYVxL/2/embedded/result,js,html,css/" frameborder="0" width="100%" height="300"></iframe>

&nbsp;

- CSS3 文字从上到下

直接使用 css3 中的属性 transform：
<iframe width="100%" height="300" src="http://jsfiddle.net/p6hkE/1/embedded/result,js,html,css/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

&nbsp;

&nbsp;

&nbsp;

- XDM 跨文档消息传递（即跨域通信）
## 发送消息

[javascript]

// 获取内嵌框架
var iframeWindow = document.getElementById(‘myFrame’).contentWindow;
// 向内嵌框架发送消息
iframeWindow.postMessage(‘你好 赖小赖’, ‘http://laispace.com');

[/javascript]

## 接受消息

[javascript]
 // message 事件是异步的
 window.onmessage = function (event) {
 // 确保消息源是已知域 event.origin
 if (event.origin == ‘http://laispace.com') {
 // 处理接收到的消息 event.data
 console.log(event.data);
 // 向消息源发送回执(event.source是消息源的window对象的代理)
 event.source.postMessage('消息已收到’, ‘http://www.消息源.com');

}
 };

[/javascript]

[DEMO](http://html5demos.com/postmessage2) or [官方文档](http://dev.w3.org/html5/postmsg/)