title: NODEJS-进程与子进程

date: 2014-06-05 14:40:49

categories: Node

tags: [Node, Child_process, Cluster]

---

在操作系统中，每一个程序都是一个进程类的实例对象。

# 进程信息

在 NodeJS 中，使用 process 代表 NodeJS 应用程序。

## 进程属性

		// 执行程序的绝对路径
		process.execPath
		
		// NodeJS版本号
		process.version
		
		// NodeJS 及其依赖的版本号
		process.versions
		
		// 运行平台
		process.platform
		
		// 一个标准输入流对象
		process.stdin
		// 恢复读取标准输入流数据
		process.stdin.resume()
		
		// 一个标准输出流对象
		process.stdout
		
		// 一个标准错误流对象
		process.stderr
		
		// 参数列表
		process.argv
		
		// 环境变量
		process.env
		
		// 配置信息
		process.config
		
		// 进程 id
		process.pid
		
		// 命令行窗口的标题
		process.title
		
		// 处理器架构
		process.arch

<!-- more -->		
	
## 进程方法：
	
		// 进程内存使用量
		process.memoryUsage()		
		
		// 将callback函数推迟执行
		process.nextTick(callback)
		// 示例：
		var fs = require('fs');
		var finish = function () {
			console.log('文件读取完成')
		}
		process.nextTick(finish);
		console.log(fs.readFileSync('./test.txt').toString());
		
		// 发出 SIGABRT 新号，终止进程	
		process.abort()
		
		// 改变程序使用的当前目录
		process.chdir(directory)
		
		// 返回当前目录
		process.cwd()
		
		// 退出程序进程，code 为 0 表示正常退出
		process.exit([code])
		
		// 获取 group id
		process.getgid()
		// 设置
		process.setgid(id)
		
		// 获取 user id
		process.getuid()
		// 设置
		process.setuid(id)
		
		// 获取附属组 id 构成的数组
		process.getgroups()
		// 设置
		process.setgroups(groups)
		
		// 指定归属组来初始化 /etc/group 组列表
		process.initgroups(user, extra_group)
		
		// 向一个进程发送信号, signal 默认为 SIGTERM 表示终止进程
		process.kill(pid, [signal])
		
		// 读取或修改运行程序进程的文件的权限掩码
		process.unmash([mask])
		
		// 读取程序当前运行时间
		process.uptime()
		
		// 测试一个代码段的运行时间
		process.hrtime()
		// 示例：
		var fs = require('fs');
		var time = process.hrtime();
		var data = fs.readFileSync('./test.txt');
		var diff = process.hrtime(time);
		console.log('读取文件耗费%d纳秒', diff[0] * 1e9 + diff[1])
		
## 进程事件：
	
		// exit - 进程退出时触发
		process.on('exit', function () {
			console.log('进程已退出');
		});
		// 手动退出进程
		process.exit();
		
		// uncaughtException - 未捕获异常
		process.on('uncaughtException', function (err) {
			console.log('未捕获异常：', err);
		});
		// 调用一个未定义的函数
		nonExectentFunction();
		
		// 其他信号事件, 如 SIGINT
		process.on('SIGINT', function () {
			console.log('捕获到SIGINT信号');
		});
		process.stdin.resume();

		

# 系统信息

此外，可以使用os模块来访问类似的操作系统信息：

		// 临时文件目录
		os.tmpdir()   
		 
		// CPU的字节序
		os.endianness()
		
		// 计算机名
		os.hostname
		
		// 操作系统类型
		os.type
		
		// 操作系统平台
		os.platform
		
		// CPS架构
		os.arch()
		
		// 操作系统版本号
		os.release()
		
		// 系统当前运行时间
		os.uptime()
		
		// 1、5、15分钟的系统平均负载
		os.loadavg()
		
		// 系统总内存，以字节为单位
		os.totalmem()
		
		// 系统空闲内存
		os.freemem()
		
		// CPS信息
		os.cpus()
		
		// 系统中所有网络接口
		os.networkInterfaces()
		
		// 系统使用的换行符
		os.EOL 		
		
		

# 使用 child_process 模块开启子进程

子进程间共享内存空间，也可通过共享端口将请求分配给多个子进程来处理。

## 使用 spawn 方法开启子进程

		child_process.spawn(command, [args], [options])
		
		command 指定运行命令
		
		args 数组，指定运行该命令时使用的参数
		
		options	对象，指定开启子进程时使用的选项
		
		// 默认值为：
		options = {
			cwd: undefined,
			env: process.env
		}
		
		options = {
			cwd: 字符串，指定子进程的当前工作目录
			stdio: 字符串或三元素数组，设置标准输入输出
			env: 对象，指定环境变量
			detached: 布尔值默认为false，是否设定为进程组中的组长进程
			uid: 数值，指定 user id
			gid: 数值，指定 group id
		}
		
		options.stdio = 'ignore'
		等同于：
		options.stdio = ['ignore', 'ignore', 'ignore'];

		options.stdio = 'pipe'
		等同于：
		options.stdio = ['pipe', 'pipe', 'pipe'];

		options.stdio = 'inherit'
		等同于：
		options.stdio = [process.stdin, process.stdout, process.stderr];
		或：
		options.stdio = [0, 1, 2];
		
		options.stdio 中的数组元素可为：
		pipe 在父子进程间创建管道
		ipc 在父子进程间创建专用于传递消息或文件描述符的IPC通道
		ignore 不为子进程设置文件描述符
		
		Stream对象 指定父子进程共享一个终端设备、文件、端口或管道
		
		null或undefined 使用默认值
		

	spawn 方式示例：
	
		var cp = require('child_process');
		
		// TODO
		// p239 代码清单 9-13
		
		
		
				
			
		

















