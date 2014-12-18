title: 'Javascript 闭包的学习'
tags:
  - 闭包
id: 511
categories:
  - Javascript
date: 2013-10-05 23:36:13
---

名词定义：包裹一些局部变量的一个函数叫做一个闭包。闭包是个函数，而它「记住了周围发生了什么」。表现为由「一个函数」体中定义了「另个函数」
实现原理：嵌套函数可以访问外部作用域中声明的变量。
组成结构：函数以及构建这个函数的环境。
使用价值：将函数与其所操作的某些数据（环境）关连起来
使用缺点：闭包会影响性能！闭包会使得函数中的变量都被保存在内存中，内存消耗很大。
- 用途1，将数据与多个函数相关联：
[javascript]
 function makeSizer(size) {
   return function() {
     document.body.style.fontSize = size + 'px';
   };
 }

 var size12 = makeSizer(12);
 var size14 = makeSizer(14);
 var size16 = makeSizer(16);

 //size12();  // 将字号调整到12px
 //size14();  // 将字号调整到14px
 //size16();  // 将字号调整到16px
 [/javascript]

- 用途2，模拟私有方法：
私有方法不仅仅有利于限制对代码的访问：还提供了管理全局命名空间的强大能力，避免非核心的方法弄乱了代码的公共接口部分。
[javascript]
 // 创建一个环境为三个函数共享，减少了污染
 var Counter = (function() {
   var privateCounter = 0;
   function changeBy(val) {
     privateCounter += val;
   }
   return {
     increment: function() {
       changeBy(1);
     },
     decrement: function() {
       changeBy(-1);
     },
     value: function() {
       return privateCounter;
     }
   }
 })();

 alert(Counter.value()); /* 提示 0 */
 Counter.increment();
 Counter.increment();
 alert(Counter.value()); /* 提示 2 */
 Counter.decrement();
 alert(Counter.value()); /* 提示 1 */
 [/javascript]

这种方法跟创建一个对象，并分别定义对象的三个方法相似吧？
实验结果显示，这两种方式是相同的，不过小赖觉得，以下这种更为直观一点吧：
[javascript]
 // 创建一个环境为三个函数共享，减少了污染
 var Counter = {
   privateCounter : 0,

   increment: function() {
       this.privateCounter +=1;
     },

     decrement: function() {
       this.privateCounter -=1;
     },

     value: function() {
       return this.privateCounter;
     }
 }

 alert(Counter.value()); /* 提示 0 */
 Counter.increment();
 Counter.increment();
 alert(Counter.value()); /* 提示 2 */
 Counter.decrement();
 alert(Counter.value()); /* 提示 1 */
 [/javascript]

使用闭包的话，三个方法共享一个环境，而使用对象来创建明明空间，实则是通过this指向这个对象，来保证共享一个环境，也是避免了污染，有异曲同工之妙吧。

参考：
1\. [闭包](<span style="text-decoration: underline;">https://developer.mozilla.org/zh-CN/docs/JavaScript/Guide/Closures</span> )
2\. [闭包](<span style="text-decoration: underline;">http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html</span> )
3\. [secrets_of_javascript_closures](<span style="text-decoration: underline;">https://app.box.com/shared/elkumrpfng</span> )