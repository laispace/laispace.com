title: NODEJS-编写命令行脚本

date: 2014-06-04 09:12:18

categories: Node

tags: 
    - node

---


# 在Shell中运行Node程序

方法1 - 指定node程序和需要运行的脚本：
	
		$ node app.js
		
方法2 - 使用 #! 将文件声明为可执行文件：

		// app.js #!表示调用解释器，执行 /usr/local/bin/node 的指令
	
		// 静态设置node程序路径 通过 $ which node 查询到
		#!/usr/local/bin/node		
		console.log('Hello Lai')
		
		// 动态设置node程序路径，使用 env 指令查找 PATH 环境变量存储的 node 路径
		#!/usr/bin/env node
		console.log('Hello Lai')
		
同时记得要将文件权限改为可执行：

		$ chmod 755 app.js
		// 或
		$ chmod +x app.js
		
接着直接运行脚本就可以了：

		$ ./app.js
		
<!-- more -->
		
# 向脚本传递参数

方法1 - 在指定node程序时，直接传入参数：

		// app.js
		console.log(process.argv)

		// 启动程序时传参
		$ node app.js "argv1" "argv2" "argv3..." 
				
方法2 - 使用 #! 启动时，参数会直接传递给程序
		
		// app.js
		#!/usr/local/bin/node		
		console.log(process.argv);
		
		// 执行脚本时自动传入
		$ ./app.js "argv1" "argv2" "argv3..."
		

		
# 使用同步方式处理文件

命令行脚本，同步方式处理非常重要。


# 处理标准输入和输出

Node 中的 console.log 和 console.error 等价于：

		process.stdout.write(text + '\n');
		process.stderr.write(text + '\n');
		

- 缓冲输入输出

	即逐行输入输出，按下Enter回车键才从数据流中读取数据，通过 readable 事件实现：
	
		process.stdin.on('readable', function () {
			var data = process.stdin.read();
			
			console.log('输入的是：', data);
		})		
		
	默认情况下，输入流处于暂停状态，调用 resume 函数 才能从输入流中接收数据
	
		process.stdin.resume();
		

	示例 - 将逐行输入的字符串进行md5处理后再输出：
	
		process.stdout.write('Hello 小赖！\n');
		process.stdout.write('按 Ctrl+C 或输入空行退出\n');
		process.stdout.write('请输入：');
		
		process.stdin.on('readable', function () {
			var data = process.stdin.read();
			if (data == null) return;
			// 输入空行退出
			if (data == '\n') process.exit(0);
			
			// 依赖模块
			var hash = require('crypto').createHash('md5');
			hash.update(data);
			
			process.stdout.write('加密后: ' + hash.digest('hex') + '\n');
		
			// 继续输入
			process.stdout.write('\n请继续输入：');
		});
		
		// 设置编码
		process.stdin.setEncoding('utf8');
		// resume 方法可保证只有用户手动才能终止程序
		process.stdin.resume();	
						
- 无缓冲输入输出

	即逐字输入输出，需要开启stdin.setRawMode来启动原始模式
	
      	#!/usr/bin/env node
      	
  		process.stdout.write('Hello 小赖！\n');
  		process.stdout.write('按 Ctrl+C 或输入空行退出\n');
  		process.stdout.write('请输入：');

  		process.stdin.on('readable', function () {
  			var data = process.stdin.read();
        // console.log(data);

        if (data == null) return;

        // 未启用原始模式
        if (!process.stdin.isRaw) {
          if (data == '\n') {
            process.exit(0);
          }

          process.stdout.write('请选择一个加密类型 ');
          process.stdout.write('1-md5, 2-sha1, 3-sha256, 4-sha512');
          process.stdout.write('\n请选择数字[1-4]：');

          // 打开原始模式
          process.stdin.setRawMode(true);
        // 启用原始模式
        } else {
          var alg;
          // 未按下 CTRL+C
          if (data != '^C') {
            var c = parseInt(data);
            switch (c) {
            case 1: alg = 'md5'; break;
            case 2: alg = 'sha1'; break;
            case 3: alg = 'sha256'; break;
            case 4: alg = 'sha512'; break;
            }

            // 使用用户选择的算法进行加密
            if (alg) {
              // 依赖模块
              var hash = require('crypto').createHash(alg);
              hash.update(data);
              process.stdout.write('使用'+ alg +'加密后: ' + hash.digest('hex') + '\n');
              // 继续输入
              process.stdout.write('\n请继续输入：');
              // 关闭原始模式
              process.stdin.setRawMode(false);
            } else {
              // 未输入算法类型
              process.stdout.write('请选择一个加密类型 ');
              process.stdout.write('1-md5, 2-sha1, 3-sha256, 4-sha512');
              process.stdout.write('\n请选择数字[1-4]：');
            }
          // 按下 CTRL+C 退出
          } else {
            process.stdout.write('\n请输入：');
            // 关闭原始模式
            process.stdin.setRawMode(false);
          }
        }
      });

  		// 设置编码
  		process.stdin.setEncoding('utf8');
  		// 继续
  		process.stdin.resume();
			

