title: Javascript碎片积累
tags:
  - 碎片
id: 406
categories:
  - Javascript
date: 2013-06-01 16:57:48
---

-
JS中的Image对象：

[javascript]
function getImageSize(imageEl) {
 var i = new Image();
 i.src = imageEl.src;
 return new Array(i.width, i.height);
}
[/javascript]

image对象现在一般常用来预加载一些图片，先将其装入 DOM，等到需要的时候，直接调用，省掉等待的时间，直接显示出来。
需要注意的是：src 属性一定要写到 onload 的后面，否则程序在 IE 中会出错。
-
for 和 class 是 JavaScript 中的关键字，所以在 JavaScript 中这两个属性名称分别用 htmlFor 和 className 代替。

[javascript]
function getAttr(el, attrName){
 var attr = {'for':'htmlFor', 'class':'className'}[attrName] || attrName;
}
[/javascript]

-
交换两个变量值的巧妙方法，利用数组：
foo = [bar, bar=foo][0]; // 多巧妙啊！编程的时候不要忘记思考，否则你就只是一个完成工作的机器。
-
双感叹号的用法：!! 一般用来将后面的表达式强制转换为布尔类型的数据（boolean），也就是只能是true或者false。
-
注意一些类型转换：

[javascript]
a=parseInt(“1234″)
a=”1234″-0 //转换为数字
b=1234+”&quot; //转换为字符串
c=someObject.toString() //将对象转换为字符串
[/javascript]

其中第1种、第4种为显式转换，2、3为隐式转换
布尔型的转换，javascript约定规则为
false、undefinded、null、0、”" 为 false
true、1、”somestring”、[Object] 为 true
-
在标准的事件绑定中绑定事件的方法函数为 addEventListener,而IE使用的是attachEvent
-
jQuery.trim()去除字符串前后的空格
-
jQuery.inArray()判断元素是否在数组中

[javascript]
var arr = [&quot;html&quot;, &quot;css&quot;, &quot;js&quot;, &quot;jquery&quot;];
$.inArray('js',arr);
[/javascript]

-
编写一个简单的 jQuery 插件（模板）

[javascript]
//You need an anonymous function to wrap around your function to avoid conflict
(function($){
 //Attach this new method to jQuery
 $.fn.extend({
 //This is where you write your plugin's name
 pluginname: function() {
 //options
 var defaults = {
 option1: &quot;default_value&quot;
 }
 var options = $.extend(defaults, options);
 //a public method
 this.methodName: function () {
 //call this method via $.pluginname().methodName();
 }
 //Iterate over the current set of matched elements
 return this.each(function() {
 var o = options;
 //code to be inserted here
 });
 }
 });
//pass jQuery to the function,
//So that we will able to use any valid Javascript variable name
//to replace &quot;$&quot; SIGN. But, we'll stick to $ (I like dollar sign: ) )
})(jQuery);
[/javascript]

-
小技巧：在浏览器地址栏中输入一行代码：data:text/html, &lt;html contenteditable&gt; ，回车即可把浏览器变临时编辑器（需要浏览器支持 HTML5 属性 contenteditable） 原文
-
所有JS对象共享的方法是toString()，返回该对象序列化格式后的字符串
-
元素的client属性(只读)指元素的内容部分再加上padding的大小，不包括border和滚动条占用的空间，故document元素的clientHeight和clientWidth属性，就代表了网页的相对大小。

[javascript]
 // 获得网页的相对大小
 function getViewport(){ // 页面加载完成后才能运行
 　　　　if (document.compatMode == &quot;BackCompat&quot;){
 　　　　　　return {
 // IE6 quirks模式
 　　　　　　　　width: document.body.clientWidth,
 　　　　　　　　height: document.body.clientHeight
 　　　　　　}
 　　　　} else {
 　　　　　　return {
 　　　　　　　　width: document.documentElement.clientWidth,
 　　　　　　　　height: document.documentElement.clientHeight
 　　　　　　}
 　　　　}
 　　}
[/javascript]

&nbsp;

