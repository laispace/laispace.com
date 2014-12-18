title: jQuery碎片积累
id: 408
categories:
  - Javascript
date: 2013-06-01 17:05:05
tags:
  - jquery
  - 碎片
 
---

-

td:last与td:last-child的区别

$('td:last'）选择所有td元素中的最后一个

$('td:last-child')选择tr下最后一个子元素td，有多个

-
让页面内所有的外链都是在新窗口中打开
[javascript]

$('a[href^=&quot;http://&quot;]').attr(&quot;target&quot;,&quot;_blank&quot;);
[/javascript]

-
禁用表单的提交按钮
[javascript]

$(&quot;form&quot;).submit(function(){
 $(&quot;:submit&quot;,this).attr(&quot;disabled&quot;,&quot;disabled&quot;);
});
[/javascript]

-
绝对定位与相对定位
W3CFUNS解释
属性为relative的元素可以用来布局页面，属性为absolute的元素用来定位某元素在父级中的位置
W3CFUNS实例
-
如果用定位来布局页面，父级元素的position属性必须为relative，而定位于父级内部某个位置的元素，最好用absolute，因为它不受父级元素的padding的属性影响，当然你也可以用 relative，计算的时候不要忘记计算padding的值。
-
-
$("#test").each(i,item)方法:
函数中的this关键字指向一个不同的DOM元素
返回 ‘false’ 将停止循环 (就像在普通的循环中使用 ‘break’)。返回 ‘true’ 跳至下一个循环(就像在普通的循环中使用’continue’)。
利用each给元素设置不同样式：
$(".laispace").each(function(i){this.style.color=['#f00','#0f0','#00f'][i]});
$.each(obj, fn)
通用例遍方法，可用于例遍对象和数组。
-DOM和JQ对象互转时要注意：

$(document.getElementById('test')) 相当于$("#test");
$("#test")[0]或者$("#test").get(0)
注意：eq()返回JQ对象，get()返回DOM对象，而JQ对象只能调用JQ方法，DOM对象同理
$("div").eq(2).html();//调用jquery对象的方法
$("div").get(2).innerHTML;//调用dom的方法属性-$("$bt1").click(function(){
$("#bt2").click(); //点击按钮1时也触发按钮2
})
-
扩展自定义功能:
[javascript]

$.extend({
 min: function(a, b){return a &lt; b?a:b; },
 max: function(a, b){return a &gt; b?a:b; }
});//为在jquery的命名空间中扩展了min,max两个方法
//可以像下面这样使用
var a = 10,b=20;
var max = $.max(a,b);//20
var min = $.min(a.b);//10
[/javascript]