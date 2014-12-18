title: 【译】ES6 Generators（1）基础篇
categories:
  - Translate
date: 2014-09-27 18:45:45
tags:
  - ES6
  - generators
---

> 译注1：此文带着自己的理解，不完全按原文翻译。[原文地址](http://davidwalsh.name/es6-generators)

> 译注2：原文晦涩难懂的地方，尽力做了注释或修饰，方便大家理解。错误之处欢迎各位校验指正。

*generator* 即生成器，是 ES6 中众多特性中的一种，是一个新的函数类型。

这篇文章旨在介绍 generator 的基础知识，以及告诉你在 JS 的未来，他们为何如此重要。

# 运行直到完成 (Run-To-Completion)

为了理清这个新的函数类型和其他函数类型有何区别，我们首先需要了解 『run to completion』 的概念。

我们知道 JS 是单线程的，所以一旦一个函数开始执行，排在队列后边的函数就必须等待这个函数执行完毕。

举个栗子：

```
setTimeout(function(){
    console.log("Hello World");
},1);

function foo() {
    // 注意: 永远不要使用这种超长的循环，这里只是为了演示方便
    for (var i=0; i<=1E10; i++) {
        console.log(i);
    }
}

foo();
// 0..1E10
// "Hello World"

```

在这段代码中，我们先执行了 `foo()` 然后执行 `setTimeout`，而 `foo()` 中的 for 循环将花费超长的时间才能完成。

只有等待这个漫长的循环结束后，`setTimeout` 中的 `console.log('Hello World')` 才能执行。

如果 `foo()` 函数能够被中断会怎样呢？

这是多线程编程语言的挑战，但我们并不需要考虑这个，因为 JS 是单线程的。

# 运行可被中止 (Run..Stop..Run)

使用 ES6 的生成器特性，我们有了一种新的函数类型：

允许这个函数的执行被中断一次或多次，在中断的期间我们可以去做其他操作，完成后再回来恢复这个函数的执行。

如果你了解过其他并发型或多线程的语言的话，你可能知道『协作(cooperative)』：

在一个函数执行期间，允许执行中断，在中断期间与其他代码进行协作。

ES6 生成器函数在并发行为中体现了这种『协作』的特性。

在生成器函数体中，我们可以使用一个新的 `yield` 关键字在内部来中断函数的执行。

需要注意的是，生成器并不能恢复自己中断的执行，我们需要一个额外的控制来恢复函数的执行。

所以，一个生成器函数能够被中断和重启。那生成器函数中断自己的执行后，怎么才知道何时恢复执行呢？

我们可以使用 `yield` 来对外发送中断的信号，当外部返回信号时再恢复函数的执行。

# 生成器的语法

我们可以这样声明一个生成器函数：

```
function *foo() {
    // ...
}
```

注意这里的星号(*)即声明了这个函数是属于生成器类型的函数。

生成器函数大多数功能与普通函数没有区别，只有一部分新颖的语法需要学习。

先介绍一个 `yield` 关键字：

`yield ___` 也叫做 『yield 表达式』，当我们重启生成器时，会向函数内部传值，这个值为对应的 `yield ___` 表达式的计算结果。

举个栗子：

```
function *foo() {
    var x = 1 + (yield "foo");
    console.log(x);
}
```

在这段代码中， `yield "foo"` 表达式将在函数中断时，向外部发送 "foo" 这个值，且当这个生成器重启时，外部传入的值将作为这个表达式的结果：

在这里，外部传入的值将会与 `1` 进行相加操作，然后赋值给 `x`。

看到双向通信的特点了么？我们在生成器内部向外发送 "foo" 然后中断函数执行，然后当生成器接收到外部传入一个值时，生成器将重启，函数将恢复执行。

如果我们只是向中止函数而不对外传值时，只使用 `yield` 即可：

```
// 注意: `foo(..)` 在这里并不是一个生成器
function foo(x) {
    console.log("x: " + x);
}

function *bar() {
    yield; // 只是中断，而不向外传值
    foo( yield ); // 当外部传回一个值时，将执行 foo() 操作
}
```

# 生成器迭代器(Generator Iterator)

迭代器是一种设计模式，定义了一种特殊的行为：

我们通过 `next()` 来获取一组有序的值。

举个栗子：我们有个数组为 [1, 2, 3, 4, 5]，第一次调用 `next()` 将返回 1，第二次调用 `next()` 将返回 2，以此类推，当数组内的值都返回完毕时，继续调用 `next()`将返回 null 或 false。

为了从外部控制生成器函数，我们使用生成器迭代器(generator iterator)来实现，举个栗子：

```
function *foo() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
}
```
我们先定义了一个生成器函数 `foo()`，接着我们调用它一次来生成一个迭代器：

```
var it = foo();
```

你可能会疑问为啥我们不是使用 `new` 关键字即 `var it = new foo()` 来生成迭代器？好吧，这语法背后比较复杂已经超出了我们的讨论范围了。

接下来我们就可以使用这个迭代器了：

```
console.log( it.next() ); // { value: 1, done: false }
```

这里的 `it.next()` 返回 `{ value: 1, done: false }`，其中的 `value: 1` 是 `yield 1` 返回的值，而 `done: false` 表示生成器函数还没有迭代完成。

继续调用 `it.next()` 进行迭代：

```
console.log( it.next() ); // { value:2, done:false }
console.log( it.next() ); // { value:3, done:false }
console.log( it.next() ); // { value:4, done:false }
console.log( it.next() ); // { value:5, done:false }
```

注意我们迭代到值为 `5`时，`done` 还是为 `false`，是因为这时候生成器函数并未处于完成状态，我们再调用一次看看：

```
console.log( it.next() ); // { value:undefined, done:true }
```

这时候我们已经执行完了所有的 `yield ___` 表达式，所以 `done` 已经为 `true`。

你可能会好奇的是：如果我们在一个生成器函数中使用了 `return`，我们在外部还能获取到 `yield` 的值么？

*答案可以是：能*

```
function *foo() {
    yield 1;
    return 2;
}

var it = foo();

console.log( it.next() ); // { value:1, done:false }
console.log( it.next() ); // { value:2, done:true }
```

让我们看看当我们使用迭代器时，生成器怎么对外传值，以及怎么接收外部传入的值：

```
function *foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield (y / 3);
    return (x + y + z);
}

var it = foo( 5 );

// 注意：这里没有给 `it.next()` 传值
console.log( it.next() );       // { value:6, done:false }
console.log( it.next( 12 ) );   // { value:8, done:false }
console.log( it.next( 13 ) );   // { value:42, done:true }
```

我们传入参数 `5` 先初始化了一个迭代器。

第一个 `next()` 中没有传递参数进去，因为这个生成器函数中没有对应的 `yield` 来接收参数，所以如果我们在第一个 `next()` 强制传参进去的话，什么都不会发生。
第一个 `yield (x+1)` 将返回 `value: 6` 到外部，此时生成器未迭代完毕，所以同时返回 `done: false` 。

第二个 `next(12)` 中我们传递了参数 `12` 进去，则表达式 `yield(x+1)` 会被赋值为 12，相当于：

```
var x = 5;
var y = 2 * 12; // => 24
```

第二个 `yield (y/3)` 将返回 `value: 8` 到外部，此时生成器未迭代完毕，所以同时返回 `done: false` 。

同理，在第三个 `next(13)` 中我们传递了参数 `13` 进去，则表达式 `yield(y/3)` 会被赋值为 13，相当于：

```
var x = 5
var y = 24;
var z = 13;
```
第三个 `yield`并不存在，所以会 `return (x + y + z)` 即返回 `value: 42` 到外部，此时生成器已迭代完毕，所以同时返回 `done: true` 。

*答案也可以是：不能！*

依赖  `return` 从生成器中返回一个值并不好，因为当生成器遇见了 `for..of` 循环的时候，被返回的值将会被丢弃，举个栗子：

```
function *foo() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
    return 6;
}

for (var v of foo()) {
    console.log( v );
}
// 1 2 3 4 5

console.log( v ); // 仍然是 `5`, 而不是 `6` 
```

看到了吧？由 `foo()` 创建的迭代器会被 `foo..of` 循环自动捕获，且会自动进行一个接一个的迭代，直到遇到 `done: true`，就结束了，并没有处理 `return` 的值。

所以，`for..of` 循环会忽略被返回的 `6`，同时因为没有暴露出 `next()` 方法，`for..of` 循环就不能用于我们在中断生成器的期间，对生成器进行传值的场景。

# 总结

看了以上 ES6 Generators 的基础知识，很自然地就会想我们在什么场景下会用到这个新颖的生成器呢？

当然有很多的场景能发挥生成器的这些特性了，这篇文章只是抛砖引玉，我们将继续深入挖掘生成器的魔力！

当你在最新的 Chrome nightly 或 canary 版，或 Firefox nightly版，甚至在 v0.11+ 版本的 node （带 --harmony 开启 ES6 功能）中运行了以上这些代码片段后，我们可能会产生以下疑问：

1. 怎么进行错误处理呢？
2. 一个生成器怎么调用另一个生成器呢？
3. 怎么异步地使用生成器呢？

别担心，请听下回分解：）