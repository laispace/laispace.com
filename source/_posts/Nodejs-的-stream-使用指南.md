title: Nodejs 的 stream 使用指南
categories:
  - Node
date: 2014-08-28 09:58:22
tags:
  - stream
  - through
  - concat-stream
  - readable
  - writeable

---

# 使用 Stream 

当我们读取一个文件内容时，可能会这么写：
    
    var http = require('http');
    var fs = require('fs');
    var server = http.createServer(function (req, res) {
        fs.readFile(__dirname + '/data.txt', function (err, data) {
            res.end(data);
        });
    });
    server.listen(8000);

当这个文件 data.txt 非常大时，不仅会占满内存，而且对于网络不好的用户而言体验将非常差。


好在 req 和 res 都是 Stream 对象，我们可以使用 Stream 的方式来写代码：

    var http = require('http');
    var fs = require('fs');
    var server = http.createServer(function (req, res) {
        var stream = fs.createReadStream(__dirname + '/data.txt');
        stream.pipe(res);
    });
    server.listen(8000);

我们使用 fs.createReadStream 创建了一个 Stream 对象，.pipe() 方法会监听对应的 `data` 和 `end` 事件。

使用 Stream 的好处在于，我们将 data.txt 分段（chunk）传输到客户端，减轻了网络带宽的压力。


<!--more-->

# 使用 oppressor 压缩数据

如果客户端支持 gzip 或 deflate 压缩的话，我们就可以使用 oppressor 这个模块来对数据进行压缩后传输：

    var http = require('http');
    var fs = require('fs');
    var oppressor = require('operessor');
    var server = http.createServer(function (req, res) {
        var stream = fs.createReadStream(__dirname + '/data.txt');
        stream.pipe(oppressor(req)).pipe(res);
    });
    server.listen(8000);

# 理解 Stream 的基础知识

## pipe 管道

我们可以这么理解 pipe:
    
    src.pipe(A).pipe(B).pipe(dst);            

等价于：

    src.pipe(A);
    A.pipe(B);
    B.pipe(dst);

即把 src 这个输入交给 A 进行处理后，输出到 B处理，然后把结果输出到 dst。

# readable streams 可读流

在上述代码中，src 就是一个 readable stream 即可读流。

让我们来创建一个可读流：

    var Readable = require('stream').Readable;
    var rs = new Readable;
    rs.push('hello ');
    rs.push('world \n');
    rs.push(null);
    rs.pipe(process.stdout);

将这段代码保存到 read0.js 中，然后执行它：

    $ node read0.js

将得到输出：

    hello world

注意 `rs.push(null)` 用于指明我们对这个可读流写入数据完毕。


在发出 `rs.push(null)` 指明写入数据完毕之前，我们可以使用 `rs.push()` 往可读流中继续输入数据。

而有时候我们希望根据特定条件完成可读流的输入，这时候就可以改写 Readable._read() 方法。

    var Readable = require('stream').Readable;
    var rs = new Readable;
    var c = 97;
    rs._read = function () {
        rs.push(String.fromCharCode(c++));
        if (c > 'z'.charCodeAt(0)) {
            rs.push(null);
        }
    };
    rs.pipe(process.stdout);

rs._read() 将从 `a` 读到 `z`，然后才停止对可读流的写入。

将这段代码保存到 read1.js 中，然后执行它：

    $ node read1.js

将得到输出：

    abcdefghijklmnopqrstuvwxyz

注意我们改写了 rs._read() 方法而并没有调用它，因为当条件 `c > 'z'.charCodeAt(0)` 成立时，我们使用 `rs.push(null)` 指明可读流写入数据完毕。

为了证明输出 `a-z` 的过程中调用了 `rs._read()` 多次，我们编写：

    var Readable = require('stream').Readable;
    var rs = new Readable;
    var c = 97 - 1;
    rs._read = function () {
        if (c >= 'z'.charCodeAt(0)) {
            return rs.push(null);
        }
        setTimeout(function () {
            rs.push(String.fromCharCode(++c));
        }, 100);
    };
    rs.pipe(process.stdout);
    process.on('exit', function () {
        console.error('\n_read() called ' + (c - 97) + ' times');
    });
    process.stdout.on('error', process.exit);

将这段代码保存到 read2.js 中，然后执行它：

    $ node read2.js

将得到输出：

    abcdefghijklmnopqrstuvwxyz
    _read() called 25 times

