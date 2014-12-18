title: 'Javascript 最佳实践'
tags:
  - 最佳实践
id: 501
categories:
  - Javascript
date: 2013-10-04 19:28:31
---

// @author lxl:使用高质量JS代码对提高性能肯定是非常有帮助的，小赖对常见的好方法总结在这里，不断更新。
// @update 2013/10/04
-
使用压缩后的文件（如lxl.min.js），并开启http gzip压缩工具
-
尽量将script标签放在前，可以尝试用异步加载的方法加载js文件
-
尽量保证js代码和HTML结构的分离，不要内嵌代码到HTML中，以统一维护和缓存处理

-

- 避免全局变量的污染

1\. 使用命名空间
2\. 匿名函数封装立即执行
3\. 始终使用var来声明变量
- 使用var声明（显式）的*全局*变量是**不能**被删除的
- 未用var声明（隐式）的*全局*变量是**可以**被删除的
- 隐式全局变量并非真正的全局变量，但却是全局对象的属性，因为属性是可以通过delete操作符删除的，而变量则不行

- for循环中将长度缓存到变量中。
```
避免重复计算HTMLCollections的长度（操作DOM一般都是比较昂贵的,缓存长度后效率竟然是是IE7下不缓存的190倍！）
```
- for循环中建议使用降序来遍历
```
向下数到0通常更快，因为和0作比较要比和数组长度或非0的东西作比较更有效率
```
- for-in循环只在遍历对象属性的时候才使用，其他情况建议使用for循环就够了
```
for-in 循环枚举出的顺序是不能保证的，且若数组对象已被自定义的功能增强，就可能发生逻辑错误
```
- 避免隐式类型转换，使用===或!==总是最严谨的！
```
这总能避免一些意想不到的类型转换问题，不是么？
```

- 使用hasOwnProperty()方法过滤从原型链继承的属性，如：
```
for (var i in man) {
if (man.hasOwnProperty(i)) { // 过滤
console.log(i, ":", man[i]);
}
}
```
- 使用单var语句声明变量
```
变量的声明会被被JS引擎提至函数顶部预解析（hoisting），不如直接使用单var统 一声明所有将会用到的变量，方便查询又易于管理，如：
var a = 1,
b = 2,
c = 3;
```
- 避免改变或增加原型对象的方法
```
随意改变或增加原型会增加维护成本，当以后使用一个方法却发现这个方法被重定义时，就会带来问题。
除非团队认可这种做法并意识到原型已添加了方法，知道怎么使用时：
if( typeof Object.prototype.myMethod !== "function"){
Object.prototype.myMethod = function(){
// 实现新增方法
}
}
```
- 避免使用eval语句
```
eval是魔鬼，除非知道它执行的代码本身会有什么问题。
eval里的代码被恶意篡改的话，就会带来严重的安全问题。
若绝对需要使用eval，实则可以
1.用new Function()替代，因为它有局部函数作用域，其中的var变量不会变成全局 的，可以避免一些问题.
2.将eval语句封装到即时的匿名函数中，与1有相同效果。
注意，setInterval、setTimeout中传递字符串跟eval()是一样的，要注意避免直接 传递字符串：
setTimeout(myFunc, 1000); // 正确
setTimeout(function () { // 正确
myFunc(1, 2, 3);
}, 1000);
```
- parseInt() 数制转换，不要忽略第二个参数指定基数
```
EC3中以字母o开头的字符串被当做八进制处理，而在EC5中已经改变，为了避免意外，应 该总是指定基数参数，尽管默认是10
```
- 团队里使用同一套缩进方案，tab或space缩进
```
比起纠结于具体的规范，团队里总是执行同一套方案更有价值！
```
- 总是使用花括号{},尽管只有一行代码
```
花括号开始的位置，是同一行还是换行，这也是团队规范的问题了：统一就好，不必 纠结。
为了避免下一条谈到js引擎自动补全分好的问题，建议花括号开始于同一行，可终端JS分 号的自动补全。
```
- 总是使用分号结束代码
```
因为JS引擎自动补全分号的机制，不小心的换行可能会中断代码逻辑，如return语句块 换到了下一行。
```
- 命名规范,多种，选择一套喜欢的呗
1\. 构造函数首字母大写，如Person(){}
2\. 构造函数驼峰命名分割单词，如MyFunc(){}
3\. 变量名用下划线分割单词，如 my_name，这可以喝ECMAScript默认属性和方法的Camel标记法相区分
4\. 常量用全大写和下划线书写，如 MAX_WIDTH
5\. 全局变量名全部大写，如GLOBAL，并使用它来定义明明空间，如GLOBAL.name = "xiaolai";GLOBAL.myMethod = function(){};
6\. 私有属性或方法用下划线前缀来表示，如 _index
-

## 编码技巧
- 访问全局对象
```
全局对象一般直接通过window属性来访问，但特殊情况下（如定义了名为window的局部 变量覆盖了全局的window）可使用匿名函数内的this来获得全局对象：
var global = (function(){
return this;
})();
```
## 新鲜概念
- HTMLCollections对象
```HTMLCollections对象指的是DOM方法返回的对象，是一个集合，如：
document.getElementsByName();
document.getElementsByClassName();
document.getElementsByTagName();
document.images; // 页面上所有的图片元素
document.links; // 所有a标签
document.forms; // 所有表单
document.forms[0].elements; // 页面上第一个表单中的所有域
```