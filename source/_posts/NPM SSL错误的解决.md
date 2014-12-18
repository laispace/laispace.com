title: 'NPM SSL错误的解决'
id: 685
categories:
  - Node
date: 2014-04-12 11:03:11
tags:
 - NPM   
---

执行 $ npm update 更新模块的时候，报错：
<pre>Error: UNABLE_TO_VERIFY_LEAF_SIGNATURE

查资料发现是防火墙的问题，要解决的话，关闭ssl的严格模式，执行：
$ npm config set strict-ssl false 

然后重新执行 $npm update 即可解决

但别忘了这样会降低npm的安全性，所以可以设置回来：
$ npm config set strict-ssl true</pre>