[javascript]
 // 获取网页的绝对大小
 // 如果网页内容能够在浏览器窗口中全部显示,不同浏览器有不同的处理，这两个值未必相等,要取它们之中较大的那个值
 function getPagearea(){
 if (document.compatMode == &quot;BackCompat&quot;){
 return {
 width: Math.max(document.body.scrollWidth,
 document.body.clientWidth),
 height: Math.max(document.body.scrollHeight,
 document.body.clientHeight)
 }
 } else {
 return {
 width: Math.max(document.documentElement.scrollWidth,
 document.documentElement.clientWidth),

height: Math.max(document.documentElement.scrollHeight,
 document.documentElement.clientHeight)
 }
 }
 }
 [/javascript]

-
每个元素都有offsetTop和offsetLeft属性，表示该元素的左上角与父容器（offsetParent对象）左上角的距离。所以，只需要将这两个值进行累加，就可以得到该元素的绝对坐标。

// 获取绝对位置的横坐标和纵坐标
// 由于在表格和iframe中，offsetParent对象未必等于父容器，所以以下函数对于表格和iframe中的元素不适用。

[javascript]
function getElementLeft(element){
 　　　　var actualLeft = element.offsetLeft;
 　　　　var current = element.offsetParent;
 　　　　while (current !== null){
 　　　　　　actualLeft += current.offsetLeft;
 　　　　　　current = current.offsetParent;
 　　　　}
 　　　　return actualLeft;
 　　}
 　　function getElementTop(element){
 　　　　var actualTop = element.offsetTop;
 　　　　var current = element.offsetParent;
 　　　　while (current !== null){
 　　　　　　actualTop += current.offsetTop;
 　　　　　　current = current.offsetParent;
 　　　　}
 　　　　return actualTop;
 　　}
[/javascript]

// 获取网页元素的相对位置:绝对坐标减去滚动条滚动的距离

[javascript]
function getElementViewLeft(element){
 　　　　var actualLeft = element.offsetLeft;
 　　　　var current = element.offsetParent;
 　　　　while (current !== null){
 　　　　　　actualLeft += current.offsetLeft;
 　　　　　　current = current.offsetParent;
 　　　　}
 　　　　if (document.compatMode == &quot;BackCompat&quot;){
 　　　　　　var elementScrollLeft=document.body.scrollLeft;
 　　　　} else {
 　　　　　　var elementScrollLeft=document.documentElement.scrollLeft;
 　　　　}
 　　　　return actualLeft-elementScrollLeft;
 　　}
 　　function getElementViewTop(element){
 　　　　var actualTop = element.offsetTop;
 　　　　var current = element.offsetParent;
 　　　　while (current !== null){
 　　　　　　actualTop += current. offsetTop;
 　　　　　　current = current.offsetParent;
 　　　　}
 　　　　 if (document.compatMode == &quot;BackCompat&quot;){
 　　　　　　var elementScrollTop=document.body.scrollTop;
 　　　　} else {
 　　　　　　var elementScrollTop=document.documentElement.scrollTop;
 　　　　}
 　　　　return actualTop-elementScrollTop;
 　　}
// scrollTop和scrollLeft属性是可以赋值的
[/javascript]

-
-
object.prop和object['prop']是等价的，当属性是带空格的string时就只能用方括号了：person['first name'];
-
for…in 循环输出的属性名顺序不可预测,使用之前先检测对象是否为null 或者 undefined
-
hasOwnProperty是js中唯一一个处理属性但是不查找原型链的函数

[javascript]
Object.prototype.prop = 'propsss';
var obj = {und:undefined};
obj.prop; // propsss
'und' in obj; // true
obj.hasOwnProperty('prop'); // false
obj.hasOwnProperty('und'); // true
//只有hasOwnProperty可以给出正确和期望的结果，尤其在遍历一个对象时
//除了hasOwnProperty外，没有其他方法可以排除原型链上的属性（不是定义在对象自身上的属性）
//如果hasOwnProperty被占用呢？来看：
var obj = {
    hasOwnProperty: function(){
        return false;
    },
    prop: 'this is bad...'
};
obj.hasOwnProperty('prop'); // 总是返回false
//这样解决：
{}.hasOwnProperty.call(obj,'prop'); // 返回true
[/javascript]

