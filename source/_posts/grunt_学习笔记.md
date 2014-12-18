title: grunt 学习笔记
date: 2014-05-09 21:19:42
categories: Tools
tags: 
  - grunt

---


- 配置 Gruntfile.js

	  grunt.initConfig({
	    // 读取 package.json 文件
    	pkg: grunt.file.readJSON('package.json'),
    	
		// 其他配置，如下面的 jshint/uglify
		// ...
	  });


- 设置 jshint 代码审查

	执行命令 ~# grunt jshint

		jshint: {
      		options: {
      			// 使用 jshint-stylish 高亮错误
        		reporter: require('jshint-stylish') 
      		},
	  	  // 配置任务启动时要验证的文件
   		  build: ['Grunfile.js', 'src/**/*.js']
   		}
   		
<!--more-->

- 设置 uglify 压缩js代码
	
	执行命令 ~# grunt uglify
	
		uglify: {
      		options: {
      		// banner 会显示在压缩后的代码文件中
        	banner: '/*\n <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> \n*/\n'
      		},
      		// 创建压缩文件路径
      		build: {
        		files: {
          			'dist/js/magic.min.js': 'src/js/magic.js'
        		}
      		}
    	}
    	
- 设置 cssmin 压缩css代码
	
	// 执行命令 ~# grunt cssmin
	
		cssmin: {
      		options: {
        		banner: '/*\n <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> \n*/\n'
      		},
      		build: {
        		files: {
          		'dist/css/style.min.css': 'src/css/style.css'
      		  	}
      		}
    	} 
   	 	
- 设置 less 代码编译

	执行命令 ~# grunt less
	
		less: {
      		build: {
        		files: {
          			'dist/css/pretty.css': 'src/css/pretty.less'
        		}
      		}
    	} 
    	
- 一次执行多个任务

	执行命令 ~# grunt

		// Gruntfile.js
		grunt.initConfig({
			...
		});
		// 设置default任务
		grunt.registerTask('default', ['jshint', 'uglify', 'cssmin', 'less']);

- 配置文件监视，自动执行任务

	执行命令 ~# grunt watch
	当文件改变并保存时，自动执行任务
	
		// Gruntfile.js
		grunt.initConfig({
			...
		// 配置监视
    		watch: {
				// 监视 css/less 文件，执行 less/cssmin 任务
	      		stylesheets: {
    	    		files: ['src/**/*.css', 'src/**/*.less'],
        			tasks: ['less', 'cssmin']
      	  		},
      			// 监视 js 文件， 执行 jshint/uplify 任务
	      		scripts: {
    	    		files: 'src/**/*.js',
		        	tasks: ['jshint', 'uglify']
      			  }
		    	}
		});

- 使用  time-grunt 记录每个任务执行的时间

- 使用 node-minify 压缩文件

   	


[参考资料](http://scotch.io/bar-talk/a-simple-guide-to-getting-started-with-grunt)