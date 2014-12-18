title: '移动 Web 开发技巧'
categories: Tips
tags:
  - 移动开发
date: 2014-12-18 14:29:22
---


## Fiddler 篇
-  使用Fiddler 快速 bugfix 的办法

线上出一个小问题时, 定位到具体某一个文件比如 online.js
使用 Fiddler 将这个文件保存为 offline.js 到本地, 并设置 AutoResponder 将这个线上文件 online.js 的请求映射到 offline.js
接着就开始改动, 这样还没拉代码就先定位并解决问题了, 轻快~

- 调试 iOS 端真机环境页面

用 Chrome 模拟器并不能百分百模拟真机.

用 Fiddler 只能进行抓包调试, 对 UI 调试比较无力吧.

用 Weinre 远程调试在 PC 上成功, 但映射到 iOS 上时毫无反应, 
不知道是不是因为同时启用了 Weinre 代理服务器和 Fiddler 代理服务器.

想自用一个在外网的机子搭建 Weinre 代理服务器(这样就不需要 PC 和 iOS 都在同一个局域网下), 但是内网 ssh 连接不上…内网这么多限制, 真是麻烦.

用 Fiddler 开启代理, iOS 接入 PC 所在的网络并设置代理为 Fiddler 服务器, 用 Mac 连接 iPhone, 使用 Safari 调试. 这个目前看来, 是最爽的了.
