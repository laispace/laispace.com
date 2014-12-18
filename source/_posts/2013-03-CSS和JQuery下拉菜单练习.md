title: 2013-03-CSS和JQuery下拉菜单练习
id: 282
categories:
  - 每日一发
date: 2013-03-15 18:32:46
tags:
---

手生，光看书不动手去写代码，就不会发现细节问题，要不断地练习，熟能生巧！

CSS和JQ分别实现下拉菜单，核心代码只有几行。

[css]

/*CSS下拉菜单*/
 nav#css-menu&gt;ul&gt;li:hover ul{
     display: block;/*鼠标悬浮时显示下拉菜单*/
 }

[/css]
[javascript]

/*jQuery下拉菜单*/
 $(document).ready(function(){
     $('#jquery-menu&gt;ul&gt;li').hover(function(){
     $(this).find('ul').slideDown('slow');
     },function(){
     $(this).find('ul').slideUp('fast');
     });
 });

[/javascript]

[Demo](http://laispace.com/xiaospace/demo/2013-03/CSS和JQuery下拉菜单练习/index.html)