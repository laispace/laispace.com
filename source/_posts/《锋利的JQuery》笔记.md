title: 《锋利的JQuery》笔记
id: 13
categories:
  - Javascript
date: 2012-07-13 22:11:56
tags:
---

<div><span>  #1.2 JQ的特点
</span>1.轻量级--压缩后只有30K（用Packer）、18K（用Gzip）2.强大的选择器--兼容CSS1到CSS3的选择器并可自定义选择器3.出色的DOM操作的封装

4.可靠的事件处理机制

5.完善的Ajax--所有Ajax操作封装到了函数$.ajax()里

6.不污染顶级变量--JQ只建立一个Jquery的对象，所有函数都在该对象下

7.出色的浏览器兼容性

8.链式操作方式--一组操作，可以直接连写而无重复获取对象

9.隐式迭代

10.行为层与结构层的分离--可用选择器选中元素直接添加事件；后期维护方便

11.丰富的插件支持

12.完善的文档

13.开源
<div></div>
<div>#1.32 在JQ库中，$就是JQ一个简写形式，如$("#foo")就代表JQuery("#foo")</div>

$(document).ready(function(){

//...

});

可简写为：

$(function(){

//...

});

&nbsp;

#1.3.3统一代码风格，方便日后维护

1.链式操作风格

1）对于同一个对象不超过三个操作的，直接写成一行。

如$("li").show().unbind("click");

2)对于同一个对象的较多操作，建议每行写一个操作。

行数过多则可以功能块来换行

3）对于多个对象的少量操作，可以每个对象写一行，若涉及子元素则适当缩进。

4）对于多对象的较多操作，则结合第2、3条。

2.给代码添加注释，否则很难看懂代码
<div></div>
<div>#1.4.1了解区分JQ和DOM对象的区别</div>
1.DOM对象

每一份DOM都可以表示成一棵树。

DOM对象可以使用JS中的方法，通过getElementsByTagName或getElementById来获取元素节点（DOM对象）

2.JQ对象

JQ对象就是通过JQ包装DOM对象后产生的对象。

只有JQ对象才能使用JQ的方法。

在JQ对象中不能使用DOM对象的任何方法，反之亦然。
<div></div>
<div>#1.4.2 JQ对象和DOM对象的相互转换</div>
约定定义变量的风格是：获取的对象是JQ对象，则加前缀$.

获取的对象是DOM对象不加前缀$.

1.JQ对象转换成DOM对象

1）若JQ对象是一个数组对象，则通过[index]的方法得到相应的DOM对象

2）通过get(index)方法得到相应的DOM对象

2.DOM对象转换成JQ对象

用$()将DOM对象包装就可以获得相应的JQ对象

我们平时用到的JQ对象都是通过$()函数制造的。
<div></div>
<div>#1.6  开发工具推荐</div>

1.DW

支持提示JQ代码,在http://xtnd.us/dreamweaver/jquery下载插件Jquery...API.MXP

2.Aptana

是Ajax开发IDE

3.JQueryWTP和Spket插件

装在Eclipse上的插件

4.Visual Studio 2008

#1.7

1）CSS选择器复习

标签选择器

E{

CSS规则

}

ID选择器

#ID{

CSS规则

}

类选择器

E.className{

CSS规则

}

群组选择器

E1，E2，E3{

CSS规则

}

后代选择器

E F{

CSS规则

}

通配符

*{

CSS规则

}

其他选择器：伪类选择器

E：Pseudo-Elements{

CSS规则

}

     子选择器

E&gt;F{

CSS规则

}

     临近选择器

E+F{

CSS规则

}

     属性选择器

E[attr]{

CSS规则

}

注意，目前并不是所有的浏览器都支持【其他选择器】
<div></div>
<div>2）学习JQ选择器</div>
JQ中的选择器完全继承了CSS的风格

简洁写法：$()作为一个选择器函数

如$("#ID")代替了document.ElementsByTagName()

扩浏览器兼容：不像CSS，JQ选择器不用考虑浏览器是否支持这些选择器
<div></div>
<div>JQ选择器的分类</div>
1)基本选择器/

通过id/class/标签名来查找DOM元素

注意id只能使用一次

2）层次选择器/

3）过滤选择器/

以冒号（：）开头，分为基本过滤、内容过滤、可见性过滤、属性过滤、子元素过滤盒表单对象过滤

4）表单选择器

&nbsp;

#2.5.1 选择器中含有特殊符号的注意事项

