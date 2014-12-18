title: NODE-调试方法
tags:
  - debug
id: 539
categories:
  - Node
date: 2013-11-30 14:29:14
---

NODE核心自带的STDIO模块，就是类似浏览器端的console.*()系列方法，可像浏览器端调试代码一样，简单对代码进行调试：

# 输出普通信息：
console.log() 在控制台输出信息，可用来记录一个函数是否执行、函数执行时某些变量的值

# 输出错误信息：
console.error() 输出错误信息，常配合try catch语句使用

# 判断代码块的性能：
console.time(‘mytime')和console.timeEnd(‘mytime’)会输出代码块执行的时间

# 设置断点：
debugger; 遇到这句断点时，代码都会中止执行，按play可继续代码执行

# 安装node-inspector调试器
$ npm install -g node-inspector
使用node-inspector 对nodejs代码进行调试,需要环境：webkit内核浏览器

&nbsp;

[javascript]
 // file t6.js
 var foo = function(){
     var a = 3, b = 5;
     var bar = function(){
         var b = 7, c = 11;
         a += b + c;
     }
     bar();
 }
 foo();

[/javascript]

<!-- more -->


写好以上代码后，开始调试
$ node --debug-brk t6.js

控制台显示对5858端口进行了监听：

![](http://laispace.u.qiniudn.com/NODE-%E8%B0%83%E8%AF%95%E6%96%B9%E6%B3%951.png)
然后启动node inspector
$ node-inspector

启动后会提示访问http://127.0.0.1:8080/debug?port=5858进行调试
开后通过单击行号来设置/移除断点，可依次按play按钮并观察Scope Variables下的变量值的变化来理解代码的执行过程

总结：使用inspector 可用来按部查询代码引用的文件和模块，让代码具备交互性