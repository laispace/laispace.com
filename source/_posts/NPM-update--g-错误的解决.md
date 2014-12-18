title: "NPM update -g 错误的解决"
categories:
  - Node
date: 2014-07-17 00:05:14
tags:
  - npm
	
---

使用 $ sudo npm update -g 报错，清除 npm 缓存 或者在 nodejs.org 的官网下载最新版重装后，还是报错。

解决办法是，执行：
		
		// 使当前用户拥有 /usr/local/lib/node_modules 目录的权限
		$ sudo chown -R $USER /usr/local/lib/node_modules

然后再执行更新就不会报错了：

		$ npm update -g	
		
			
让普通用户拥有 npm 的全局权限，即可以不用在 install 或 update 时加上 sudo：

		// 使当前用户拥有 /usr/local 目录的权限
		$ sudo chown -R $USER /usr/local
		// 然后就可以直接全局安装/更新一些包了
		$ npm install -g xxx
		$ npm update -g