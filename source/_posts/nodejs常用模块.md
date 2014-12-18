title: NODEJS-常用模块

date: 2014-05-14 09:42:25

categories: Node

tags: [Node modules] 

---

## dns模块 解析域名

- dns.resolve() 将一个域名解析为一组DNS记录

- dns.reverse() 将一个IP地址饥饿虚伪一组域名

- dns.lookup() 将一个域名转换为一个IP地址 

		// rrtype即记录类型，默认为A，可选 A/AAAA/CNAME/MX/TXT/SRV/PTR/NS
		// A 将IPV4地址映射为域名
		// AAAA 将IPV6地址映射为域名
		// CNAME 别名解析，如 laispace.com 是 www.laispace.com 的别名
		// MX 邮件服务器解析
		// dns.resolve(domain, [rrtype], callback)
		var dns = require('dns');
		var domain = 'www.laispace.com';
		dns.resolve(domain, 'A', function (err, addresses) {
			if (err) {
				throw err;
			}
			console.log('域名解析结果为：\n', addresses);
		})

<!--more-->
	
	更便捷的方法：		
	
		// 解析 A 记录
		dns.resolve4(domain, callback)
		// 解析 AAAA 记录
		dns.resolve6(domain, callback)
		// 解析 CNAME 记录
		dns.resolveCname(domain, callback)
		// 解析 MX 记录
		dns.resolveMx(domain, callback)
		// 解析 TXT 记录
		dns.resolveTxt(domain, callback)
		// 解析 SRV 记录
		dns.resolveSrv(domain, callback)
		// 解析 NS 记录
		dns.resolveNs(domain, callback)
		
	dns.resolve() 回调中返回的是 一组地址数组 addresses
	
	dns.lookup() 则返回 addresses 中的第一个：
	
		// family 默认为 null，即可取IPV4或IPV6， 若为 4 或 6，则取为 IPV4 或 IPV6
		// dns.lookup(domain, [family], function (err, address, family) {})
		var dns = require('dns');
		var domain = 'www.laispace.com';
		dns.lookup(domain, 4, function (err, address) {
			if (err) {
				throw err;
			}
			console.log('域名解析结果为：\n', address)
		})	

	dns.reverse() 则反过来解析IP地址为域名：
	
		// dns.reverse(ip, callback)
		var dns = require('dns');
		var ip = '173.194.127.82';
		dns.reverse(ip, function (err, domains) {
			if (err) {
				throw err;
			}
			console.log('IP解析结果为：\n', domains);
		})		

## os模块 获取操作系统信息

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
		
		
## util 模块 一些实用方法

		// 格式化字符串
		// util.format(format, [...])
		util.format('输入%d个参数，分别为%s, %s和%s', 3, 'lai', 'xiao', 'lai');
		%s 字符串参数
		%d 数值参数
		%j JSON对象
		%% 一个百分号
		
		// debug输出标准错误流，同步方法，阻塞线程
		// util.debug(string)
		util.debug('出现错误！');
		
		// error输出标准错误流，也是同步方法，阻塞线程
		// util.error([...])
		util.error(['error1', 'error2', 'error3'])
		
		// puts输出标准输出流，也是同步方法，阻塞线程
		// util.puts([...])
		util.puts(['ouput1', 'output2', 'output3'])
		
		// print输出标准输出流，也是同步方法，阻塞线程，输出后不产生新行
		// util.print([...])
		util.puts(['ouput1', 'output2', 'output3'])
		
		// log输出标准输出流，前面会自动加上系统时间
		// util.log(string)
		util.log('Hello Lai!')
		
		// inspect输出一个对象信息
		// util.inspect(object, [options])
		var obj = {
			name: '小赖1',
			child: {
				name: '小赖2',
				child: {
					name: '小赖3'
				}
			},
			sayHi: function () {console.log('Hi Lai!')}
		}
		util.inspect(obj, {showHidden: true});
		// options = {
		//	showHidden: 布尔值默认false，是否显示对象的不可枚举属性
		//	depth: 整数，指定查看对象的信息的深度
		//	colors: 布尔值默认false，是否输出时应用颜色高亮
		//	customInspect: 布尔值默认为true，是否在查看对象信息时调用自定义的Inspect方法
		// }
		// 设置inspect的默认颜色
		util.inspect.styles = {
			number: 'yellow',
			boolean: 'yellow',
			string: 'green',
			date: 'magenta',
			regexp: 'red',
			null: 'bold',
			undefined: 'grey',
			special: 'cyan'
		}
		
		// isArray 判断数组
		// util.isArray(object)
		util.isArray([1, 2, 3]);
		
		// isRegExp 判断正则
		// util.isRegExp(object)
		util.isRegExp(/a regexp/);
		
		// isDate 判断日期
		// util.isDate(object)
		util.isDate(new Date());
		
		// isError 判断错误
		// util.isError(object)
		util.isError(new Error());
		
		// inherits 将父类方法继承给子类
		// util.inherits(constructor, superConstructor)
		var util = require('util');
		function Father() {}
		Father.prototype = {
			sayHi: function () {
				console.log('Hi Lai');
			}
		}		
		function Child() {}
		// 继承方法
		util.inherits(Child, Father);
		// 生成实例
		var child = new Child()
		child.sayHi(); // => 'Hi Lai'
		
## validator模块 客户端/服务器端验证

		// 使用 npm 或者 bower 安装：
		$ npm install validator
		// 或者 
		$ bower install validator-js
		
		// 使用示例：
		// 服务器端
		var validator = require('validator');
		validator.isEmail('foo@bar.com'); //=> true
		
		// 客户端
		<script type="text/javascript" src="validator.min.js"></script>
		<script type="text/javascript">
		  validator.isEmail('foo@bar.com'); //=> true
		</script>

[地址](https://github.com/chriso/validator.js
)		
	
		
		
		


	
	
