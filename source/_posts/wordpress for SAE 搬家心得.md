title: wordpress for SAE 搬家心得
id: 86
categories:
  - Tools
date: 2013-06-05 10:20:17
tags:
  - wordpress
  - SAE
	
---

原来[laispace.com](http://www.laispace.com) 用的便宜香港空间，速度实在太慢忍受不了了！小赖尝试将wordpress从自己主机上搬迁到SAE上，提高访问速度，总结步骤如下：

1.备份文章内容

进入laispace【后台】-【工具】-【导出】，选择【所有内容】-【下载导出的文件】生成一个laispace.xml文件到本地

2.备份数据库内容

进入laispace phpMyAdmin 【数据库名】-【导出】，生成一个laispace.sql文件到本地

<!-- more -->

3.SAE上安装wordpress

SAE上安装worpress有很多权限问题，所以不能直接安装wordpress.org下载的安装包，需要在这里下载[WordPress for SAE](http://sae.sina.com.cn/?m=apps&amp;a=detail&amp;aid=1 "点击进入SAE下载wordpress")安装

4.SAE上导入文章等数据

安装[WordPress for SAE](http://sae.sina.com.cn/?m=apps&amp;a=detail&amp;aid=1 "点击进入SAE下载wordpress")成功后，进入sae wordpress【后台】-【工具】-【导入】，选择【Wordpress】将步骤2中的laipace.xml上传并导入，这一步可导入wordpress的文章内容等信息

5.SAE上导入wordpress设置

wordpress的很多设置是保存在数据库中的，单纯按步骤1备份只能备份文章的内容，要包括后台设置（如导航栏设置，用户信息）整站搬迁，需要进入SAE账户后台[http://sae.sina.com.cn/](http://sae.sina.com.cn/) 【服务管理】-【MySQL】-【SQL管理】-【管理SQL】，将步骤2中的laispace.sql上传并导入，这一步可导入评论、链接、标签、用户信息等详细设置

6.修改博客路径SiteUrl

紧接着步骤5，在数据库中找到数据表wp_options，将siteurl从http://www.laispace.com 修改为 http://xiaolai.sinaapp.com

这一步可让博客里的相对链接跳转正确

7.使用SVN上传插件、主题、附件

前面说到，Wordpress for SAE限制了很多权限，无法直接在上面安装主题、插件等，需要自己备份laispace中的plugins、themes、uploads上传到SAE中，方法是使用SVN。先用FTP工具从laispace主机中将wp-content文件夹中的plugins、themes、uploads打包下载到本地，再使用SVN将这几个文件夹覆盖到Wordpress for SAE中，然后进入wordpress后台，启用主题、配置插件。注意，这里的uploads文件夹是使用laispace时上传的附件，将其上传到Wordpress for SAE后，才能在文章中正确显示那些附件（如图片）。PS：如此上传附件可能麻烦，可考虑SAE的Storge服务或者其他的云存储服务。

经此折腾，我就把laispace.com整站搬迁到xiaolai.sinaapp.com啦！SAE好处多多，最大的亮点当然访问速度是比我使用收费的香港空间快多了（该死的校园网，还不能访问这个香港空间）！

8.添加独立域名

在SAE应用后台中进行【独立域名设置】，把www.laispace.com绑定到SAE的服务器中，这么一来输入www.laispace.com就可以跳转到托管到SAE上的wordpress但链接仍显示www.laispace.com了，但设置这个的时候发现，在我的域名设置中（万网域名），能将www.laispace.com或abc.laispace.com等域名CNAME到SAE服务器，但万网不提供给我的主域名laispace.com CNAME解析，即浏览器栏直接输入laispace.com将不能访问！这可是个致命的问题。

9.修改DNS解析

google了好久，发现dnspod的域名解析服务，于是在[dnspod官网](www.dnspod.cn)注册后，获得dnspod的域名DNS，然后到万网【域名管理】-【域名DNS修改】，将万网的DNS修改为dnspod的DNS，直接在dnspod中管理域名解析。将主域名laispace.com CNAME到SAE服务器上，这么一来，输入地址laispace.com就可以直接访问了！

说明，写此文时小赖安装的Wordpress for SAE版本是 SAE 3.4.1 测试版

附一些参考资料：

SAE官网 [http://sae.sina.com.cn](http://sae.sina.com.cn/)

Wordpress for SAE 官方 [http://wp4sae.org/](http://wp4sae.org/)

SAE部署SVN指南 [http://sae.sina.com.cn/?m=devcenter&amp;catId=212](http://sae.sina.com.cn/?m=devcenter&amp;catId=212)