-
Object的每个实例都具有下列属性方法：
1.Constructor：保存着用于创建当前对象的函数 上面例子 构造函数就是 Object()
2.hasOwnProperty(prop):检查给定的属性是否在当前对象实例中（而不是在实例的原型中）。作为参数的属性必须以string形式指定
3.isPrototypeOf(object):用于检查传入的对象是否是另一个对象的原型。
4.propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用for in语句
5.toLocaleString():返回对象的字符串表示，与环境的地区对应
6.toString():同上
7.valueOf(): 返回对象的字符串、number、Boolean表示。通常与toString()相同
-
javascript中的乘法问题：
可用 10000 作为基数确保精度，如 31.12 * 10000 * 9.7 / 10000
-
function语句在解析时会被提升，对代码求值时js引擎在第一遍会声明函数并将它们放到源代码树的顶部。

[javascript]
     alert(sum(10,10))
      function sum(n1,n2){
          return n1+n2;
     }
     //单独使用下面代码时，函数表达式会出错：
      alert(sum(10,10));
      var sum = function (n1,n2){
          return n1+n2;
     }
[/javascript]

命名函数表达式即被认为是函数声明也被认为是函数表达式

[javascript]
typeof g; // &quot;function&quot;
var f = function g(){};
//上面这个例子论证了 jScript 是如何把一个命名函数表达式处理成一个函数声明的
//在函数声明发生之前就把 g 给解析了   【在IE中检测】
[/javascript]

-
this是在函数调用时才被确定的而不是定义的时候

[javascript]
     var Dog = {
          toString: function() { return 'dog';},
          fn: function() { alert(this);},
     };
      var Cat = {
          toString: function() { return 'cat';}
     };
      Dog.fn(); // dog
      Dog['fn']() // dog
      Cat.fn = Dog.fn;
      Cat.fn(); // cat
      var func = Dog.fn;
      func(); // window
[/javascript]

-

apply和call的区别，在于apply用数组传递参数，而call用多参数逗号传递参数：

[javascript]

function myFunc(arg1,arg2,arg3){

// code ...

}

myFunc.apply(null,[arg1,arg2,aeg3]);

myFunc.call(null,arg1,arg2,arg3);

// 第一个参数指定调用函数myFunc的对象

[/javascript]

-

构造函数的理解：当new修饰符修饰函数时，this就会指向新对象，该对象会自动返回，称之为构造函数：

[javascript]
function Person(name){
    this.name = name;
    this.sayName = function(){
        alert(&quot;我的大名是：&quot; + this.name);
}
}

var person1 = new Person(&quot;xiaolai&quot;); // 构造函数返回一个对象，赋值给变量 xiaolai
person1.sayName(); // 『我的大名是：xiaolai』
[/javascript]

-
March 6, 2014—添加—

- 获取元素的classList:

[javascript]
 selector.classList 获取所有的class
 selector.clasList.add(value); // 添加类
 selector.clasList.remove(value); // 删除类
 selector.clasList.toggle(value); // 切换类
 selector.clasList.contains(value); // 判断是否包含类
 [/javascript]

- 获取DOM中获得了焦点的元素

[javascript]
 document.activeElement
 [/javascript]

- 判断DOM是否加载完成

[javascript]
 document.readyState // ‘complete’ or ‘loading’
 [/javascript]

- 判断浏览器采用了哪种渲染模式

[javascript]
 document.compatMode // ‘CSS1Compat’ 为标准模式，’BackCompat’ 为混杂模式
 [/javascript]

- 判断浏览器采用的字符集

[javascript]
 document.defaultCharset   // 默认字符集
 document.charset // 采用的字符集
 [/javascript]

- 访问所有自定义属性

[javascript]
 selector.dataset.myid; // 获取div[data-myid=‘100’]中的100
 [/javascript]

- 将某个元素调入视窗内

[javascript]
 selector.scrollIntoView()
 [/javascript]

- 返回元素的子元素节点（不包括其他节点）

[javascript]
 selector.children // 是HTMLCollection的一个实例
 [/javascript]

- 获取视窗大小
[javascript]
function getViewport() {
 // 判断是否混杂模式
 if (document.compatMode == 'BackCompat') {
 return {
 width: document.body.clientWidth,
 height: document.body.clientHeight
 }
 } else {
 return {
 width: document.documentElement.clientWidth,
 height: document.documentElement.clientHeight
 }
 }
}
[/javascript]

- 获取元素在视窗内的位置
[javascript]
selector.getBoundingClientRegt() // 返回对象含属性 left right top bottom
[/javascript]

- 获取页面的样式表
[javascript]
document.styleSheets;
document.styleSheets[0].disabled = true; // 禁用第一个样式表
[/javascript]