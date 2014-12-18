title: PhoneGap入门-用Ionic+Angular创建Hybrid应用
tags:
  - Phonegap
id: 593
categories:
  - Hybrid
date: 2014-03-01 01:04:09

---

使用HTML5制作移动应用，寒假前确定了方案《[HTML5移动开发方案探索](http://www.laispace.com/?p=551)》，是时候开始实施了！

这是第一次做HTML5移动应用的开发，Google了不少发现坑不少，中文文档也不多，只得自己好好摸索多查官方文档了。

处女PhoneGap应用的笔记先贴在这里，项目完工再好好做个总结吧：）

<!-- more -->


选择方案：

PhoneGap + Ionic + AngularJS

开发环境：

操作系统：mac OS10.9

$ node -v

v0.10.26

$ npm -v

1.4.3

$ phonegap -v    # 安装后才查看

3.3.0-0.19.6

$ cordova -v

3.4.0-0.1.0

第一步：安装 command_line_tools

第二步：安装 phonegap和cordova
# 第一次安装失败，将nodejs更新到最新版后才成功
$ sudo npm install -g phonegap
$ sudo npm install -g cordova

第三步：使用cordova创建项目
$ cordova create hello com.example.hello "HelloWorld"
$ cd hello
# 添加ios开发平台
$ cordova platform add iOS
# 初始化
$ cordova prepare              # 或 "cordova build”

第四步：测试模拟器

Xcode打开文件 hello/platforms/ios/hello.xcodeproj

文件夹  hello/www/即是存放html/css/js的地方。

修改文件 hello/www/index.html，添加 &lt;h1&gt;Hello 赖小赖&lt;/h1&gt;

点击Xcode左上角的run，即打开了模拟器，就能看到 『Hello 赖小赖』啦！

第五步：编写www目录下的代码

【未完待续...】

------- 我是风骚的分隔符 -------

都说了phoneGap开发的坑太多，所以我得谨慎查多点资料后确立一个终极方案。

找了好久，发现Ionic这个很酷的框架（项目做出来再对它做个介绍）。

Ionic的UI配合AngularJS的MVC功能，应该能省不少事，所以我就想做个尝试。

开始吧！

第一步：保证已安装cordova

$ npm install -g cordova

第二步：安装ionic

$ sudo npm install -g ionic

第三步：使用ionic创建种子项目

$ ionic start ionicApp

第四步：保证已安装 ios-sim 和 ios-deploy

$ sudo npm install -g ios-sim

$ sudo npm install -g ios-deploy

第五步：添加ios开发平台

$ cd ionicApp

$ ionic platform ios

第六步：启动模拟器

$ ionic emulate ios

$ ionic run ios

酷！一个IOS7.0.3版本的种子界面就生成了：

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/iOS-Simulator-Screen-shot-Mar-1-2014-11.29.27-AM.png "iOS Simulator Screen shot Mar 1, 2014, 11.29.27 AM")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/iOS-Simulator-Screen-shot-Mar-1-2014-11.29.27-AM.png)

第七步：使用模拟器调试代码

每次更新完代码，想看到实时代码变化的话，可以重启模拟器：

$  ionic emulate ios

但是这样非常繁琐耗时，一点都不酷！

调试：使用浏览器调试代码

打开python自带的服务器（提前装好python）：

    $ python -m SimpleHTTPServer 8000`
    `然后访问 http://localhost:8000 就能直接用浏览器来调试啦。`
    或者：使用chrome或safari的mobile调试功能：
    http://moduscreate.com/enable-remote-web-inspector-in-ios-6/
    https://developers.google.com/chrome-developer-tools/docs/remote-debugging
    或者：使用weinre进行远程调试：
    https://developer.mozilla.org/en-US/Firefox_OS/Platform/Gaia/Weinre_As_Remote_Debugger</pre>
    【未完待续...】

    第X步：真机测试之android

    # mac下安装android SDK

    http://developer.android.com/sdk/index.html，解压缩adt-bundle-mac-x86_64-20131030.zip到/Applications文件夹下

    装好后，USB连接android，执行命令：
    <pre>`$ cordova run android`
    `即可在android真机上测试

第X步：发布应用之android

发布前先关闭开发模式：
<pre>`# 关闭ios开发模式`
`$ cordova plugin rm org.apache.cordova.console `
`# 关闭android开发模式`
`修改 ``platforms/android目录下的`AndroidManifest.xml文件，
将其中的android:debuggable="true"改为"false"
`# 生成应用`
`$ cordova build --release android`
`提示ant未安装，则使用HomeBrew安装ant:`
`$ brew install ant`
`提示android SDK未加入环境，则加入:`
</pre>
# 增加SDK到环境
export PATH=${PATH}:/Applications/adt-bundle-mac-x86_64-20131030/sdk/platform-tools:/Applications/adt-bundle-mac-x86_64-20131030/sdk/tools

重新生成：

$ cordova build --release android

成功后会在 platform/android/ant-build/ 文件夹下找到StarterApp-release-unsigned.apk

# 生成密钥
<pre>`$ keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000`
<span style="font-family: monospace;">成功后在当前目录下能找到文件 my-release-key.keystore</span>
<span style="font-family: monospace;"># 使用密钥对应用进行签名</span>

`$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore StarterApp-release-unsigned.apk alias_name`
`# 优化应用`

`$ zipalign -v 4 ``StarterApp-release-unsigned.apk `StarterApp.apk
`成功后在当前目录下找到文件 `StarterApp.apk 就可以直接安装到手机上了。
附上小赖的第一个HTML5 Hybrid应用android版截图：
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/1-614x1024.jpg "1")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/1.jpg)
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/2-614x1024.jpg "2")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/2.jpg)
[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/3-614x1024.jpg "3")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/03/3.jpg)
安装到手机上体验一番后感觉流畅，但这还只是静态页面。
袋继续完善它后，再做评估吧。

# IOS的真机测试【待补充】

# 发布到App store 【待补充】</pre>
<div></div>
参考链接：

(Angular.js + jQuery Mobile + PhoneGap)[http://www.adobe.com/cn/devnet/html5/articles/angular-jquery-phonegap.html]

(Ionic 官网)[http://ionicframework.com/]