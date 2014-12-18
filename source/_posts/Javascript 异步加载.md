title: 'Javascript 异步加载'
tags:
  - async
id: 498
categories:
  - Javascript
date: 2013-10-04 19:05:32
---

异步编程，即非阻塞地执行代码，其实可以用来加载一些附属功能的代码，比如分享按钮代码、GA分析代码等。
建议将script标签放置在</body>就是为了不让JS代码阻塞DOM的渲染，不会在JS执行期间，网页一片空白卡顿的糟糕体验。
今天学习到GA的异步加载代码：
[javascript]
(function() {
     var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
     ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
     var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
 })();
[/javascript]
动态生成script标签并利用HTML5才新增的async属性设置为异步(可不写，但最好加上)，加上用匿名函数封装，避免了内部变量泄露到外部污染全局。
这份代码可以兼容不支持HTML5中async属性的浏览器，而如果只考虑现代浏览器的话，其实可以偷懒，直接给要异步加载的script一个async属性即可实现上述异步加载的效果了：
<script src="asyncFile.js" async></script> 