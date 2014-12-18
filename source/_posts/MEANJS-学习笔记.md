title: 'MEANJS 学习笔记'
tags:
date: 2014-07-13 10:28:20
---

- server.js 为程序入口文件

- config/config.js 为配置入口文件

- config/env/all.js 配置将在所有环境(development、production、test)中生效

	config.db
	config.port
	config.app.title
	config.app.description
	config.app.keywords
	// 项目css文件路径，glob 模式匹配
	config.assets.css 
	config.assets.js
	// 项目的 Jasmine 测试文件路径
	config.assets.tests 
	// 依赖的第三方 css 文件路径
	config.assets.lib.css 
	config.assets.lib.js

- 指定环境启动应用
		
	// 开发环境，将使用 config/env/development.js 配置
	$ NODE_ENV=development grunt 	
	// 生产环境，将使用 config/env/production.js 配置
	$ NODE_ENV=production grunt
	// 测试环境，将使用 config/env/test.js 配置
	$ NODE_ENV=test grunt
		
- 应用启动后会自动加载的文件

	// 在这些目录下创建的 model、route、strategy 等，在需要的地方可直接引用，无须手动引入
	app/models
	app/routes
	config/strategies
	public/modules
	
<!-- more -->

[官方API](http://meanjs.org/docs.html)