而如果我们执行：

    $ node read2.js | head -c5

这里的 `| head -c5` 为 *nix 命令，表示只输出 5 个字节的数据，这时候将得到输出：

    abcde
    _read() called 5 times

有了 `| head -c5` 这个参数，当输出了 5 个字节的数据后，操作系统发出 SIFPIPE 信号，中断进程，process.stdout 产生错误 EPIPE。

接着 `process.stdout` 捕获到错误，触发 `exit` 事件，所以这时候记录下 `rs._read()` 的执行次数为 5。


## 使用 readable stream 可读流

直接使用可读流非常简单：

    process.stdin.on('readable', function () {
        var buf = process.stdin.read();
        console.dir(buf);
    });

将这段代码保存到 consume0.js 中，然后执行它：

    $ (echo abc; sleep 1; echo def; sleep 1; echo ghi) | node consume0.js 

我们在命令行中输出一些数据当做 consume0.js 的输入，将得到输出：

    <Buffer 61 62 63 0a>
    <Buffer 64 65 66 0a>
    <Buffer 67 68 69 0a>
    null

当 `process.stdin` 监听到有数据传入时，我们就可以使用 `process.stdin.read()` 读取到这些数据。

我们看到了输出中有 `null` 是因为当数据读取完毕时，`process.stdin.read()` 将返回 `null`。

而如果我们给 `process.std.read(n)` 传入了参数 `n` 时，将得到 n 字节的数据输出：

    process.stdin.on('readable', function () {
        var buf = process.stdin.read(3);
        console.dir(buf);
    });

将这段代码保存到 consume1.js 中，然后执行它：

    $ (echo abc; sleep 1; echo def; sleep 1; echo ghi) | node consume1.js 
   
我们在命令行中输出一些数据当做 consume1.js 的输入，但给 `process.stdin.read(3)` 传入了参数数字 3，将得到输出：

    <Buffer 61 62 63>
    <Buffer 0a 64 65>
    <Buffer 66 0a 67>

注意我们并没有得到 `abc` `def` `ghi` 对应的完整输出，因为我们限制了读取的字节数为 3，所以剩下的数据保存在了内存中。

我们能需要读出剩余的数据，改写代码为：

    process.stdin.on('readable', function () {
        var buf = process.stdin.read(3);
        console.dir(buf);
        process.stdin.read(0);
    });

将这段代码保存到 consume2.js 中，然后执行它：

    $ (echo abc; sleep 1; echo def; sleep 1; echo ghi) | node consume2.js 
   
我们在命令行中输出一些数据当做 consume2.js 的输入，读完 3 个字节数据后，继续读取。将得到输出：

    <Buffer 61 62 63>
    <Buffer 0a 64 65>
    <Buffer 66 0a 67>
    <Buffer 68 69 0a>

## writable streams 可写流

只需要使用 Writable._write() 即可创建 writable strearm 可写流：

    var Writable = require('stream').Writable;
    var ws = new Writable();
    ws._write = function (chunk, enc, callback) {
        console.dir(chunk);
        callback();
    };
    process.stdin.pipe(ws);

将这段代码保存到 write0.js 中，然后执行它：

    $ (echo hello; sleep 1; echo world) | node write0.js 

将得到输出：
    
    <Buffer 68 65 6c 6c 6f 0a>
    <Buffer 77 6f 72 6c 64 0a> hello world    

第一个参数 `chunk` 表示将要写入的数据。

第二个参数 `enc` 表示编码，如果 chunk 为字符串，编码类型则为字符串。

除非我们在创建可写流时指定了 `Writable({ decodeStrings: false })`，否则数据将会被转化为 Buffer 类型。

第三个参数 `callback` 表示回调函数。

## 使用 writable stream 可写流

直接使用 `.write()` 方法就即可使用可写流：

    process.stdout.write('hello world \n');

我们可以将文件内容创建为可写流：

    var fs = require('fs');
    var ws = fs.createWriteStream('message.txt');
    ws.write('hello ');
    setTimeout(function () {
        ws.end('world \n');
    }, 1000);

将这段代码保存到 writing1.js 中，然后执行它：

    $ node writing1.js 

注意这里用 `ws.end()` 指明我们写入数据完毕，数据将被写入到 `message.txt` 中：
    
    $ cat message.txt
    hello world

## transform 转换

Transform streams 即转换流，是用于转换输入为输出的可读/写的双工流。

