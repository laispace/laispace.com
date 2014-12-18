title: 【译】ES6 Generators（3）异步篇
categories:
  - Translate
date: 2014-09-29 18:45:45
tags:
  - ES6
  - generators
---

> 译注1：此文带着自己的理解，不完全按原文翻译。[原文地址](http://davidwalsh.name/async-generators)

> 译注2：原文晦涩难懂的地方，尽力做了注释或修饰，方便大家理解。错误之处欢迎各位校验指正。

生成器提供了同步方式编写的代码风格，这就允许我们隐藏异步的实现细节。

我们就可以用一种非常自然的方式来表达程序的执行流程，避免了同时处理异步代码的语法和陷阱。

换句话说，我们利用生成器从内到外、从外到内双向传值的特点，将不同的值的处理交给了不同的生成器逻辑，只需要关心获取到特定的值进行某种操作，而无需关心特定的值如何产生（通过`netx()` 将值的产生逻辑委托出去）。

这么一来，异步处理的优点以及易读的代码结合到一起，就加强了我们程序的可维护性。

# 最简单的异步

举个栗子，假定我们已经有了以下代码：

```
function makeAjaxCall(url,cb) {
    // 执行一个 ajax 请求
    // 请求完成后执行 `cb(result)` 
}

makeAjaxCall( "http://some.url.1", function(result1){
    var data = JSON.parse( result1 );

    makeAjaxCall( "http://some.url.2/?id=" + data.id, function(result2){
        var resp = JSON.parse( result2 );
        console.log( "我们请求到的数据是: " + resp.value );
    });
} );
```

使用简单的生成器来表达的话，就像这样：

```
function request(url) {
   // 调用这个普通函数来隐藏异步处理的细节
   // 使用 `it.next()` 来恢复调用这个普通函数的生成器函数的迭代器
    makeAjaxCall( url, function(response){
        // 异步获取到数据后，给生成器发送 `response` 信号
        it.next( response );
    } );
    // 注意: 这里没有返回值
}

function *main() {
    var result1 = yield request( "http://some.url.1" );
    var data = JSON.parse( result1 );

    var result2 = yield request( "http://some.url.2?id=" + data.id );
    var resp = JSON.parse( result2 );
    console.log( "The value you asked for: " + resp.value );
}

var it = main();
it.next(); // 开始迭代
```

`request()` 这个工具函数只是将我们的异步请求数据的代码进行了封装，需要注意的是在回调函数中调用了生成器的 `next()` 方法。

当我们使用 `var it = main();` 创建了一个迭代器后，紧接着使用 `it.next();` 开始迭代，这时候遇到第一个 `yield` 中断了生成器，转而执行 `request( "http://some.url.1" )`

当 `request( "http://some.url.1" )` 异步获取到数据后，在回调函数中调用 `it.next(response)` 将 `response` 传回给生成器刚刚中断的地方，生成器将继续迭代。

这里的亮点就是，我们在生成器中无需关心异步请求的数据如何获取，我们只知道调用了  `request()` 后，当需要的数据获取到了，就会通知生成器继续迭代。

这么一来在生成器中我们使用同步方式的编写风格，其实我们获取到了异步数据！

同理，当我们继续调用 `it.next()` 时，会遇到第二个 `yield` 中断迭代，发出第二个请求 `yield request( "http://some.url.2?id=" + data.id )` 异步获取到数据后再恢复迭代，我们依旧不用关心异步获取数据的细节了，多爽！

以上这段代码中，`request()` 请求的是异步 AJAX 请求，但如果我们后续改变程序给 AJAX 设置了缓存了，获取数据会先从缓存中获取，这时候没有执行真正的 AJAX 请求就不能在回调函数中调用 `it.next(response)` 来恢复生成器的中断了啊！

没关系，我们可以使用一个小技巧来解决这个问题，举个栗子：

```
// 给 AJAX 设置缓存
var cache = {};

function request(url) {
    // 请求已被缓存
    if (cache[url]) {
        // 使用 setTimeout 来模拟异步操作
        setTimeout( function(){
            it.next( cache[url] );
        }, 0 );
    }
    // 请求未被缓存，发出真正的请求
    else {
        makeAjaxCall( url, function(resp){
            cache[url] = resp;
            it.next( resp );
        } );
    }
}
```

看，当我们给我们的程序添加了 AJAX 缓存机制甚至其他异步操作的优化时，我们只改变了  `request()` 这个工具函数的逻辑，而无需改动调用这个工具函数获取数据的生成器：
```
var result1 = yield request( "http://some.url.1" );
var data = JSON.parse( result1 );
..
```
在生成器中，我们还是像以前一样调用 `request()` 就能获取到需要的异步数据，无需关心获取数据的细节实现！

这就是将异步操作当做一个细节实现抽象出来后展现出的魔力了！

# 更好的异步

上面介绍的异步方案对于简单的异步生成器来说工作良好，但用途有限，我们需要一个更强大的异步方案：使用 Promises.

如果你对 ES6 Promises 有迷惑的话，我建议你先读 [我写的介绍 Promises 的文章](http://blog.getify.com/promises-part-1/)

我们的代码目前有个严重的问题：回调多了会产生多重嵌套（即回调地狱）。

此外，我们目前还缺乏的东西有：

1. 清晰的错误处理逻辑。我们使用 AJAX 的回调可能会检测到一个错误，然后使用 `it.throw()` 将错误传回给生成器，在生成器中则使用 `try..catch` 来捕获错误。
    一来我们需要猜测我们可能发生错误且手动添加对应的错误处理函数，二来我们的错误处理代码没法重复使用。

2. 如果 `makeAjaxCall()` 函数不受我们控制，调用了多次回调的话，也会多次触发回调中的 `it.next()` ，生成器就会变得非常混乱。

    处理和阻止这种问题需要大量的手动工作，也非常不方便。

3. 有时候我们需要 『并行地』执行不只一个任务（比如同时触发两个 AJAX 请求）。而生成器中的 `yield` 并不支持两个或多个同时进行。
    
以上这些问题都可以用手动编写代码的方式来解决，但谁会想每次都重新编写类似的重复的代码呢？

我们需要一个更好的可信任、可重复使用的方案来支持我们基于生成器编写异步的代码。

怎么实现？使用 Promises ！

我们将原来的代码加入 Promises 的特性：

```
function request(url) {
    // 注意: 这里返回的是一个 promise
    return new Promise( function(resolve,reject){
        makeAjaxCall( url, resolve );
    } );
}
```

`request()` 函数中创建了一个 promise 实例，一旦 AJAX 请求完成，这个实例将会被 `resolved`。

我们接着将这个实例返回，这样它就能够被 `yield` 了。

接下来我们需要一个工具来控制我们生成器的迭代器，接收返回的 promise 实例，然后再通过 `next()` 来恢复生成器的中断：

```
// 执行异步的生成器
// 注意: 这是简化的版本，没有处理错误
function runGenerator(g) {
    // 注意：我们使用 `g()` 自动初始化了迭代器
    var it = g(), ret;

    // 异步地迭代
    (function iterate(val){
        ret = it.next( val );

        // 迭代未完成
        if (!ret.done) {
            // 判断是否为 promise 对象，如果没有 `then()` 方法则不是
            if ("then" in ret.value) {
                // 等待 promise 返回
                ret.value.then( iterate );
            }
            // 如果不是 promise 实例，则说明直接返回了一个值
            else {
                // 使用 `setTimeout` 模拟异步操作
                setTimeout( function(){
                    iterate( ret.value );
                }, 0 );
            }
        }
    })();
}
```

注意：我们在 `runGenerator()` 中先生成了一个迭代器 `var it = g()`，然后我们会执行这个迭代器直到它完成(`done: true`)。

接着我们就可以使用这个 `runGenerator()` 了：

```
runGenerator( function *main(){
    var result1 = yield request( "http://some.url.1" );
    var data = JSON.parse( result1 );

    var result2 = yield request( "http://some.url.2?id=" + data.id );
    var resp = JSON.parse( result2 );
    console.log( "你请求的数据是: " + resp.value );
} );
```

我们通过生成不同的 promise 实例，分别对这些实例进行 `yield`，不同的实例等待自己的 promise 被 `resolve` 后再执行对应的操作。

这么一来，我们只需要同时生成不同的 promise 实例，就可以『并行地』执行不只一个任务（比如同时触发两个 AJAX 请求）了。

既然我们使用了 promises 来管理生成器中处理异步的代码，我们就解决了只有在回调中才能实现的功能，这就避免了回调嵌套了。

使用 Generotos + Promises 的优点是：

1. 我们可以使用内建的错误处理机制。虽然这没有在上面的代码片段中展示出来，但其实很简单：

    监听 promise 中的错误，使用 `it.throw()` 把错误抛出，然后在生成器中使用 `try..catch` 进行捕获和处理即可。

2. 我们可以使用到 Promises 提供的 [control/trustability](http://blog.getify.com/promises-part-2/#uninversion) 特性。

3. Promises 提供了大量处理多并行且复杂的任务的特性。
    
    举个栗子：`yield Promise.all([ .. ])` 方法接收一组 promise 组成的数组作为参数，然后 `yield` 一个 promise 提供给生成器处理，这个 promise 会等待数组里所有 promise 完成。当我们得到 `yield` 后的 promise 时，说明传进去的数组中的所有 promise 都已经完成，且是按照他们被传入的顺序完成的。

首先，我们体验一下错误处理：

```
// 假设1: `makeAjaxCall(..)` 第一个参数判断是否有错误产生
// 假设2: `runGenerator(..)` 能捕获并处理错误

function request(url) {
    return new Promise( function(resolve,reject){
        makeAjaxCall( url, function(err,text){
            // 如果出错，则 reject 这个 promise
            if (err) reject( err );
            // 否则，resolve 这个 promise
            else resolve( text );
        } );
    } );
}

runGenerator( function *main(){
    // 捕获第一个请求的错误
    try {
        var result1 = yield request( "http://some.url.1" );
    }
    catch (err) {
        console.log( "Error: " + err );
        return;
    }
    var data = JSON.parse( result1 );
    
    // 捕获第二个请求的错误
    try {
        var result2 = yield request( "http://some.url.2?id=" + data.id );
    } catch (err) {
        console.log( "Error: " + err );
        return;
    }
    var resp = JSON.parse( result2 );
    console.log( "你请求的数据是: " + resp.value );
} );
```

如果一个 promise 被 `reject` 或遇到其他错误的话，将使用 `it.throw()` (代码片段中没有展示出来)抛出一个生成器的错误，这个错误能被 `try..catch` 捕获。

再举个使用 Promises 管理更复杂的异步操作的栗子：

```
function request(url) {
    return new Promise( function(resolve,reject){
        makeAjaxCall( url, resolve );
    } )
    // 对 promise 返回的字符串进行后处理操作
    .then( function(text){
        // 是否为一个重定向链接
        if (/^https?:\/\/.+/.test( text )) {
            // 是的话对向新链接发送请求
            return request( text );
        }
        // 否则，返回字符串
        else {
            return text;
        }
    } );
}

runGenerator( function *main(){
    var search_terms = yield Promise.all( [
        request( "http://some.url.1" ),
        request( "http://some.url.2" ),
        request( "http://some.url.3" )
    ] );

    var search_results = yield request(
        "http://some.url.4?search=" + search_terms.join( "+" )
    );
    var resp = JSON.parse( search_results );

    console.log( "Search results: " + resp.value );
} );

```

`Promise.all([ .. ])` 构造了一个 promise ，等待数组中三个 promise 的完成，这个 promise 会被 `yield` 给 `runGenerator()` 生成器，然后这个生成器就可以恢复迭代。

# 使用其他的 Promise 类库

在上面的代码片段中，我们自己编写了 `runGenerator()` 函数来提供 Generators + Promises 的功能，其实我们也可以使用社区里优秀的类库，举几个栗子： [Q](https://github.com/kriskowal/q) 、[Co](https://github.com/visionmedia/co)、 [asynquence](https://github.com/getify/asynquence/tree/master/contrib#runner-plugin) 等

接下来我会简要地介绍下 [asynquence](http://github.com/getify/asynquence) 中的 [runner插件](https://github.com/getify/asynquence/tree/master/contrib#runner-plugin) 。如果你感兴趣的话，可以阅读我写的[两篇深入理解 asynquence 的博文](http://davidwalsh.name/asynquence-part-1/)。

首先，asynquence 提供了回调函数中错误为第一参数的编写风格(error-first style)，举个栗子：

```
function request(url) {
    return ASQ( function(done){
        // 传进一个以错误为第一参数的回调函数
        makeAjaxCall( url, done.errfcb );
    } );
}
```

接着，asynquence 的 runner 插件会接收一个生成器作为参数，这个生成器可以处理传入的数据处理后再传出来，而所有的的错误会自动地传递：

```
// 我们使用 `getSomeValues()` 来产生一组 promise，并链式地进行异步操作
getSomeValues()

// 现在使用一个生成器来处理接收到的数据
.runner( function*(token){
    var value1 = token.messages[0];
    var value2 = token.messages[1];
    var value3 = token.messages[2];

    // 并行地执行三个 AJAX 请求
    // 注意: `ASQ().all(..)` 就像之前提过的 `Promise.all(..)`
    var msgs = yield ASQ().all(
        request( "http://some.url.1?v=" + value1 ),
        request( "http://some.url.2?v=" + value2 ),
        request( "http://some.url.3?v=" + value3 )
    );

    // 当三个请求都执行完毕后，进入下一步
    yield (msgs[0] + msgs[1] + msgs[2]);
} )

// 现在使用前面的生成器返回的值作为参数继续发送 AJAX 请求
.seq( function(msg){
    return request( "http://some.url.4?msg=" + msg );
} )

// 完成了一系列请求后，我们就获取到了想要的数据
.val( function(result){
    console.log( result ); // 获取数据成功!
} )

// 如果产生错误，则抛出
.or( function(err) {
    console.log( "Error: " + err );
} );
```

# ES7 async

在 ES7 草案中有一个提议，建议采用另一种新的 `async` 函数类型。

使用这种函数，我们可以向外部发出 promises，然后使用 `async` 函数自动地将这些 promises 连接起来，当 promises 完成的时候，就会恢复 `async` 函数自己的中断（不需要在繁杂的迭代器中手动恢复）。

这个提议如果被采纳的话，可能会像这样：

```
async function main() {
    var result1 = await request( "http://some.url.1" );
    var data = JSON.parse( result1 );

    var result2 = await request( "http://some.url.2?id=" + data.id );
    var resp = JSON.parse( result2 );
    console.log( "The value you asked for: " + resp.value );
}

main();
```

我们使用 `async` 声明了这种异步函数类型，然后使用 `main()` 直接调用这个函数，而不用像使用 `runGenerator()` 或 `ASQ().runner()` 一样进行包装。

此外，我们没有使用 `yield` 关键字，而是使用了新的 `await` 关键字来声明等待 `await` 后面的 promise 的完成。

# 总结

一言以蔽之：Generators + Promises 的组合，强大且优雅地用同步编码风格实现了复杂的异步控制操作。

使用一些简单的工具类库，比如上面提到的 [Q](https://github.com/kriskowal/q) 、[Co](https://github.com/visionmedia/co)、 [asynquence](https://github.com/getify/asynquence/tree/master/contrib#runner-plugin) 等，我们可以更方便地实现这些操作。

可以预见在不久的将来，当 ES7+ 发布的时候，我们使用 `async` 函数甚至可以无需使用一些类库支撑就可以实现原生的异步生成器了！


(译注：本文是第三篇文章，其实还有最后一篇是讲述并发式生成器的实现思路，涉及到 CSP 的相关概念，原文中引用了比较多的东西，读起来比较晦涩难懂，怕翻译出来与原文作者想要表达的东西相差太远，就先放一边了，感兴趣的可以直接[查看原文](http://davidwalsh.name/concurrent-generators)。
欢迎大牛接力)