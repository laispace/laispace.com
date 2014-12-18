title: Meta标签属性学习
id: 215
categories:
  - HTML
date: 2012-12-03 22:43:46
tags:
  - meta
---

META标签用来描述一个HTML网页文档的属性。

一般有三个属性：http-equiv,name,scheme.

1.http-equiv

1.1 content-type：字符集的设定，决定读取文件的形式和编码,用法：

&lt;meta http-equiv="Content-Type" content="text/html; charset=gb2312" /&gt;

1.2 expires：期限，设定网页到期时间（网页到期后必须服务器重新上传），用法：

用法：＜meta http-equiv="expires" content="Mon, 03 Dec 2012 18:18:18 GMT"＞

1.3 refresh：定时重刷新到指定页面，用法：

＜meta http-equiv="Refresh" content="2"；URL=http://www.laispace.com"＞

1.4 set-cookie：网页过期，那么存盘的cookie将被删除。，用法：

＜meta http-equiv="Set-Cookie" content="cookievalue=xxx; expires=Monday, 03-Jan-2012 18:18:18 GMT； path=/"＞

1.5 pragma:缓存模式，禁止浏览器使用本地缓存（无法脱机浏览），用法：

＜meta http-equiv="Pragma" content="no-cache"＞

1.6 window-target：显示窗口设定，用法（独立页面显示，防止别人在框架里调用自己的页面。）：

＜meta http-equiv="Window-target" content="_top"＞

1.7 page-enter/exit：网页进出动态效果，用法：

＜meta http-equiv="Page-Enter" content="revealTrans(duration=５.０, transition=２０)"＞

＜meta http-equiv="Page-Exit" content="revealTrans(duration=５.０, transition=２０)"＞

&nbsp;

2.name

2.1 revisit-after：

&lt;meta name="revisit-after" content="1 days" &gt;

2.2 author：作者

2.3 description：内容描述

2.4 keywords：关键词，keywords的content用逗号隔开

2.5 robots:机器人向导，声明需要索引的页面，content值可选all,none,index,noindex,follow,nofollow。默认是all。

2.5 generator：网页采用的技术版本版本

2.6 revised：修复

2.7 others：其他，用法：

&lt;meta NAME="copyright" content="Copyright 2012 -laispace.com" /&gt;

&nbsp;

3.scheme

some_text：定义与 http-equiv 或 name 属性相关的元信息，用法：

&lt;meta scheme="ISBN" name="identifier" content="0-14-043205-1" /&gt;

&nbsp;

head中其他元素的用法：

1.link:链接，用法：

&lt;link href="URL" rel="relationship"&gt;如&lt;link href="xiaolai.ico" rel="shortcut icon"&gt;

2.base：基链接，将网页内的相对路径改成绝对路径，用法：

&lt;base href="http://www.laispace.com" target="_blank"&gt;

&lt;base href="http://www.laispace.com" target="_top"&gt;

&nbsp;

&nbsp;

