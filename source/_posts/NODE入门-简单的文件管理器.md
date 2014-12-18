title: NODE入门-简单的文件管理器
tags:
  - node
  - fs
categories:
  - Node
date: 2014-01-17 22:55:49
---

今天学习了一个简单的CLI文件管理器的编写：

## 安装方法：

- 运行 $ node index.js后会列出当前文件夹下的文件或文件夹
- 选择文件，则显示这个文件内容
- 选择文件夹，则列出这个文件夹下的文件

> Hello, 这是小赖的第一个nodejs命令行程序！

<!-- more -->

    // index.js
    // fs 模块是唯一一个同时提供异步和同步API的模块
    var fs = require('fs');
    // !!! 定义快捷变量
    var stdin = process.stdin,
        stdout = process.stdout;
    // #1 获取文件列表
    fs.readdir(__dirname, function(err, files) {
        // __dirname 即当前文件夹的路径
        // console.log('    当前文件夹路径：', __dirname);
        // 当前文件夹下的文件
        // console.log('    文件夹下的文件：', files);
    })
    // #2 列出当前目录下的文件，然后等待用户输入
    // process.cwd()返回当前的工作目录，http://nodejs.org/api/process.html#process_process_cwd
    fs.readdir(process.cwd(), function(err, files) {
        // 将下面出现的Stat对象存储起来
        var stats = [];
        // 为了界面友好，输出一个空行
        console.log('');
        if (!files.length) {
            return console.log('    目录下没有文件存在！ \n');
        }
        console.log('   当前目录下的文件或目录列表：\n');
        function file(i) {
            var filename = files[i];
            // fs.stat(path, callback)输出文件的状态信息，http://nodejs.org/api/fs.html#fs_fs_stat_path_callback
            fs.stat(__dirname + '/' + filename, function(err, stat) {
                // 将当前Stat对象存到数组中供以后使用
                stats[i] = stat;
                // 判断是否为文件夹
                if (stat.isDirectory()) {
                    console.log('   ' + i + ' ' + filename + '/');
                } else {
                    console.log('   ' + i + ' ' + filename + ' ');
                }
                // 继续遍历文件
                if (++i == files.length) {
                    // !!!
                    read();
                } else {
                    file(i);
                }
            });
            function read() {
                console.log('');
                // process.stdout.write()与console.log()不同在于无需换行就可直接输入，http://nodejs.org/api/process.html#process_process_stdout
                stdout.write('  请选择：');
                // process.stdin.resume() 等待用户输入
                stdin.resume();
                // 设置编码为utf8才支持特殊字符
                stdin.setEncoding('utf8');
                // 监听用户输入
                stdin.on('data', option);
            }
            // 监听用户输入后调用的函数
            function option(data) {
                // 用户输入的不是数字,不匹配files数组的下标
                var filename = files[Number(data)];
                if (!filename) {
                    stdout.write('  请选择：');
                } else {
                    // 用户输入通过，pause()确保再次将流暂停（回到默认状态）
                    stdin.pause();
                    // 读取这个文件，以utf8编码，http://nodejs.org/api/fs.html#fs_fs_readfile_filename_options_callback | http://www.cnblogs.com/yangdejin/archive/2012/03/01/2375123.html
                    // 读取的是目录
                    if (stats[Number(data)].isDirectory()) {
                        fs.readdir(__dirname + '/' + filename, function(err, files) {
                            console.log('');
                            console.log('   文件夹' + filename + '下有' + files.length + '个文件：');
                            files.forEach(function(file) {
                                console.log('   - ' + file);
                            });
                            console.log('');
                        });
                    } else {
                        // 读取的是文件
                        fs.readFile(__dirname + '/' + filename, 'utf8', function(err, data) {
                            console.log('');
                            // 使用正则，添加辅助性的缩进来输出，正则入门http://deerchao.net/tutorials/regex/regex.htm
                            console.log('   文件' + filename + '的内容是：\n', data.replace(/(.*)/g, '      $1'))
                        })
                    }
                }
            }
        }
        // 初始遍历第一个文件或文件夹
        file(0);
    });


- 执行参数 process.argv 包含node程序运行时的参数值：node argv.js

  - 第一个参数是 'node'
  
  - 第二个参数是 argv.js的路径
  
  - 第三个参数是 传给argv.js 的参数
  
  - 要获取真正传给argv.js的参数，使用 process.argv.slice(2) 即可
  
- 文件路径 __dirname 和 process.cwd 的区别

  - __dirname 获取的是文件在文件系统中的目录
  
  - process.cwd 获取的是当前工作目录
  
  - 使用 process.chdir 方法可以改变灵活地工作目录 

- 环境变量 process.env 可以访问 shell 环境下的变量

- 退出程序 process.exit()  

$ node 进入node环境后，输入 process.exit(1) 就可以退出，不用老是 CTRL C 来退出啦

- fs.readFile('myFile.txt'， callback) 和 fs.creatrReadStream('myFile.txt') 的区别
 
  - fs.readFile('myFile.txt', funtion(err, data)) 是当 myFile.txt 完全读取完后才执行回调函数
  - 当 myFile.txt 这个文件非常大，甚至读取的视频的时候呢？延迟就会非常厉害了
  - fs.createReadStream() 创建可读的 Stream 对象，分块处理：
    
    var stream = fs.createReadStream('myFile.txt');     
    stream.on('data', function(chunk){
      // chunk 是传输的内容块        
    });
    stream.on('end', function(chunk){
      // 文件全传输完毕
    });