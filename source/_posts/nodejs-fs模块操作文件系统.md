title: NODEJS-fs模块操作文件系统

date: 2014-05-09 21:19:42

categories: Node

tags: [fs, path] 

---
  
## 使用 fs模块 对文件/目录进行操作

- 读取文件内容
		
		// fs.readFile(filename, [options], callback) 
		// options 中的 flag 默认为 r，表示读取文件
		fs.readFile('test.txt', function (err, data) {
			if(err) throw err;
			console.log('文件内容是：', data);
		})
		
		
		// fs.readFileSync(filename, [options]) 
		// 同步方式读取
		try {
			var data = fs.readFileSync('test.txt');
			console.log('文件内容是：', data);
		} catch (err) {
			throw err;
		}

<!--more-->
	
	options 中的 flag 取值：r/r+/rs/w/wx/w+/wx+/a/ax/a+/ax+

	options 中的 encoding 取值：utf8/ascii/base64
	
- 写入文件内容

		// fs.writeFile(filename, data, [options], callback)
		// options 中的 flag 默认为 w，表示写入文件，mode 默认为 0666（可读写的读写权限）
		fs.writeFile('test.txt', '我是被写入的内容', function (err) {
			if (err) throw err;
			console.log('成功写入.')
		})		
		
		// 同步方式写入
		fs.writeFile(filename, data, [options])
		
		// 将 data 添加到文件底部, flag 默认为 a
		fs.appendFile(filename, data, [options], callback)
		// 同步方式添加
		fs.appendFileSync(filename, data, [options])
	
	options 中的 flag 和 encoding 与上同。
	
	options 中的 mode 为表示读写权限的数字，默认为 0666 可读写
	
	写入的 data 可以是一个 Buffer	
	
- 在指定位置读写文件

		fs.open(filename, flags, [mode], function (err, fd){
			// 读
			fs.read(fd, buffer, offset, length, position, function(err, bytesRead, buffer){})
			// 写
			fs.write(fd, buffer, offset, length, position, function(err, written, buffer){})
			// 关闭
			fs.close(fd)
		})
		

- 创建目录

		// fs.mkdir(path, [mode], callback)
		// mode 默认为 0777，表示可读可写
		fs.mkdir('./testDir', function (err) {
			if (err) {
				throw err;
			}
			console.log('目录创建成功')
		})	
- 读取目录

		fs.readdir('./testDir', function (err, files) {
			if (err) {
				console.log('读取目录失败')
			}
			console.log(files);
		})	
		
- 查看文件/目录信息

		// fs.fstat('testDir', function (err, stats) {
		fs.stat('testFile.txt', function (err, stats) {
			if (err) {
				console.log('读取文件信息失败')
			}			
			// stats 是一个 Fs.Stats 对象
			console.log(stats);
			
			console.log('是否为文件：', stats.isFile);
			console.log('是否为目录：', stats.isDictionary);
			console.log('读写权限是：', stats.mode);
			console.log('文件大小是：', stats.size);
			console.log('访问时间是：', stats.atime);
			console.log('修改时间是：', stats.mtime);
			console.log('创建时间是：', stats.ctime);
		})	

- 检查文件/目录是否存在

		fs.exists('./testFile.txt', function (exists) {
			console.log('testFile.txt是否存在：', exists);
		})	

- 获取文件的绝对路径

		fs.realpath('./testFile.txt', function (err, resolvedPath) {
			if (err) {
				throw err;
			}
			console.log('文件的绝对路径是：', resolvedPath);
		})
		
- 修改文件时间

		// fs.utimes(path, atime, mtime, callback)
		fs.utimes('./testFile.txt', new Date(), new Date(), function (err) {
			if (err) {
				console.log('修改失败')
			}
			console.log('修改成功');
		})	
	
- 修改文件/目录的读取权限

		// fs.chmod(path, mode, callback)
		// 0600 表示所有者可读写，其他人不可
		fs.chmod('./testFile.txt', 0600, function (err) {
			if (err) {
				console.log('修改失败');
			}
			console.log('修改成功');
		})
	
- 移动/重命名文件

		// fs.rename(oldPath, newPath, callback);		// oldPath 与 newPath 所在目录相同但文件名不同时，重命名
		// oldPath 与 newPath 所在目录不相同时，移动；若文件名不同，则移动后重命名
		fs.rename('./testFile.txt', './test/testNewFile.txt', function (err) {
			if (err) {
				console.log('文件移动失败');		
			}
			console.log('文件移动成功');
		})
		
