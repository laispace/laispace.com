title: learnyounode 练习
date: 2014-05-21 10:58:08
categories: Node
tags: 
  - node
  - learnyounode

---

> learnyounode 是 nodeschool.io 出品的nodejs入门练习项目

> 通过这个练习算是对nodejs有了个入门的认识吧，边学边敲边写笔记
      

- 2.输入任意数字求和.js

      // 输入任意个数字，输出这任意个数字的和
      // process.argv 变量保存了输入的参数，注意第一个永远为'node',第二个是执行路径'path/to/my/file'，第三个开始才是我们真正传入的参数
      // 传入的参数当做字符串了，注意类型转换，如下面Number() 将数字字符转换为数字再进行计算

      var len = process.argv.length;
      // slice(2) 截取数组
      var numbers = process.argv.slice(2);
      var sum = 0;
      for (var i = numbers.length - 1; i >= 0; i--) {
            sum += Number(numbers[i]);
      };
      console.log(sum);

<!-- more -->      
      
- 3.fs.readFileSync同步方式读取文件.js

      var fs = require('fs');

      // 用同步方法读取README.md,以utf8编码
      var path = process.argv[2];
      var data = fs.readFileSync(path, 'utf8');
      // console.log(data);

      // 打印这个文件的行数
      var len = data.toString().split('\n').length;
      // 注意：长度减一才是真正的行数！
      console.log(len-1)
            
- 4.fs.readFile异步方式读取文件.js

      var fs = require('fs');
      // 用异步方法读取README.md,以utf8编码
      var path = process.argv[2];
      fs.readFile(path, 'utf8', function (err, data) {
            if (err) {
                  throw err;
            }
            // console.log(data);
            // 打印这个文件的行数，指定了上述utf8时才能省略以下的toString()方法
            var len = data.toString().split('\n').length;
            // 注意：长度减一才是真正的行数！
            console.log(len-1);
      });
            
- 5.fs.readdir读取某目录下文件并过滤输出.js

      var fs = require('fs');
      // 用异步方法读取参数1指定的目录下的文件列表，以参数2为过滤条件
      var dir = process.argv[2];
      var filter = process.argv[3];
      fs.readdir(dir, function (err, data) {
            if (err) {
                  throw err;
            }
            // 显示目录下的所有文件, 文件数为data.length
            // console.log(data);
            // 遍历，以参数2提供的后缀进行过滤
            // 构造正则，https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp
            var reg = new RegExp('\\.' + filter + '$');
            for (var i = data.length - 1; i >= 0; i--) {
            // 如果文件后缀符合参数2 则添加到新数组中
                  if (reg.test(data[i])) {
                        console.log(data[i]);
                  }
            }
      });
      
- 6.module.export使用的模块 mymodule.js

      var fs = require('fs');
      function readAdir(dir, filter, callback) {
            fs.readdir(dir, function (err, data) {
                  if (err) {
                        // 将错误传给回调函数处理
                        return callback(err);
                  } else {
                        var reg = new RegExp('\\.' + filter + '$');
                        var newData = [];
                        for (var i = 0; i < data.length; i++) {
                              if (reg.test(data[i])) {
                                    newData.push(data[i]);
                              }
                        }
                        // 将处理后的数据传给回调函数处理
                        callback(null, newData);
                  }
            });
      }
      // 导出模块
      module.exports = readAdir;
      
      
- 7.http.get简单的http客户端.js

      var http = require('http');
      var url = process.argv[2];      
      http.get(url, function (res) {
            // 设置编码
            res.setEncoding('utf8');
            // 监听数据
            res.on('data', function (data) {
                  console.log(data);
            });
            // 数据传输完成
            res.on('end', function (data) {
                  console.log('数据接受完成！', data);
            });

            // 数据传输错误
            res.on('error', function (err) {
                  console.log('啊哦~发生了错误。')
            });
      })
            
- 8.http.get简单的http客户端-输出完整数据流.js

      var http = require('http');
      var url = process.argv[2];
      http.get(url, function (res) {
            // 设置编码
            res.setEncoding('utf8');
            var result = '';
            // 监听数据
            res.on('data', function (data) {
                  result += data;
            });
            // 数据传输完成，将完整的数据流输出
            res.on('end', function (data) {
                  console.log(result.length);
                  console.log(result);
            });
            // 数据传输错误
            // res.on('error', function (err) {
                  // console.log('啊哦~发生了错误。', err)
                  // });
      });
      
      
- 9.http.get简单的http客户端-同步get操作.js

      var http = require('http');
      var urls = process.argv.slice(2);
      var results = [];
      var count = 0;
      for (var i=0;i<urls.length;i++) {
            // 用闭包来保证顺序同步操作
            ;(function(i){
                  http.get(urls[i], function (res) {
                        var result = '';
                        res.on('data', function (data) {
                              result += data;
                        });
                        res.on('end', function (data) {
                              results[i] = result;
                              count++;
                              // 计数器达到url总数，说明完成了顺序同步操作，输出结果
                              if (count === urls.length) {
                                    for (var j=0;j<count;j++) {
                                          console.log(results[j]);
                                    }
                              }
                        })
                  })
            })(i);
      }
            
- 10.net.createServer简单的TCP服务器端.js

      var net = require('net');
      var server = net.createServer(function (socket) {
            // 获取当前年、月、日、小时、分钟
            var date = new Date(),
            y = date.getFullYear(),
            m = date.getMonth() + 1,
            d = date.getDate(),
            h = date.getHours(),
            minutes = date.getMinutes();
            // 格式化
            y = y< 10 ? '0'+y: y,
            m = m< 10 ? '0'+m: m,
            d = d< 10 ? '0'+d: d,
            h = h< 10 ? '0'+h: h,
            minutes = minutes< 10 ? '0'+minutes: minutes;
            var dateString = y + '-' + m + '-' + d + ' ' + h + ':' + minutes + '\n';
            // console.log(dateString);
            // socket.write(data);
            // 输出
            socket.end(dateString);
      });
      // 监听参数指定的端口
      var port = Number(process.argv[2]);
      server.listen(port);
      
      
- 11.createReadStream 输出文件内容.js

      var http = require('http');
      var fs = require('fs');
      var portNo = process.argv[2];
      var filePath = process.argv[3];
      var server = http.createServer(function (req, res) {
            // fs,createReadStream 返回 ReadStream 对象 http://nodejs.org/api/fs.html#fs_fs_createreadstream_path_options
            var readStream = fs.createReadStream(filePath);
            var body = '';
            readStream.on('data', function (chunk) {
                  body += chunk;
            });
            readStream.on('end', function () {
                  res.writeHead(200);
                  res.end(body.toString());
            });
      });
      server.listen(portNo);

      // 官方答案
      // var http = require('http')
      // var fs = require('fs')

      // var server = http.createServer(function (req, res) {
            // fs.createReadStream(process.argv[3]).pipe(res)
      // })
      // server.listen(Number(process.argv[2]))
            
- 12.使用through2-map将POST数据转成大写输出

      var http = require('http')
      var map = require('through2-map')
      var server = http.createServer(function (req, res) {
            // 非POST请求
            if (req.method != 'POST') {
                  res.end();
            }
            req.pipe(map(toUpper)).pipe(res);
      });
      function toUpper (chunk) {
            return chunk.toString().toUpperCase();
      }
      server.listen(process.argv[2]);
      