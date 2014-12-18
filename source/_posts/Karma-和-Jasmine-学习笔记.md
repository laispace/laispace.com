title: 'Karma 和 Jasmine 学习笔记'

tags:

	- jasmine

	- karma

date: 2014-07-13 10:28:04
---

# Jasmine 

Jasmine 是一个用于编写 js 测试的框架。	
	
下载

	$ git clone https://github.com/pivotal/jasmine.git
	$ mkdir jasmine && cd jasmine
	$ mv jasmine/dist/jasmine-standalone-2.0.0.zip jasmine/jasmine
	$ cd jasmine/jasmine
	// 解压
	$ unzip jasmine-standalone-2.0.0.zip

	// 除了使用 git 也可以使用 bower 来安装 $ bower install jasmine

创建测试文件

	// test.html
	<!-- 引入jasmine依赖文件 -->
	<link rel="stylesheet" type="text/css" href="jasmine/lib/jasmine-2.0.0/jasmine.css">
	<script type="text/javascript" src="jasmine/lib/jasmine-2.0.0/jasmine.js"></script>
 	<script type="text/javascript" src="jasmine/lib/jasmine-2.0.0/jasmine-html.js"></script>
	<script type="text/javascript" src="jasmine/lib/jasmine-2.0.0/boot.js"></script>
  
	<!-- 编写需要测试的代码 -->
	<script>
	  function sayHello (name) {
    	return 'Hello ' + name;
	  }
	</script>

	<!-- 编写测试脚本 -->
	<script>
	  describe('A suite of basic functions', function () {
	    it('sayHello', function () {
    	  var name = 'Xiaolai';
      	var exp = 'Hello Xiaolai';
	      expect(exp).toBe(sayHello(name));
    	})
	  })
	</script>		

浏览器打开 test.html 即可看到测试效果

更多的 jasmine 语法，查看[官方文档](http://jasmine.github.io/2.0/introduction.html)

<!-- more -->

# Karma

Karma 是一个基于 NodeJS 的js测试执行过程管理工具。

安装

	$ npm install -g karma-cli		

初始化

	$ mkdir karma && cd karma
	$ karma init
	// 根据提示完成初始化

安装 jasmine 插件

	$ npm install karma-jasmine -g		
	
创建源文件
	
	// source.js
	function reverse(name){
    	return name.split("").reverse().join("");
	} 	
	
创建测试文件，使用 jasmine 语法编写
	
	// test.js
	describe("A suite of basic functions", function() {
   		it("reverse word",function(){
   	 		expect("DCBA").toEqual(reverse("ABCD"));
	        expect("Xiaolai").toEqual(reverse("ialoaiX"));
	    });
	});

修改配置文件
	
	// karma.conf.js 修改以下部分
	files: ['*.js']
	exclude: ['karma.conf.js']
	
启动测试

	$ karma start karma.conf.js	
	
### 参考链接

- [karma-runner.github.io](http://karma-runner.github.io/)
- [jasmine.github.io](http://jasmine.github.io/)
- [jasmine.github.io/2.0/](http://jasmine.github.io/2.0/introduction.html)
- [Karma和Jasmine自动化单元测试](http://blog.fens.me/nodejs-karma-jasmine/)