- 创建和删除硬连接

	新创建的硬连接与旧的硬连接会指向相同文件
	
	删除的硬连接若是最后一个，则删除这个文件
	
		// 创建
		// fs.link(srcPath, dstPath, callback);
		fs.line('./testFile.txt', './test/testNewFile.txt', function (err) {
			if (err) {
				console.log('创建硬连接失败');
			}
			console.log('创建硬连接成功');
		})	
		// 删除
		fs.unlink(path, callback);

- 截断文件

	清除文件内容，后修改文件尺寸的操作
	
		fs.truncate(filename, len, callback)
		
- 删除空目录

		fs.rmdir(path, callback);
		
- 监视文件/目录

		// fs.watchFile(filename, [options], listener)
		// options 中的 interval 指多久检查一次，这里设定了 1小时
		fs.watchFile('./testFile.txt', {interval: 60*60*1000}, function (curr, prev) {
			if (Date.parse(prev.ctime) == 0) {
				console.log('文件被创建');
			} else if (Date.parse(curr.ctime) == 0) {
				console.log('文件被删除');
			} else if (Date.parse(prev.mtime) != Date.parse(curr.mtime)
				console.log('文件被修改');
			})
		})			
		// 取消监视, 不指定listener则清空所有监视
		fs.unwatchFile(filename, [listener])

- 使用 ReadStream对象 读取文件

		// 创建
		// fs.createReadStream(path, [options])
		// options 可填的key有：flags/encoding/autoClose/start/end
		var file = fs.createReadStream('./testFile.txt');
		var body = '';
		// 监听
		file.on('open', function (fd) {
			console.log('文件被打开，开始读取...');
		});
		file.on('data', function (data) {
			console.log('正在读取数据...');
			body += data;
		});
		file.on('end', function(){
			console.log('文件被读取完毕：');
			console.log(body);
		});
		file.on('close', function () {
			console.log('文件已关闭');
		})
		file.on('error', function (err) {
			console.log('文件读取出错');
		})
		
		// 手动出发暂停/恢复读取文件
		// file.pause();
		// file.resume();

- 使用 WriteStream对象 写入文件

		// 创建
		// var writable = fs.createWriteStream(path, [options])
		// options 可填的key有：flags/encoding/start
		
		// writable.write(chunk, [encoding], [callback])		// chunk 可为 Buffer/string
		
		// writable.end([chunk], [encoding], [callback])
		// 调用end方法结束写入
		
		// 将 file1.txt 的内容写入 file2.txt
		var file1 = fs.createReadStream('./file1.txt');
		var file2 = fs.creatWriteStream('./file2.txt');
		file1.on('data', function (data) {
			file2.write(data);
		});
		file1.on('end', function () {
			file2.end(function() {
				console.log('文件写入完成');
				console.log('共写入的 %d 字节数据', file2.bytesWritten);
			});
		});		
	
	
		// 使用 pipe方法 写入数据
		// readStream.pipe(destination, [options])
		var file1 = fs.createReadStream('./file1.txt');
		var file2 = fs.createWriteStream('./file2.txt');
		// options 中的 end 设置为 false 表示不会自动关闭文件
		file1.pipe(file2, {end: false});
		// 手动关闭
		file1.on('end', function () {
			// 这样可以继续写入数据再关闭
			file2.end('我是被继续写入的数据');
		});
		
		// 取消写入
		// readStream.unpipe([destination])
		

## 使用 path模块 对路径进行操作	

		// 将非标准路径转化为标准路径
		var unStdPath = './..//testFile.txt';
		var stdPath = path.normalize(unStdPath);
		
		// 将多个字符串拼接
		var newPath = path.join(__dirname, 'aaa', 'bbb', 'ccc');
		
		// 解析出绝对路径
		path.resolve(path1, [path2])	
		
		// 找出两个路径间的关系
		path.relative(from, to)
		
		// 获取某路径中的目录名
		path.dirname(path1)
		
		// 获取某路径中的文件名，去除 ext后缀		
		path.basename(path1, [ext])
		
		// 获取某路径中的拓展名
		path.extname(path1)
	

			
	
		
		
	

				
		
			
	
		