# 使用 readline 模块

使用这个模块来逐行读取文件

	官方样例：
	
		var readline = require('readline');
		var rl = readline("./somefile.txt");
	    rl.on("line", function (line){
		    //do something with the line of text
		});
	    rl.on('error', function (e){
		   //something went wrong
	    });		



# 使用 conmmander 模块

方便快速构建命令行工具

#!/usr/bin/env node

	官方样例：
	
		/**
		 * Module dependencies.
		 */

		var program = require('commander');

		program
		  .version('0.0.1')
		  .option('-p, --peppers', 'Add peppers')
		  .option('-P, --pineapple', 'Add pineapple')
		  .option('-b, --bbq', 'Add bbq sauce')
		  .option('-c, --cheese [type]', 'Add the specified type of cheese [marble]', 'marble')
		  .parse(process.argv);

		console.log('you ordered a pizza with:');
		if (program.peppers) console.log('  - peppers');
		if (program.pineapple) console.log('  - pineapple');
		if (program.bbq) console.log('  - bbq');
		console.log('  - %s cheese', program.cheese);
		
	使用 conmmander 模块，调用Github的API写一个简单的 repo 查询工具：

        #!/usr/bin/env node

        // 使用 commander 模块快速构建命令行工具应用
        // 使用 request 模块发起网络请求
        // 使用 chalk 模块美化命令行输出
        var program = require('commander');
        var request = require('request');
        var chalk = require('chalk');

        program
          .version('0.0.1')
          .usage('[options] <keywords>')
          .option('-o, --owner [name]', 'Filter by the repositories owner')
          .option('-l, --language [language]', 'Filter by the repositories language')
          .option('-f, --full', 'Full output without any styling')
          .parse(process.argv);

        // console.log(program.args.length)
        // 未输入参数，则输出帮助
        if (!program.args.length) {
          program.help();
          // 带错误退出
          process.exit(1);
        } else {
          var keywords = program.args;
          var url = 'https://api.github.com/search/repositories?sort=stars&order=desc&q='+keywords;

          // 处理输入的 --option
          if(program.owner) {
                url = url + '+user:' + program.owner;
          }

          if(program.language) {
              url = url + '+language:' + program.language;
          }


          // console.log('Keywords: ' + program.args);
          var options = {
            method: 'GET',
            headers: {
              'User-Agent': 'laispace'
            },
            url: url
          }
          request(options, function (err, res, data) {
            if (err) {
              console.log('Error: ' + err);
            } else if (!err && res.statusCode == 200) {
              var body = JSON.parse(data);
              console.log('查询结果 '+ body.items.length +'\n');
              if (program.full) {
                // 无过滤输出
                console.log(body);
              } else {
                  for(var i = 0; i < body.items.length; i++) {
                    console.log(chalk.cyan.bold.underline('Name: ' + body.items[i].name));
                    console.log(chalk.magenta.bold('Owner: ' + body.items[i].owner.login));
                    console.log(chalk.grey('Desc: ' + body.items[i].description));
                    console.log(chalk.grey('Clone url: ' + body.items[i].clone_url + '\n'));
                  }

                  // 无错误退出
                  process.exit(0);
              }
            }
          });

        }

	记得将gitsearch.js的权限修改为可执行.
	
	测试命令：
		
		// repo名为lai的repo
		$ ./gitsearch.js lai
		// 作者为laispace
		$ ./gitsearch.js lai -o laispace
		// 语言分类为 javascript
		$ ./gitsearch.js lai -o laispace -l javascript
		// 不过滤输出
		$ ./gitsearch.js lai -o laispace -l javascript -f
		// 将结果复制到剪贴板
		$ ./gitsearch.js lai -o laispace | pbcopy		// 将结果分页输出（vim模式）
		$ ./gitsearch.js lai -o laispace | less
		// 将结果正则匹配后输出，这里将输出匹配有 'node' 的信息
		$ ./gitsearch.js lai -o laispace | grep node
		
		
					
### 参考资料

1. [readline 模块](https://www.npmjs.org/package/readline)
2. [commander 模块](https://github.com/visionmedia/commander.js/)
3. [chalk 模块](https://www.npmjs.org/package/chalk)
4. [Gtihub API](https://developer.github.com/v3/)
4. [Command-line utilities with Node.js](http://cruft.io/posts/node-command-line-utilities/)
	
		
		


				
		

						