## duplex 双工

Duplex streams 即双工流，流的两端都可进行读或写：

    A.pipe(B).pipe(A);

## classic streams 经典流

Classic streams 即经典流，最早出现在 node v0.4 中。

当一个流注册了 `data` 监听函数时，就会转换到静电流模式，这时候可以使用旧的 API 对流进行操作。

### classic readable streams 经典可读流

经典可读流只有 `data` 和 `end` 事件触发器，分别用来接收数据和停止接收数据。

`.pipe()` 通过判断 `stream.readable` 的值来检查一个经典流是否可读：

    var Stream = require('stream');
    var stream = new Stream;
    stream.readable = true;
    var c = 64;
    var iv = setInterval(function () {
        if (++c >= 75) {
            clearInterval(iv);
            stream.emit('end');
        }
        else stream.emit('data', String.fromCharCode(c));
    }, 100);
    stream.pipe(process.stdout);

将这段代码保存到 classic0.js 中，然后执行它：

    $ node classic0.js 
   
将得到输出：

    ABCDEFGHIJ

上面这段代码中的 `.emit` 用于触发 `data` 和 `end` 事件。

为了从命令行中得到输入，我们使用 `on` 对这两个事件进行监听：

    process.stdin.on('data', function (buf) {
        console.log(buf);
    });
    process.stdin.on('end', function () {
        console.log('__END__');
    });

将这段代码保存到 classic1.js 中，然后执行它：

    $ node classic1.js 
   
我们接着输入 `hello world` 和 `hello xiaolai`将分别得到输出：

    hello world
    <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64 0a>
    hello xiaolai
    <Buffer 68 65 6c 6c 6f 20 78 69 61 6f 6c 61 69 0a>

或者在运行 classic1.js 时就传入数据：

    $ (echo hello; sleep 1; echo world) | node classic1.js 

将得到输出：

    <Buffer 68 65 6c 6c 6f 0a>
    <Buffer 77 6f 72 6c 64 0a>
    __END__


注意 `data` 和 `end` 事件我们可以不再使用了，毕竟这是老旧的 API 了。

我们可以使用一些模块比如 [through](https://www.npmjs.org/package/through) 来处理流：

    var through = require('through');
    process.stdin.pipe(through(write, end));
    function write (buf) {
        console.log(buf);
    }
    function end () {
        console.log('__END__');
    }

将这段代码保存到 through.js 中，然后执行它：

    $ (echo hello; sleep 1; echo world) | node through.js
   
将得到输出：
    
    <Buffer 68 65 6c 6c 6f 0a>
    <Buffer 77 6f 72 6c 64 0a>
    __END__

也可以使用 [concat-stream](https://www.npmjs.org/package/concat-stream) 模块来进行操作：

    var concat = require('concat-stream');
    process.stdin.pipe(concat(function (body) {
        console.log(JSON.parse(body));
    }));

将这段代码保存到 concat-stream.js 中，然后执行它：

    $ echo '{"hello":"world"}' | node concat-stream.js 

将得到输出：

    { hello: 'world' }

经典的可读流具有 `.pause()` 和 `.resume()` 逻辑对暂停、恢复读取数据进行支持。

如果我们要使用这些操作的话，最好通过 [through](https://www.npmjs.org/package/through) 模块来完成。

### classic writable streams 经典可写流

经典可写流很简单，只需要定义 `.write(buf)` `.end(buf)` `.destroy()` 即可。

注意 `.end(buf)` 可能不包含参数，即 相当于 `stream.write(buf); stream.end()` 指明写入流完毕。

## 阅读更多

- [流的官方文档](http://nodejs.org/docs/latest/api/stream.html#stream_stream)
- 使用 [readable-stream](https://npmjs.org/package/readable-stream) 模块兼容 v0.8 及以下版本的 node，只需要用 `require('readable-stream')` 取代 `require('stream')` 来操作即可。

## built-in streams 内置的流

这些流是 node 中内置的流。

！！！未完待续 https://github.com/substack/stream-handbook#built-in-streams










### 参考链接

- [Implement readable._read(size), but do NOT call it directly](http://nodejs.org/api/stream.html#stream_readable_read_size_1)

- [readable.push(chunk, [encoding]) should be called by Readable implementors, NOT by consumers of Readable streams.](http://nodejs.org/api/stream.html#stream_readable_push_chunk_encoding)



   