Meta的使用方法技巧（以下为摘抄[转载](http://blog.sina.com.cn/s/blog_6dd5ebcb01013oqw.html)内容）：

Meta标签是用来描述网页属性的一种语言，标准的Meta标签可以便于搜索引擎排序，提高搜索引擎网站权重排名。要想网站做的更符合搜索引擎标准就必须了解meta标签，下面由Seoer惜缘于大家讲讲meta标签含义与使用方法：

1、META标签的keywords

写法为：&lt;meta name="Keywords" content="信息参数" /&gt;

meat标签的Keywords的的信息参数，代表说明网站的关键词是什么。

2、META标签的Description

&lt;meta name="Description" content="信息参数" /&gt;

meta标签的Description的信息参数，代表说明网站的主要内容，概况是什么。

3、META标签的http-equiv=Content-Type content="text/html

http-equiv=Content-Type代表的是HTTP的头部协议，提示浏览器网页的信息，

&lt;meta http-equiv="Content-Type" content="text/html; charset=信息参数" /&gt;

meta标签的charset的信息参数如GB2312时，代表说明网站是采用的编码是简体中文；

meta标签的charset的信息参数如BIG5时，代表说明网站是采用的编码是繁体中文；

meta标签的charset的信息参数如iso-2022-jp时，代表说明网站是采用的编码是日文；

meta标签的charset的信息参数如ks_c_5601时，代表说明网站是采用的编码是韩文；

meta标签的charset的信息参数如ISO-8859-1时，代表说明网站是采用的编码是英文；

meta标签的charset的信息参数如UTF-8时，代表世界通用的语言编码；

4、META标签的generator

&lt;meta name="generator" content="信息参数" /&gt;

meta标签的generator的信息参数，代表说明网站的采用的什么软件制作。

5、META标签的author

&lt;meta name="author" content="信息参数"&gt;

meta标签的author的信息参数，代表说明网页版权作者信息。

6、META标签的http-equiv="Refresh"

&lt;Meta http-equiv="Refresh" Content="时间; Url=网址参数"&gt;

meta标签的Refresh代表多少时间网页自动刷新，加上Url中的网址参数就代表，多长时间自动链接其他网址。

7、META标签的HTTP-EQUIV="Pragma" CONTENT="no-cache"

&lt;META HTTP-EQUIV="Pragma" CONTENT="no-cache"&gt;代表禁止浏览器从本地计算机的缓存中访问页面内容,这样设定，访

问者将无法脱机浏览。

8、META标签的COPYRIGHT

&lt;META NAME="COPYRIGHT" CONTENT="信息参数"&gt;

meta标签的COPYRIGHT的信息参数，代表说明网站版权信息。

9、META标签的http-equiv="imagetoolbar"

&lt;meta http-equiv="imagetoolbar" content="false" /&gt;

指定是否显示图片工具栏，当为false代表不显示，当为true代表显示。

10、META标签的Content-Script-Type

&lt;Meta http-equiv="Content-Script-Type" Content="text/javascript"&gt;

W3C网页规范，指明页面中脚本的类型。

11、META标签的revisit-after

&lt;META name="revisit-after" CONTENT="7 days" &gt;

revisit-after代表网站重访,7 days代表7天，依此类推。

12、META标签的Robots

&lt;meta name="Robots" contect="信息参数"&gt;

Robots代表告诉搜索引擎机器人抓取哪些页面

其中的属性说明如下：

信息参数为all：文件将被检索，且页面上的链接可以被查询；

信息参数为none：文件将不被检索，且页面上的链接不可以被查询；

信息参数为index：文件将被检索；

信息参数为follow：页面上的链接可以被查询；

信息参数为noindex：文件将不被检索，但页面上的链接可以被查询；

信息参数为nofollow：文件将被检索，但页面上的链接不可以被查询；

2、英文前缀meta-前缀 pref.

1.表示"变化","变换"

2.表示"继","在...之后"

3.表示"超越"

4.表示"在...之间","介于"

例词：metaphysics

n.

1.形而上学；玄学

2.深奥莫测的推理；空谈；空头理论

来自希腊语，最初来源是作为亚里士多德所著《形而上学》一书的书名，意指“第一哲学”，也就是以“作为存在的存在（being as being）”为研究对象的形而上学，其意义为“在具体科学之后”

3.模板meta语言

模板meta语言由GDMO提出，采用类似于BNF的语法，因此与ASN.1相似，只要了解了它与ASN.1的不同之处就可以在ASN.1有关知识的基础上正确使用。因此，这里只将有关要点进行如下说明：

1.分号（；）用于终止结构和中止模板

2.空格，空行，注释和行尾只起分割符的作用。在需要标志一个元素结束，另一个元素开始时使用。

3.注释由双连字符（--）引导，在行尾或遇到另外的双连字符终止。可以出现在任何分隔区中，但不能出现在结构名或模板名所包含的空格之间。

4.方括号（[ ]）用于指出模板定义中的可选元素。

5.右圆括号中的星号（*）指出模板定义中的可选元素。

6.选择对象由竖线（|）分割。这个符号旨在支持件的定义中使用。

7.将由用户确定的字符串扩在尖括号(&lt;&gt;)中。

8.附件用一个引用标号，后接符号-&gt;&gt;,后接一个由文本字符串和符号构成的语法定义组成。

9.分隔串出现在模板定义中自然语言文本或形式说明文本之中。他们由任意的字符串组成，字符串可以由以下任意一个分隔符引导和终止。分隔符是“$ % ^ &amp; * ` ' ~ ? @ \”。如果分隔串由某个分隔符开始，则这个分隔串直到再次遇到相同的分隔符才结束。

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;