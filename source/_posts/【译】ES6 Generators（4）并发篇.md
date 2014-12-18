title: 【译】ES6 Generators（4）并发篇
categories:
  - Translate
date: 2014-09-30 18:45:45
tags:
  - ES6
  - generators
---

注意：这篇文章没翻译完，可以先看[原文](http://davidwalsh.name/async-generators)

> 译注1：此文带着自己的理解，不完全按原文翻译。[原文地址](http://davidwalsh.name/concurrent-generators)

> 译注2：原文晦涩难懂的地方，尽力做了注释或修饰，方便大家理解。错误之处欢迎各位校验指正。

我们最好的主题是探索前沿的东西，接下来的概念可能会听起来有点懵，但一想到在未来这些东西会大派用场，想想都有点小激动呢！


这篇文章的主题受到 [David Nolen](http://github.com/swannodette) [@swannodette](http://twitter.com/swannodette) 的鼓舞，他写了介绍 CSP 的一些文章：

- [Communicating Sequential Processes](http://swannodette.github.io/2013/07/12/communicating-sequential-processes/)
- [ES6 Generators Deliver Go Style Concurrency](http://swannodette.github.io/2013/08/24/es6-generators-and-csp/)
- [Extracting Processes](http://swannodette.github.io/2013/07/31/extracting-processes/)

当然，你也可以继续阅读这篇文章，听我娓娓道来并发式生成器的介绍。

我尝试了 Go 语言风格的 CSP API 的实现。当然比我更聪明的同行可能会看到我在这个探索中所遗漏的地方，我会持续不断地探索和尝试，并坚持和你们分享我所发现的东西。

# 了解 CSP (Communicating Sequential Processes)

CSP 这个概念来自 Tony Hoare的《[Communicating Sequential Processes](http://www.usingcsp.com/)》一书。

这是一个非常深的计算机理论，我并不打算以太多晦涩难懂的计算机专业术语，而是轻松地介绍它。

## 『sequential』 即顺序

这是描述 ES6 生成器中单线程行为和同步风格代码的另一种方式。

还记得一个生成器的语法么？

```
function *main() {
    var x = yield 1;
    var y = yield x;
    var z = yield (y * 2);
}
```

这里的每一个表达式是按序执行，`yield` 关键字虽然指明了生成器中断和恢复的地方，但并没有改变生成器函数中从上到下执行的顺序，对吧？

## 『Processes』 即进程

每一个生成器表现得就像是一个虚拟的进程，它可以自己中断，向其他生成器(进程)传递信号，且能从其他生成器(进程)接收信号后，恢复自己的执行流程。

如果生成器能够访问共享的内存空间的话（也就是能访问除自己内部的本地变量外的自由变量），它就不是那么独立了。

假设我们有一个不访问外部变量的生成器函数，那么它在理论上就可以执行自己的进程。

但我们通常同时有多个生成器(多进程)绑定在一起，需要彼此间协作以完成任务。

那我们为什么要将生成器分离为多个，而不是合在一起呢？因为我们要做到 *功能与关注点的分离(separation of capabilities/concerns)* 。

假定我们将 XYZ 任务分离为连续的子任务 X, Y, Z 分别实现，就增加了程序的维护性。举个栗子：

```
// 原来是这样
function XYZ() {
    console.log('x');
    console.log('y');
    console.log('z');
}
// 可以拆分为：
function X() {
    console.log('x');
    Y();
}
function Y() {
    console.log('y');
    Z();
}
function Z() {
    console.log('z');
}
```
将功能进行模块化划分，增大了程序的可维护性。

同理，对于多生成器(多进程)来说，我们也可以这么做。

## 『communicating』 即通信

生成器(进程)之间互相协作，就需要一个通信频道(communication channel)来传递消息。

实际上，我们并不一定需要在通信频道上传递消息来实现通信，我们可以通过移交控制权的方式来实现。

为什么要通过移交控制权的方式呢？主要是因为 JS 是一个单线程的语言。

单线程意味着同一时刻只能执行一个任务，其他任务排在队列里被挂起(或者说是中断)，等待队列前面的任务完成才能恢复自己的执行。

多个独立的生成器(线程)能够协作和通信好像不是很现实，将多个生成器分离以实现松耦合的目标看似美好但好像不切实际。

可能我是错的，但我并没有找到实现任意两个生成器绑定到一起实现 CSP 匹配的方法。要实现这种设计的话，两个生成器或许需要一个通信协议来支撑。

# JS 中的 CSP 

这里有几个应用于 JS 的 CSP 理论探索。

前面提到的 David Nolen 有几个有趣的项目，包括 [Om](https://github.com/swannodette/om)、[core.async](http://www.hakkalabs.co/articles/core-async-a -clojure-library/)。

[Koa](http://koajs.com/) 也有一个有趣的实现，主要是通过它的 `use()` 方法。

还有一个类似 core.async/Go CSP  API 实现的 [js-csp](https://github.com/ubolonton/js-csp)。

你可以去了解这几个项目用 JS 实现的不同的 CSP。

## asynquence 中 CSP 的实现

我已经有 asynquence 的 runner() 插件来处理[异步的生成器操作](http://davidwalsh.name/async-generators/#rungenerator-library-utility)，所以我在这里尝试实现了 CSP 功能。

我需要解决的第一个问题是：我们怎么知道哪一个生成器即将接管控制权呢？

我们可以让每一个生成器都有一些特定的属性如 ID 来告知其他生成器的话，这样做好像比较繁琐笨重。

在经过各种实验后，我选择了一种循环调度的方法：如果我们要将 A, B, C 三个生成器连接起来，且 A 会得到控制权，接着 A 发出 `yield` 信号将控制权移交给 B, 再接着 B 发出 `yield` 信号将控制权移交给 C，最后 C 再把控制权移交给 A，形成一个循环。

但我们怎么精确地实现控制呢？有明确的 API 么？同样，在经过很多实验后，我使用了一个巧妙的办法，与 [Koa 中实现的类似](http://koajs.com/#cascading)：

每一个生成器都有一个指向共享的令牌(token)，对这个令牌  `yield` 后就会发出一个移交控制的信号。

未完待续。。。http://davidwalsh.name/concurrent-generators