1）.选择器中含有"." "#" "(" "]"等字符时,需要用转义字符"\\"(注意是两个反斜杠)

2).JQ1.3.1以后的版本都不能在属性名前加“@”

3)选择器中添加空格可能产生严重错误
<div></div>
<div>#2.7其他选择器</div>
1.JQ选择器是可以进一步拓展的

1）[MoreSelectors for jQuery](http://plugins%2cjquery.com/project/moreSelectors) 这插件用于增加更多的选择器

2）[Basic XPath](http://plugins.jquery.com/project/xpath)插件（使用人数不多且降低了选择器匹配的效率）

2.使用其他CSS选择器的方法

1）document.getElementsBySelector()---通过选择器来获取文档元素

2）cssQuery()-----通过CSS选择器查找元素
<div>3.querySelectorAll()也是用于实现通过CSS选择器来获取元素的（W3C在Selectors API中的方法）</div>
<div></div>
<div></div>
<div> #3.1 jquery中的DOM操作</div>
<span style="color: #333333;">1.DOM core</span>

<span style="color: #333333;">2.HTML_DOM</span>

<span style="color: #333333;">获取某些对象、属性可用这两个来实现，但显然HTML_DOM比较简短，但只能用来处理WEB文档</span>

<span style="color: #333333;">3.CSS_DOM用来获取和设置style对象的各种属性</span>

jquery对DOM的各种操作都围绕DOM树展开

<span style="color: #333333;">1.查找元素节点</span>

&nbsp;

<span style="color: #333333;">2.查找属性节点</span>

<span style="color: #333333;">用attr()方法来获取各种属性的值</span>

<span style="color: #333333;">创建节点</span>

<span style="color: #333333;">1.创建节点，建议添加顺序为：元素-文本-属性</span>

<span style="color: #333333;">var $li_1= $("&lt;li title="属性1"&gt;文本1&lt;/li&gt;");</span>

<span style="color: #333333;">var $li_2= $("&lt;li title="属性2"&gt;文本2&lt;/li&gt;");</span>

<span style="color: #333333;">   将新元素插入到节点ul中</span>

<span style="color: #333333;">$("ul").append($li_1);</span>

<span style="color: #333333;">$("ul").append($li_2);（或采用链式写法：$("ul").append($li_1).append($li_2);）</span>

<span style="color: #333333;">  注意：1）动态创建的新元素节点需要使用方法才能插入文档中而不会自动添加</span>

<span style="color: #333333;">    2）创建元素时，注意书写的规范性（注意闭合标签和使用标准的XHTML格式）</span>

<span style="color: #333333;">2.插入节点</span>

<span style="color: #333333;">几种插入节点的方法-----$("节点").方法("元素")</span>

<span style="color: #333333;">1）append()</span>

<span style="color: #333333;">向每个匹配的元素内部后置内容</span>

<span style="color: #333333;">2）appendTo()</span>

<span style="color: #333333;">后置追加到指定的元素中</span>

<span style="color: #333333;">3）prepend()</span>

<span style="color: #333333;">向每个匹配的元素内部前置内容</span>

<span style="color: #333333;">4）prependTo()</span>

<span style="color: #333333;">后置追加到指定的元素中</span>

<span style="color: #333333;">5）after()</span>

<span style="color: #333333;">在每个匹配的元素后插入内容</span>

<span style="color: #333333;">6）insertAfter()</span>

<span style="color: #333333;">将所有匹配的元素插入到指定元素的后面</span>

<span style="color: #333333;">7）before()</span>

<span style="color: #333333;">在每个匹配的元素之前插入内容</span>

<span style="color: #333333;">8）insertBefore()</span>

<span style="color: #333333;">将所有匹配的元素插入到指定的元素的前面</span>
<div></div>
<div>3.删除节点</div>
<span style="color: #333333;">1）remove()</span>

<span style="color: #333333;">从DOM中删除所有匹配的元素</span>

<span style="color: #333333;">
</span>

<span style="color: #333333;">该方法返回的是一个指向已被删除的节点的引用，因而可以在以后使用这些元素</span>

<span style="color: #333333;">2）empty()</span>

<span style="color: #333333;">清空节点，清空元素的所有后代节点</span>
<div></div>
<div>4.复制节点</div>
复制节点后，新元素并不具有任何行为

<span style="color: #333333;">注意：若传递true为参数，则表示复制元素的同时复制元素中所绑定的事件</span>

<span style="color: #333333;">$(this).clone(true).appendTo("body");//注意参数true</span>
<div><span style="color: #333333;">
</span></div>
<div><span style="color: #333333;">5.替换节点</span></div>
<span style="color: #333333;">replaceWith()</span>

<span style="color: #333333;">将所有匹配的元素都换成指定的HTML或DOM元素</span>

<span style="color: #333333;">replaceAll()</span>

<span style="color: #333333;">与replaceWith()作用相同但颠倒了操作</span>

注意：如果在替换之前已经为元素绑定事件，替换后原先绑定的事件将会与被替换的元素一起消失，需要在新元素上重新绑定事件。

<span style="color: #333333;">6.包裹节点</span>

<span style="color: #333333;">wrap()</span>

<span style="color: #333333;">将某个节点用其他标记包裹起来，不破坏原始文档的语义</span>

<span style="color: #333333;">wrapAll()</span>

<span style="color: #333333;">将所有匹配的元素用一个元素来包裹</span>

与wrap()不同，wrap()是将所有的元素进行单独的包裹

<span style="color: #333333;">wrapInner()</span>

<span style="color: #333333;">将每一个匹配的元素的子内容（包括文本节点）用其他结构化的标记包裹起来</span>

wrap是外包，wrapInner是内包

&nbsp;

<span style="color: #333333;">#3.2.8 属性操作</span>

<span style="color: #333333;">在JQ中，用attr()方法来获取和设置元素属性，removeAttr()方法来删除元素属性</span>

<span style="color: #333333;">1.获取属性和设置属性</span>
<div><span style="color: #333333;">获取用attr()方法</span></div>
<div>也可以直接设置</div>
<span style="color: #333333;">$("p").attr("title","my  title");</span>

<span style="color: #333333;">同时设置多个属性</span>

<span style="color: #333333;">$("p").attr({"title":"my title","name":"test"});</span>

<span style="color: #333333;">2.删除属性</span>

<span style="color: #333333;">用removeAttr()方法删除某个元素的特定属性</span>

<span style="color: #333333;">$("p").removeAttr("title");</span>
<div><span style="color: #333333;">
</span></div>
<div><span style="color: #333333;">#3.2.9  样式操作</span></div>
<span style="color: #333333;">1.获取样式和设置样式</span>

<span style="color: #333333;">获取</span>

<span style="color: #333333;">var p_class$=$("p").attr("class");</span>

<span style="color: #333333;">设置</span>

<span style="color: #333333;">$("p").attr("class","high");</span>

<span style="color: #333333;">2.追加样式</span>

<span style="color: #333333;">用addClass()方法</span>

区别：attr()设置样式会覆盖替换，而addClass()则是追加兼并

3.移除样式

<span style="color: #333333;">用removeClass()方法</span>

<span style="color: #333333;">$("p").removeClass("high");//移除&lt;p&gt;元素中值为"high"的class</span>

<span style="color: #333333;">若要删除多个class，则</span>

<span style="color: #333333;">$("p").removeClass("high another");</span>

<span style="color: #333333;">或者直接使用</span>

<span style="color: #333333;">$("p").removeClass();//默认移除所有class</span>

<span style="color: #333333;">4.切换样式</span>

<span style="color: #333333;">toggle()</span>

<span style="color: #333333;">交替执行代码③、④函数，控制行为上的重复切换</span>

$toggleBtn.toggle(

<span style="color: #333333;">function(){</span>

<span style="color: #333333;">//显示元素      代码③</span>

<span style="color: #333333;">},</span>

<span style="color: #333333;">function(){</span>

<span style="color: #333333;">//显示元素       代码④</span>

<span style="color: #333333;">}</span>

<span style="color: #333333;">)</span>

<span style="color: #333333;">toggleClass()</span>

<span style="color: #333333;">如果类型存在则删除，否则添加。控制样式上的重复切换。</span>

<span style="color: #333333;">5.判断是否含有某个样式</span>

<span style="color: #333333;">hasClass()方法，若有则返回true，否则返回false</span>

<span style="color: #333333;">$("p").hasClass("another");</span>

<span style="color: #333333;">等价于：</span>

<span style="color: #333333;">$("p").is(".another");</span>
<div><span style="color: #333333;">
</span></div>
<div><span style="color: #333333;">#3.2.10 设置和获取HTML、文本和值</span></div>
<span style="color: #333333;">1.html()</span>

<span style="color: #333333;">读取或者设置某个元素中的HTML内容（与javascript中的innerHTML属性类似）</span>

<span style="color: #333333;">2.text()</span>

<span style="color: #333333;">读取或者设置某个元素中的文本内容（与javascript中的innerRext属性类似）</span>

<span style="color: #333333;">3.val()</span>

<span style="color: #333333;">读取或者设置元素的值(与javascript中的value属性类似)</span>

<span style="color: #333333;">元素师文本框、下拉列表、单选框都能返回，若元素为多选，则返回包含所有选择的值的数组</span>

<span style="color: #333333;">val()方法还能使select、checkbox、radio相应的选项被选中，在表单操作中会经常用到</span>
<div></div>
<div>#3.2.11 遍历节点</div>
<span style="color: #333333;">1.children()</span>

<span style="color: #333333;">取得匹配元素的子元素的集合</span>

<span style="color: #333333;">2.next()</span>

<span style="color: #333333;">取得匹配元素后面紧邻的同辈元素</span>

<span style="color: #333333;">3.prev()</span>

<span style="color: #333333;">取得匹配元素前面紧邻的同辈元素</span>

<span style="color: #333333;">4.siblings()</span>

<span style="color: #333333;">取得元素前后所有的同辈元素</span>

<span style="color: #333333;">5.closest()</span>

<span style="color: #333333;">取得最近的匹配元素</span>

<span style="color: #333333;">6.其他方法</span>

<span style="color: #333333;">find()/filter()/nextAll()/preAll()/parent()/parents()等</span>

<span style="color: #333333;">#3.2.12 CSS-DOM操作</span>

<span style="color: #333333;">css()</span>

<span style="color: #333333;">获取元素的样式属性 </span>

设置某个元素的样式

<span style="color: #333333;">$("p").css("color");     //获取&lt;p&gt;元素的样式颜色</span>

<span style="color: #333333;">$("p").css("color","red");//设置&lt;p&gt;元素的样式颜色为红色</span>

<span style="color: #333333;">$("p").css({"fontSize":"30px","backgroundColor":"#888888"});//同时设置字体颜色和背景色</span>

<span style="color: #333333;">height()</span>

<span style="color: #333333;">取得匹配元素当前计算的高度值</span>

<span style="color: #333333;">$("p").height(100);//设置高度值为100px</span>

<span style="color: #333333;">$("p").height("10em");//设置高度值为10em</span>

<span style="color: #333333;">css()获得的高度值与样式的设置有关，可能得到“auto”“10px”之类的字符串，而height()方法获得的则是在页面中的实际高度，与样式设置无关且不带单位</span>

<span style="color: #333333;">width()</span>

<span style="color: #333333;">取得匹配元素的宽度值（px）</span>

<span style="color: #333333;">$("p").width();//获取宽度值</span>

<span style="color: #333333;">$("p").width("40px");设置宽度值</span>

<span style="color: #333333;">几个常用方法</span>

<span style="color: #333333;">1.offset()</span>

<span style="color: #333333;">获取元素在当前视窗的相对位移，返回top和left属性，只对可见元素有效</span>

<span style="color: #333333;">var offset=$("p").offset();//获取offset</span>

<span style="color: #333333;">var left=offset.left;//获取左偏移</span>

<span style="color: #333333;">var top=offset.top;//获取上偏移</span>

<span style="color: #333333;">2.position()</span>

<span style="color: #333333;">获取元素相对于最近一个position样式属性设置为relative或者absolute的祖父节点的相对位移，也返回top和left属性</span>

<span style="color: #333333;">var position=$("p").position();//获取offset</span>

<span style="color: #333333;">var left=position.left;//获取左偏移</span>

<span style="color: #333333;">var top=position.top;//获取上偏移</span>

<span style="color: #333333;">3.scrollTop()</span>

<span style="color: #333333;"> scrollLeft()</span>

<span style="color: #333333;">获取元素的滚动条距顶端和左侧的距离</span>

<span style="color: #333333;">var $p=$("p");</span>

<span style="color: #333333;">var scolltop=$p.scollTop();</span>

<span style="color: #333333;">var scollleft=$p.scollLeft();</span>

<span style="color: #333333;">另外，可指定参数让滚动条滚动到指定位置</span>

<span style="color: #333333;">$("textarea").scollTop(300);//元素的垂直滚动条滚动到指定位置</span>

<span style="color: #333333;">$("textarea").scollLeft(300);//元素的横向滚动条滚动到指定位置</span>

&nbsp;
<div></div>
&nbsp;

</div>