title: 【译】ES6 Generators（2）深入篇
categories:
  - Translate
date: 2014-09-28 18:45:45
tags:
  - ES6
  - generators
---

> 译注1：此文带着自己的理解，不完全按原文翻译。[原文地址](http://davidwalsh.name/es6-generators-dive)

> 译注2：原文晦涩难懂的地方，尽力做了注释或修饰，方便大家理解。错误之处欢迎各位校验指正。

如果你仍然对 ES6 Generators 不熟悉的话，建议你先阅读并运行 [【译】ES6 Generators 基础篇（1）]() 中的代码片段，理解了生成器的基础知识后，就可以阅读这篇文章了解更多的细节啦。

# 错误处理

ES6 中生成器的其中一个强大的特点就是：函数内部的代码编写风格是同步的，即使外部的迭代控制过程可能是异步的。

也就是说，我们可以简单地对错误进行处理，类似我们熟悉的 `try..catch` 语法，举个栗子：

```
function *foo() {
    try {
        var x = yield 3;
        console.log( "x: " + x ); // 如果出错，这里可能永远不会执行
    }
    catch (err) {
        console.log( "Error: " + err );
    }
}
```

即使这个生成器可能会在 `yield 3` 处中断，当接收到外部传入的错误时，`try..catch` 将会捕获到。

具体一个错误是怎样传入生成器的呢，举个栗子：

```
var it = foo();

var res = it.next(); // { value:3, done:false }

// 我们在这里不调用 it.next() 传值进去，而是触发一个错误
it.throw( "Oops!" ); // Error: Oops!
```

我们可以使用 `throw()` 方法产生错误传进生成器中，那么在生成器中断的地方，即 `yield 3` 处会产生错误，然后被 `try..catch` 捕获。

注意：如果我们使用 `throw()` 方法产生一个错误传进生成器中，但没有对应的 `try..catch` 对错误进行捕获的话，这个错误将会被传出去，外部如果不对错误进行捕获的话，则会抛出异常：

```

function *foo() { }

var it = foo();
// 在外部进行捕获
try {
    it.throw( "Oops!" );
}
catch (err) {
    console.log( "Error: " + err ); // Error: Oops!
}
```

当然，我们也可以进行反方向的错误捕获：

```
function *foo() {
    var x = yield 3;
    var y = x.toUpperCase(); // 若 x 不是字符串的话，将抛出TypeError 错误
    yield y;
}

var it = foo();

it.next(); // { value:3, done:false }

try {
    it.next( 42 ); // `42` 是数字没有 `toUpperCase()` 方法，所以会出错
}
catch (err) {
    console.log( err ); // 捕获到 TypeError 错误
}
```

# 生成器委托

另一个我们想做的可能是在一个生成器中调用另一个生成器。

我并不是指在一个生成器中初始化另一个生成器，而是说我们可以将一个生成器的迭代器控制交给另一个生成器。

为了实现委托，我们需要用到 `yield` 关键字的另一种形式：`yield *`，举个栗子：

```
function *foo() {
    yield 3;
    yield 4;
}

function *bar() {
    yield 1;
    yield 2;
    yield *foo(); // `yield *` 将迭代器控制委托给了 `foo()`
    yield 5;
}

for (var v of bar()) {
    console.log( v );
}
// 1 2 3 4 5
```

以上这段代码应该通俗易懂：当生成器 `bar()` 迭代到 `yield 2` 时，先将控制权交给了另一个生成器 `foo()`迭代完后再将控制权收回，继续进行迭代。

这里使用了 `for..of` 循环进行示例，正如在基础篇我们知道 `for..of` 循环中没有暴露出 `next()` 方法来传递值到生成器中，所以我们可以用手动的方式：

```
function *foo() {
    var z = yield 3;
    var w = yield 4;
    console.log( "z: " + z + ", w: " + w );
}

function *bar() {
    var x = yield 1;
    var y = yield 2;
    yield *foo(); // `yield *` 将迭代器控制委托给了 `foo()`
    var v = yield 5;
    console.log( "x: " + x + ", y: " + y + ", v: " + v );
}

var it = bar();

it.next();      // { value:1, done:false }
it.next( "X" ); // { value:2, done:false }
it.next( "Y" ); // { value:3, done:false }
it.next( "Z" ); // { value:4, done:false }
it.next( "W" ); // { value:5, done:false }
// z: Z, w: W

it.next( "V" ); // { value:undefined, done:true }
// x: X, y: Y, v: V
```

尽管我们在这里只展示了一层的委托关系，但具体场景中我们当然可以使用多层的嵌套。

一个 `yield *` 技巧是，我们可以从被委托的生成器（比如示例中的 `foo()`） 获取到返回值，举个栗子：

```
function *foo() {
    yield 2;
    yield 3;
    return "foo"; // 返回一个值给 `yield*` 表达式
}

function *bar() {
    yield 1;
    var v = yield *foo();
    console.log( "v: " + v );
    yield 4;
}

var it = bar();

it.next(); // { value:1, done:false }
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // "v: foo"   { value:4, done:false } 注意：在这里获取到了返回的值
it.next(); // { value:undefined, done:true }
```

`yield *foo()` 得到了 `bar()` 的控制权，完成了自己的迭代操作后，返回了一个 `v: foo` 值 给`bar()` ，然后 `bar()` 再继续迭代下去。

`yield` 和 `yield *` 表达式的一个有趣的区别是：在 `yield` 中，返回值在 `next()` 中传入的，而在 `yield *` 中，返回值是在 `return` 中传入的。

此外，我们也可以在委托的生成器中进行双向的错误绑定，举个栗子：

```
function *foo() {
    try {
        yield 2;
    }
    catch (err) {
        console.log( "foo caught: " + err );
    }

    yield; // 中断

    // 现在抛出另一个错误
    throw "Oops!";
}

function *bar() {
    yield 1;
    try {
        yield *foo();
    }
    catch (err) {
        console.log( "bar caught: " + err );
    }
}

var it = bar();

it.next(); // { value:1, done:false }
it.next(); // { value:2, done:false }

it.throw( "Uh oh!" ); // 将会在 `foo()` 内部捕获
// foo caught: Uh oh!

it.next(); // { value:undefined, done:true }  --> 这里没有错误
// bar caught: Oops!
```

`throw( "Uh oh!" )` 在代理给 `foo()` 的过程中，抛了个错误进去，所以错误在 `foo()` 中被捕获。

同理，`throw "Oops!"`  在 `foo()` 内部抛出的错误，将会传回给 `bar()` 后，被 `bar()` 中的 `try..catch` 捕获到。

# 总结

生成器有着同步方式的编写语法，意味着我么可以使用 `try..catch` 在 `yield` 表达式中进行错误处理。

生成器迭代器中也有一个 `throw()` 方法用于在中断期间向生成器内部传入一个错误，这个错误能被生成器内部的 `try..catch` 捕获。

`yield *` 允许我们将迭代器的控制权从当前的生成器中委托给另一个生成器。好处是 `yield *` 扮演了在生成器间传递消息和错误的角色。

了解了这么多，还有一个很重要的问题没有解决：

怎么异步地使用生成器呢？

关键是要实现这么一个机制：在异步环境中，当迭代器的 `next()` 方法被调用，我们需要定位到生成器中断的地方重新启动。

别担心，请听下回分解：）