title: NODEJS-Buffer对象

date: 2014-05-09 21:19:42

categories: Node

tags: [buffer] 

---
 

- 构造一个指定大小的 buffer

		var newBuffer = new Buffer(1024);
		// var len = newBuffer.length;
- 用指定值初始化 buffer 的内容

		// 填充第10字节开始的内容为 1 
		newBuffer.fill(1, 10);
		// 填充第10~20字节的内容为 2 
		newBuffer.fill(2, 10, 20);
		
- 用数组初始化 buffer 的内容
		
		// 用数组 [0, 1, 2] 初始化
		var newBuffer = newBuffer([0, 1, 2]) 
		
- 用字符串初始化 buffer 的内容

		// 用字符串 'xiaolai' 初始化
		var newBuffer = new Buffer('xiaolai');
		// 指定编码, 默认为 utf8，可选 ascii/utf8/utf16le/ucs2/base64/hex 等编码格式
		// var newBuffer = new Buffer('xiaolai', 'utf8');

<!--more-->
		
- 取出 buffer 中的字节		
		
		// 取出第2~4个字节
		var buff = newBuffer.slice(2, 4);	

- 将 buffer 转化为字符串

		// Buffer.toString([encoding], [start], [end])
		// 默认以uft-8 编码，将 buffer 转化为字符串
		var str = newbuffer.toString();
		// 以uft-8 编码，将第4~10字节转化为字符串
		var str = newbuffer.toString('utf8', 4, 10);
	
- 将字符串写入已有的 buffer 中

		// Buffer.write(string, [offset], [length], [encoding]);
		// 在 newBuffer 的第3个字节后插入长度为3的字符串 "赖"
		newBuffer.write('赖', 3, 3)	
		
- 使用 string_decoder 模块解决中文被截断乱码的问题

	使用场景：在 utf-8 编码中，『赖小赖』这三个字占用9个字节，如果将这九个字节分为两个buffer，一个5字节另一个4字节，分别打印时就会乱码了（因为一个汉子占用3字节）. 在遇上长字符串时虽然可以将多个这样的buffer使用concact()方法合并后再调用toString()方法输出，但性能不好。

		// 使用 string_decoder 模块解决这个问题
		var StringDecoder = require('string_decoder').StringDecoder;
		var decoder = new StringDecoder();
		// 分别解码
		decoder.write(str1); // '赖'
		decoder.write(str2); // '小赖'
		
- 使用 JSON.stringify() 将 buffer 转换为字符串

		var newBuffer = new Buffer('我叫赖小赖');
		var json = JSON.stringify(newBuffer);
		//使用 JSON.parse() 将转化后的字符串转化为数组
		var arr = JSON.parse(json);
		
- 复制 buffer 到另一个 buffer

		// Buffer.copy(targetBuffer, [targetStart], [sourceStart], [sourceEnd])
		var oldBuffer = new Buffer('我叫赖小赖');
		var newBuffer = new Buffer(1024);
		// 将 oldBuffer 复制到 newBuffer;
		oldBuffer.copy(newBuffer);	

- Buffer.isBuffer(object) 判断是否为 Buffer对象			

		var isObjBuffer = Buffer.isBuffer(obj);
		
- Buffer.byteLength(string, [encoding]) 计算字符串的字节数

		var str = '赖小赖';
		var byteLen = Buffer.byteLength(str);	

- Buffer.concat(BufferList, [totalLength]) 将多个 Buffer 合并为一个

		var str1 = new Buffer('我');	
		var str2 = new Buffer('叫');	
		var str3 = new Buffer('小');	
		var str4 = new Buffer('赖');
		var str5 = Buffer.concat([str1, str2, str3, str4]); 
		console.log(str5.toString()); // "我叫小赖"	
	

	
			
	


			
	