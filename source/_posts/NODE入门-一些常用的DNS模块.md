title: NODE入门-一些常用的DNS模块
id: 633
categories:
  - Node
date: 2014-03-08 21:09:59
tags:
  - dns
---

记录一些Modules的简单用法，方便快速查阅。

- DNS 解析模块

[javascript]
// 解析DNS
var dns = require('dns');
var href = 'www.laispace.com';
var type = 'A';
// href 待解析的域名字符串, type 表示记录类型的字符串(A-地址解析/CNAME-别名解析/MX-邮件域名解析/SRV-服务记录等);
dns.resolve(href, type, function (err, result) {
 if (err) {
 throw err;
 }
 console.log('DNS解析结果是：', result);
})
[/javascript]

解析结果：

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-08-at-9.04.05-PM.png "Screen Shot 2014-03-08 at 9.04.05 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-08-at-9.04.05-PM.png)

除resolve()方法外，还有lookup()、resolve4()、resolve6()。resolveMx() 等其他方法见：[DNS模块官方文档](http://nodejs.org/api/dns.html#dns_dns_resolve_domain_rrtype_callback)

<!-- more -->

- Crypto 加密模块

[javascript]

// 安装Node时保证添加了 OpenSSL 支持

var crypto = require('crypto');

// 创建 hash 对象实例, 传入 md5/sha1/sha256/sha512/ripemd160
var md5 = crypto.createHash('md5');

var password = 'helloXiaoLai';

console.log('输入的明文是：', password);

// 生成hash
md5.update('myPassword');
// 添加数据更新hash
md5.update('laispace.com');
// 生成密钥, 16进制显示
password = md5.digest('hex');
console.log('输出的密文是：', password);

[/javascript]

加密结果：

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-08-at-9.23.22-PM.png "Screen Shot 2014-03-08 at 9.23.22 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-08-at-9.23.22-PM.png)

除hashing加密外，还有HMAC加密、公钥加密等其他方法见：[Crypto模块官方文档](http://nodejs.org/api/crypto.html#crypto_crypto_createhash_algorithm)

- Process 全局变量

[javascript]
&lt;span style=&quot;font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;&quot;&gt;console.log('版本号是：', process.version);&lt;/span&gt;&lt;/pre&gt;
console.log('运行平台是：', process.platform);

console.log('当前进程已运行的时间：', process.uptime());

// 捕获进程信号量
// 输入
console.log('输入你的姓名:');
// 激活进程输入
process.stdin.resume();
// 设置编码
process.stdin.setEncoding('utf8');
var body = '';
// 监听
process.stdin.on('data', function (chunk) {
 body += chunk;
 // process.stdout.write('数据传输中...' + chunk);
});
process.stdin.on('end', function () {
 process.stdout.write('你好，' + body);
});

// 监听
process.on('SIGINT', function () {
 console.log('捕获到SIGINT事件，按 ctrl+D 退出');
})

// 监听进程退出
process.on('exit', function () {
 console.log('监听到进程退出');
})
&lt;pre&gt;[/javascript]

监听结果：
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-08-at-9.58.58-PM.png "Screen Shot 2014-03-08 at 9.58.58 PM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/Screen-Shot-2014-03-08-at-9.58.58-PM.png)

更多方法见：[Process 进程](http://nodejs.org/api/process.html#process_process)