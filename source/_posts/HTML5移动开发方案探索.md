title: HTML5移动开发方案探索
tags:
  - html5
id: 551
categories:
  - Hybrid
date: 2014-01-09 23:12:08
---

2013是移动互联爆发的一年，2014也肯定也是移动互联网的天下！

笨鸟先飞，小赖打算进入移动开发，先从HTML5移动应用开始，再连带学学IOS，希望2014会有更大的收获。

查阅资料后就开始！不过要做好被移动开发各种坑坑死的打算先 - -
<div>
<div>

**三种开发方式的简单比较：**

** Native（原生）：**

*   丰富的用户体验
*   平台指向性
*   久经考验的移动应用开发途径
**Hybrid（混合）：**

*   与应用类似的使用体验
*   利用设备自身功能
*   多平台支持能力
**HTML 5：**

*   更快的开发周期
*   跨平台运行
*   实时更新
<div><span style="color: #333333; font-family: 宋体;">
</span></div>
</div>
<div><span style="color: #333333; font-family: 宋体;">Native开发方法在性能和设备访问方面很出色，但成本和更新方面有缺点。Web方法更新起来简单得多，成本较低，也更容易，但是目前功能有限，也无法获得使用Native API调用所能获得的那种出色的用户体验。Hybrid开发方法提供了折中方案：在许多情况下，它集两者之所长，如果开发者面向多种操作系统更是如此。</span></div>
<div><span style="color: #333333; font-family: 宋体;">Hybrid是同时利用HTML 5与CSS3创建移动UI，同时又通过JavaScript代码实现与移动SDK之间的通信。</span></div>
<div><span style="color: #333333; font-family: 宋体;">
</span></div>
<div><span style="color: #333333; font-family: 宋体;">目前比较知名的hybrid框架有Phonegap：</span></div>
<div><span style="color: #333333; font-family: 宋体;">     PhoneGap为移动应用开发人员提供一套名为phonegap-3.0.0.js的JavaScript API。该JavaScript API会调用PhoneGap的特殊平台引擎/桥接机制，后者则反过来调用原生平台SDK以实现对设备的操作，例如访问联系人名单或者拨打电话等。</span></div>
<div>![](file:///C:/Users/%E5%B0%8F%E8%B5%96/AppData/Local/Temp/enhtmlclip/Image.png)</div>
<div>PhoneGap还提供一套与HTML 5、JavaScript以及CSS3在非Chrome浏览器（例如不提供用户界面的浏览器）中相绑定的创建系统。</div>
<div>[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/01/Image1.png "Image1")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/01/Image1.png)</div>
<div>[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/01/Image2.png "Image2")](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/01/Image2.png)</div>
<div>![](file:///C:/Users/%E5%B0%8F%E8%B5%96/AppData/Local/Temp/enhtmlclip/Image(1).png)</div>
<div></div>
<div>phonegap提供了接口，使我们通过编写JS代码，调用原生的API。为了快速开发，一般还会搭配一个移动开发框架，常见的有jquery mobile、KendoUI Mobile、Sencha Touch等。</div>
<div>     Jquery mobile 优点是比较容易上手，而且为了设计jQuery Mobile页面，提供了一套便捷的代码设计工具——也就是Codiqa。</div>
<div>缺点是应用的列表条目一旦达到五十到六十个，性能就会出现疲软（甚至直接导致移动浏览器崩溃）。在另一方面，Sencha Touch能够载入超过两百个条目，且不会引发任何性能问题。</div>
<div>     KendoUI Mobile是一款基于MVVM的移动应用框架，附带图表及多款非常实用的移动工具，整体方案售价为699美元。即便它有非常好的表现，但比较昂贵，就暂且不考虑了。</div>
<div>     Sencha Touch用极高的使用复杂性外加相当夸张的学习曲线换得无与伦比的性能表现。Sencha Touch属于MVC且完全采用JavaScript机制，但由于Sencha Touch最初只针对iOS平台，而后才添加了对Android、黑莓以及Windows Phone的支持能力，因此大家应该做好心理准备——其在各平台上的性能表现并不完全一致。</div>
<div></div>
<div>查阅资料后，小赖得出的方案是：使用Phonegap+jquery mobile 开发这个HTML5 Hybrid应用。</div>
<div>phonegap好在提供了跨平台的方案，坏在当应用复杂起来，性能是个问题；</div>
<div>jquery mobile 好在提供了一些UI，轻量且快速，坏在一旦DOM操作繁多，会造成性能问题。</div>
<div></div>
<div>Hybrid的优点在于用跨平台Web技术，开发应用程序的大部分代码，又可以在需要时直接访问Native API。</div>
<div>我的想法是，先按Hybrid的方案做出产品原型，然后再评估是否需要针对特定功能用NativeAPI来加强改善。</div>
<div></div>
<div>以上都是Ctrl C+V加自己的理解得出的初步方案，接着就需要去具体实践，得出自己真正的开发心得和新方案来了！</div>
<div></div>
<div>寒假继续加油！</div>
<div></div>
<div>参考链接：</div>
<div>[【白皮书】HTML5、Native或Hybrid App开发全接触]([http://www.360doc.com/content/13/1128/18/21412_332891741.shtml](http://www.360doc.com/content/13/1128/18/21412_332891741.shtml))</div>
<div>[专题：跨平台移动web中间件PhoneGap开发入门]([http://www.360doc.com/content/13/1115/09/9200790_329335555.shtml](http://www.360doc.com/content/13/1115/09/9200790_329335555.shtml))</div>
<div>[phoneGap可行性分析]([http://sunny-liang.iteye.com/blog/1452495](http://sunny-liang.iteye.com/blog/1452495)) 这个写得非常棒</div>
<div>[开发者眼中的PhoneGap体验]([http://www.360doc.com/content/13/1115/08/9200790_329325455.shtml](http://www.360doc.com/content/13/1115/08/9200790_329325455.shtml))</div>
<div>[四大Hybrid App移动开发平台对比]([http://cio.zdnet.com.cn/cio/2013/0427/2157148.shtml](http://cio.zdnet.com.cn/cio/2013/0427/2157148.shtml))</div>
<div>[Phonegap3.0特性]([http://www.cocoachina.com/applenews/devnews/2013/0724/6665.html](http://www.cocoachina.com/applenews/devnews/2013/0724/6665.html))</div>
<div>[手机应用开发平台选择资料汇总]([http://lanhy2000.blog.163.com/blog/static/4367860820131874524745/](http://lanhy2000.blog.163.com/blog/static/4367860820131874524745/))</div>
<div>[如何选择AppCan与PhoneGap跨平台开发框架]([http://cio.zdnet.com.cn/cio/2013/0628/2165993.shtml](http://cio.zdnet.com.cn/cio/2013/0628/2165993.shtml))</div>
<div>[Phonegap VS AppCan]([http://www.cnblogs.com/comsokey/archive/2012/08/30/PhonegapVSAppCan.html](http://www.cnblogs.com/comsokey/archive/2012/08/30/PhonegapVSAppCan.html))</div>
<div></div>
<div></div>
<div>参考实例：</div>
<div>[Phonegap官方实例]([http://phonegap.com/app/feature/](http://phonegap.com/app/feature/))</div>
<div>[如何使用 jQuery Mobile 与 PhoneGap 来开发移动应用]([http://www.open-open.com/lib/view/open1323767996984.html](http://www.open-open.com/lib/view/open1323767996984.html))</div>
<div>[HTML5开发实战之网易微博]([http://uedc.163.com/9494.html](http://uedc.163.com/9494.html))</div>
<div>[手机搜狐-移动Web单页应用开发实践]([http://bbs.9tech.cn/index.php/topic/show/363616#top](http://bbs.9tech.cn/index.php/topic/show/363616#top))</div>
<div>[腾讯广告平台产品团队谈PhoneGap使用]([http://www.infoq.com/cn/news/2013/01/tencent-ad-department-phonegap](http://www.infoq.com/cn/news/2013/01/tencent-ad-department-phonegap))</div>
<div><span style="color: #333333; font-family: 宋体;">
</span></div>
<div><span style="color: #333333; font-family: 宋体;">
</span></div>
<div><span style="color: #333333; font-family: 宋体;">
</span></div>
</div>