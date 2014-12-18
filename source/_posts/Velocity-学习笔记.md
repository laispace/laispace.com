title: Velocity 学习笔记
categories:
  - Node
tags:
  - vm
date: 2014-07-17 22:35:17
---

单行注释
		
		## This is a comment.

多行注释

		#*
			This is a mutil-line comment
			This is a mutil-line comment
			This is a mutil-line comment		
		*#		

变量

		<div>
			#set ( $name = 'xiaolai')
			Hello $name !
		</div>
		
属性

		$person.Name

方法

		$person.getName()
		$person.setName('xiaolai')
		
		## 注意 $person.getName() 等同于 $person.Name

<!-- more -->

单引号双引号
	
	放在双引号内的变量才会被解析

		#set ( $foo = 'xiaolai')
		#set ( $bar1 = '$foo' )
		$set ( $bar2 = "$foo")
		$bar1 ##=> '$foo'		
		$bar2 ##=> 'xiaolai'
		

条件语句

		#set ( $foo = 100 )
		#if ( $foo < 10 )
			<div> 123 </div>
		#elseif ( $foo < 50 )
			<div> 456 </div>
		#else
			<div> 789 </div>
		#end			

关系、逻辑运算符

		## 全等，使用 == 
		$foo == $bar		
	
		## AND、OR、NOT，使用 &&、||、!
		$foo && $bar
		$foo || $bar
		!$foo

循环

		<ul>
			#foreach ( $product in $allProducts)
				<li> $product <li>
			#end	
		</ul>	

include 引入文件
	
		## 文件必须包含在TEMPLATE_ROOT目录下	
		#include ('test.txt')
		#include ('test1.txt', 'test2.txt', 'test3.txt')

parse 渲染文件
	
		## 文件必须包含在TEMPLATE_ROOT目录下
		#parse('test.vm')				

macro 宏的定义

		## 宏可以定义可重用的代码
		#macro ( tpl $title $description )
			<div>
				<h1> $title </h1>
				<p> $description </p>
			<div>	
		
		## 使用这个宏，将会进行替换
		#tpl('title', 'This is a description')	
								
		
			
		
		
		
		
		
							