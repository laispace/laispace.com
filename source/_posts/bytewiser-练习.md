title: bytewiser 练习
categories:
  - Node
date: 2014-08-23 14:04:01
tags:
  - bytewiser
  - buffer
  
---

> bytewiser 是 nodeschool.io 出品的nodejs入门练习项目

# bytewiser-exercise-1

Write a node program that prints a buffer object containing the string "bytewiser" using console.log.

```
var str = 'bytewiser';
var buffer = new Buffer(str);
console.log(buffer);
```

# bytewiser-exercise-2

Given an unknown number of bytes passed via process.argv, create a buffer from them and output a hexadecimal encoded representation of the buffer.
```
var array = process.argv.slice(2);
var buffer = new Buffer(array);
console.log(buffer.toString('hex'));

// 官方答案
// var bytes = process.argv.slice(2).map(function(arg) { return parseInt(arg) })
// console.log(new Buffer(bytes).toString('hex'))
```

<!--more-->

# bytewiser-exercise-3

Write a program that takes the first buffer written to `process.stdin`,
updates all instances of . with ! and then logs out the updated buffer object.
```
// 监听用户输入，将 . 替换为 ! 输出
process.stdin.on('data', function(buffer) {
  for (var i = 0; i < buffer.length; i++) {
    // 0x2e 对应为 .
    // 0x2e 对应为 !
    if (buffer[i] === 0x2e) buffer[i] = 0x21
  }
  console.log(buffer);
});
```

# bytewiser-exercise-4

The argument given to you from `process.argv[2]` will be a path to a file.
Read this file and split it by newline characters ('\n'). You should log one Buffer per line.

```
var fs = require('fs');
var file = process.argv[2];
fs.readFile(file, function (err, data) {
    if (err) {
        throw err;
    }
    var array = data.toString().split('\n');
    var len = array.length;
    var buffer;
    for (var i = 0; i < len; i++) {
        buffer = new Buffer(array[i]);
        console.log(buffer);
    };
});

// 官方答案
// var fs = require('fs')
// var file = fs.readFileSync(process.argv[2])
// var offset = 0
// for (var i = 0; i < file.length; i++) {
//   if (file[i] === 10) {
//     console.log(file.slice(offset, i))
//     i++
//     offset = i
//   }
// }
// console.log(file.slice(offset, i))
```

# bytewiser-exercise-5

Write a program that combines all of the buffers from `process.stdin`
and then writes the single big buffer out to the console.

```
var buffers = [];
process.stdin.on('data', function (chunk) {
    var buffer = new Buffer(chunk);
    buffers.push(buffer);
});
process.stdin.on('end', function () {
    var result = Buffer.concat(buffers);
    console.log(result);
});

// 官方答案
// var buffers = [];
// process.stdin.on('readable', function() {
//   var chunk = process.stdin.read();
//   if (chunk !== null) {
//     buffers.push(chunk);
//   }
// });
// process.stdin.on('end', function() {
//   console.log(Buffer.concat(buffers));
// });
```

# bytewiser-exercise-6

Read the first buffer from process.stdin, copy all bytes into a
Uint8Array and then log out a JSON stringified representation of the typed array.

```
process.stdin.on('data', function (chunk) {
    var uInt8array = new Uint8Array(chunk);
    var result = JSON.stringify(uInt8array);
    console.log(result);
});

// 官方答案
// process.stdin.once('data', function(buff) {
//   var ui8 = new Uint8Array(buff)
//   console.log(JSON.stringify(ui8))
// })
```

# bytewiser-exercise-7

Take the integer from process.argv[2] and write it as the first
element in a single element Uint32Array. Then create a Uint16Array from the Array
Buffer of the Uint32Array and log out to the console the JSON stringified version
of the Uint16Array.

```
var int = parseInt(process.argv[2]);
var uint32array = new Uint32Array(1);
uint32array[0] = int;
var uint16array = new Uint16Array(uint32array.buffer);
var result = JSON.stringify(uint16array);
console.log(result);

// 官方答案
// var num = +process.argv[2]
// var ui32 = new Uint32Array(1)
// ui32[0] = num
// var ui16 = new Uint16Array(ui32.buffer)
// console.log(JSON.stringify(ui16))
```