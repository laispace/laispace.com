title: Cookies学习笔记
tags:
  - cookies
id: 513
categories:
  - Javascript
date: 2013-10-06 11:45:17
---

Cookie 的格式是：

[javascript]
name=&lt;value&gt;[; expires=&lt;date&gt;][; domain=&lt;domain&gt;][; path=&lt;path&gt;][; secure]
//名称=&lt;值&gt;[; expires=&lt;日期&gt;][; domain=&lt;域&gt;][; path=&lt;路径&gt;][; 安全]
[/javascript]

// 设置cookie

[javascript]
document.cookie=&quot;key=escape(value)&quot;;
[/javascript]

//escape()函数进行编码，它能将一些特殊符号使用十六进制表示，例如空格将会编码为“20%”，从而可以存储于cookie值中，而且使用此 种方案还可以避免中文乱码的出现。在取值的时候需要unescape(value)对value再进行转码即可。

// 设置多个cookie
设置多个cookie需要多次使用这样的方法。正确的设置方法是：

[javascript]
document.cookie=&quot;key=escape(value)&quot;;
document.cookie=&quot;key1=escape(value1)&quot;
// 而不是
document.cookie=&quot;key=escape(value);key1=escape(value1)&quot;;
[/javascript]
// 获取cookie，注意第二个开始key值前面有空格：

[javascript]
function getCookie(key){
     var aCookie = document.cookie.split(&quot;;&quot;);
     for (var i=0; i &lt; aCookie.length; i++){
         var aCrumb = aCookie[i].split(&quot;=&quot;);
         if (key === aCrumb[0].replace(/^\s*|\s*$/,&quot;&quot;)){
            return unescape(aCrumb[1]);
         }
     }
}
[/javascript]
// 设置cookie的存活时间：

[javascript]
var liveDate = new Date();
liveDate.setTime(liveDate.getTime() + 3*24*60*60*1000); //设置cookie的name的存活时间为3天。
document.cookie=&quot;name=test;expires=&quot; + liveDate.toGMTString();
[/javascript]
// 删除cookie,设置expires一个过期的时间即可

[javascript]
var liveDate = new Date();
liveDate.setTime(liveDate.getTime() - 10000);
document.cookie = &quot;name=test;expires=&quot; + date.toGMTString();
[/javascript]
// 拓展cookie的作用域到根目录：

[javascript]
document.cookie=&quot;key=escape(value);path=/&quot;;
[/javascript]
// 设置cookie的访问域

[javascript]
document.cookie=&quot;name=value;domain=cookieDomain&quot;;
//以Laispace为例，要实现跨主机访问，可以写为：
document.cookie=&quot;name=value;domain=.laispace.com&quot;; //所有Laispace.com下的主机都可以访问该cookie
[/javascript]
// 设置cookie的访问权限
设置了该属性，只有使用https协议才能够访问到
注意点：
如果设置cookie时带path属性，那么在删除的时候一定要加上path属性，否则删除的是当前目录下设置的cookie值。