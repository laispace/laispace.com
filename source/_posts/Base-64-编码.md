title: Base 64 编码
categories:
  - Javascript
tags:
  - base64
date: 2014-07-28 04:23:26
---

在浏览器中，使用 window.btoa() 将字符串或二进制值转化为 Base64 编码，使用 window.atob() 还原。

```
window.btoa('laispace'); //=> "bGFpc3BhY2U="
window.atob("bGFpc3BhY2U="); //=>"laispace"
```

注意，要将非 ASCII 编码字符转化为 Base64 编码的话，需要先进行转码，否则会报错

```
window.btoa(encodeURI('赖小赖')); //=> "JUU4JUI1JTk2JUU1JUIwJThGJUU4JUI1JTk2"
window.atob('JUU4JUI1JTk2JUU1JUIwJThGJUU4JUI1JTk2'); //=> "%E8%B5%96%E5%B0%8F%E8%B5%96"
decodeURI("%E8%B5%96%E5%B0%8F%E8%B5%96"); //=> "赖小赖"
```