title: XMLHttpRequest 对象新特性
categories:
  - Javascript
date: 2014-07-21 06:22:21
tags:
	- ajax
	
---

# 旧版本

```
// 新建实例
var xhr = new XMLHttpRequest();

// 向服务器发出请求
xhr.open('GET','example.php');
xhr.send();

// 监控xhr对象的状态变化，制定回调函数
xhr.onreadystatechange  = function(){
     if(xhr.readyState ==4 && xhr.status ==200){ // 4表示数据接收完毕，200表示服务器返回一切正常
          alert( xhr.responseText ); 
     } else {
          alert( xhr.statusText );
     }
}
```

<!-- more -->

- xhr.readyState：XMLHttpRequest对象的状态，等于4表示数据已经接收完毕。

- xhr.status：服务器返回的状态码，等于200表示一切正常。

- xhr.responseText：服务器返回的文本数据

- xhr.responseXML：服务器返回的XML格式的数据

- xhr.statusText：服务器返回的状态文本。


## 旧版本的缺点

- 只支持文本数据传送，不能读取和上传二进制文件

- 没有进度提示，只能提示有没有完成

- 有同域限制


# 新版本


## 新版本的改进

- 可设置HTTP请求时限

- 可用FormData对象管理表单数据

- 可上传文件、读取服务器端的二进制信息

- 有进度信息

- 可跨域请求

```
// 设置请求时限
xhr.timeout = 3000; // 过了 3s 就停止HTTP请求
//请求超时的毁掉函数
xhr.ontimeout = function(event){
     console.log('Time out!');
}

// 新建FormData对象（HTML5）
var formData = new FormDate();
// 可在这个 FormData 对象中添加表单项
formData.append('name','xiaolai');
formData.append('id',123456);
// 发送该表单，与提交网页表单效果一样
xhr.send(formData);

// 获取页面中表单
var form = document.getElementById('myform');
// 生成 FormData 对象表单
var formData = new FormData(form); 
// 可继续添加表单项，如 csrf
formData.append('csrfToken','123456789'); 
//发送表单
xhr.open('POST',form.action);             
xhr.send(formData);

// 新酷特性：可上传文件！
// 假定 files 是 input[type="file"] 的元素
vat formData = new FormData();
var files = document.getElementById('myFiles');
// 注意可能是多文件
for(var i = 0; i < files.length; i++) {
     formData.append('files[]',files[i]);
}
// 发送啦！
xhr.send(formData);
```

## 注意几点：

- 跨域请求 Cross-origin resource sharing 前提是浏览器的支持且服务器同意
	
	写法与不跨域请求的写法一样

	```
	xhr.open('GET','http://other.server/and/path/to/script');
	```

- 读取二进制数据
	
	- 方法1：改写MIMEType属性
	
	- 方法2：改变 responseType属性

- 显示进度的 progress 事件

	- 下载的progress事件属于XMLHttpRequest对象
	
	- 上传的progress事件属于XMLHttpRequest.upload对象










