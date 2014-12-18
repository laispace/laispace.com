title: CSS碎片积累
categories:
  - CSS
tags:
  - 碎片
date: 2013-03-02 21:18:41
---

- /* em的理解 */
em是相对于父元素来计算大小的，浏览器默认为16px，设置body为0.75em(16*0.75=12px)后，若设置再body里的div为1em，则div里的文字大小实则为12px。
PS：若设置ol为font-size:0.6em;则嵌套的ol为60%*60%=36%的大小，列表嵌套会出问题。原则上用em定义字体时，嵌套不超过两层。
- /* 文字底部对齐 */
设置文本在div里底部对齐用vertical-align：bottom;是不够的，还需设置display:table-cell;才会有效果。
- /* 内容居中文字不居中 */
设置div绝对定位在网页中间position：absolute;后设置margin:0 auto;即可。设置内容居中但文字不跟着居中，另加text-align:left;即可。
- /* 行高建议用相对单位 */
设置行高时建议使用百分比%或者em为单位。
- /* 浏览器默认行高 */
浏览器默认行高为1.2em，建议设置为1.6~1.8em。
-/* 上下边距的合并 */
在CSS中，上下的margin都设置时，取大的margin值合并，而不是简单的叠加。
-
不能简单地用line-height来替代margin的值，因为不同浏览器的解析不一样。
- /* 图片替代文字 */
图片代替文字时设置text-indent：-999px;要加overflow:hidden;
最佳实践是Kellum法：
[css]
.hide-text {
     text-indent: 100%;
     white-space: nowrap;
     overflow: hidden;
}
[/css]
- /* 隐藏元素的方法 */
visibility:hidden;元素不可见，但占据原来位置；display:none;元素不可见，不占据原来位置。
-
图像作为链接时默认会有蓝色边框，与 a 的默认样式一致

- /* 元素居中的方法 */
方法一：设置width和margin；
方法二：子元素inline-block父元素text-align设置为center；
方法三：
div设置为float:left;position:relative;
div下的ul设置为float:right;position:relative;left: 50%;/*整个分页向右边移动宽度的50%*/
ul下的li设置为float:left;position:relative;right:50%;
[原理分析](http://www.w3cplus.com/css/elements-horizontally-center-with-css.html)
方法四：绝对定位，原理类似方法三
div设置为relative；
ul设置为absolute，left：50%；
li设置为relative，float，right:50%;
方法五：CSS3的flex实现水平居中方法
方法六CSS3的fit-content实现水平居中方法
- /* 图片宽度自适应容器的宽度 */
1\. 从固定宽度改为流式宽度，面临的一个主要问题是图片的显示尺寸。而这个问题在css中有个简单的解决方法，就是只需要设置图片的宽度是100%。
2\. 制作自适应大小的图片，即背景图片总是占满容器：给div设置固定的高度并设置背景图片为居中：
[css]
div.auto_image{
height: 200px;
margin: 0 auto;
background: url(auto_image.jpg) no-repeat center;
}
[/css]

- /* 高亮用id定位的元素 */
用#id定位页面内的元素时，稍稍高亮背景颜色提升体验：
[css]
div:target{
background:#333;
}
[/css]

- /* outline替代border做测试 */
之前尝试在鼠标hover一个图像时突出当前图像使用的是border，但总是会影响到周边元素，应该使用outline：不占据空间！outline的属性跟border一样：outline:1px solid #eee;
- /* 消除relative图片偏移后的空白*/
经常将一个图片使用relative定位，有了一定的位移后原位置空白，可设置负边距让文字填充进来。
- /* 关系选择器 + 的妙用 */

使用相邻选择符时常用h3 + p 来h3后的第一个p，别忘了h3 + p + p选择第二个p，以此类推
- /* inline-block */
inline-block会激发IE的haslayout，且注意inline-block元素间若有空格，会有影响
- /*  z-index的理解 */
z-index只对定位元素起作用。如果你尝试对非定位元素设定一个z-index值，那么肯定不起作用。
- /* CSS3动画 */
CSS3 是个独立于 JS 的线程，这个特点目前已经在 Desktop Safari / IOS Safari / Android Chrome 中被支持，所以说，移动 webapp 中的动画应用，尽可能使用 CSS3 吧
- /* 负边距的妙用 */
当static元素的margin-top/margin-left被赋予负值时，元素将被拉进指定的方向。例如：
/* 元素向上移10px*/
#mydiv1 {margin-top:-10px;}
但如果你设置margin-bottom/right为负数，元素并不会如你所想的那样向下/右移动，而是将后续的元素拖拉进来，覆盖本来的元素。
[css]
#mydiv1 {
margin-bottom:-10px;      /* #mydiv1后续元素向上移10px, #mydiv1 本身不移动 */
}[/css]
- /* box-sizing的使用 */
处理盒模型时，让width包括padding和border的宽度：
[css]
* {/*所有元素*/
 box-sizing: border-box;/*别忘了加浏览器前缀*/
}[/css]

- /* 简易浮动 */
[css]

.clearfix{
 overflow: auto;
 zoom: 1;/*兼容IE6*/
}

[/css]

//lxl:最佳实践

[css]
/* 现代浏览器 */
.cf:before,
.cf:after {
     content: &quot;&quot;;
     display: table;
}

.cf:after {
     clear: both;
}

/* IE6/7 触发hasLayout */
.cf {
     *zoom: 1;
}     
[/css]
- /* 媒介查询 */

[css]

@media screen and (min-width:600px) {
nav { float: left; width: 25%;}
}
@media screen and (max-width:599px) {
nav li { display: inline;}
}

...
[/css]

- /* 视窗宽度的理解 */
指定视窗宽就仿佛告诉了浏览器你的网页在这个宽度下显示是最合适的。要是做了一个专门在iPhone上浏览的网页，那么你就设置视窗宽为320px.
但是这不易于 响应式设计，因为在平板上浏览它的时候，它会被缩小到320px的区域中。在响应式设计中最好指定视窗宽和设备的屏宽一致。
[html]
&lt;meta name=&quot;viewport&quot; content=&quot;width=320&quot;&gt;
[/html]

/* 视窗缩放 */
在移动端，屏幕上开合手指可以控制缩放，但当你设定视窗宽和设备宽度一致时，就没必要去放大浏览整个网页了。为了确保网页的初始显示不是放大过的，可以用initial-scale属性来设置初值。若用户在浏览过程中不需要缩放，你可以设置它为1。
[html]
&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width&quot;&gt;
[/html]

甚至是如果你连用户的滚屏操作都想禁止，你可以设置maximum-scale为1，这样就完全不能放大了。
[html]
&lt;meta name=&quot;viewport&quot; content=&quot;maximum-scale=1&quot;&gt;
[/html]

-/* HTML4与HTML5文档头的理解 */
 HTML 4.01 中的 doctype 需要引用一个 DTD，这是因为 HTML 4.01 基于 SGML。HTML 5 不基于 SGML，也不需要引用 DTD，但是需要声明文档类型让浏览器按照它们应该的方式来运行。
-/* 表单值设为disabled将不会被发送 */
如果一个元素被设置成 disabled, 那么它的值就不会被发送的server端。 正确的做法应该是使用 readonly。
禁用（disabled）：value 不会在 Form 提交时发送出去。这个对于按钮来说用处比较大，一般的 type="text" 最好是隐藏，而不是禁用，因为它不需要发送数据。
只读（readonly）：value 会在 Form 提交时被发送出去。所以需要在外观上显示跟一般 input/textarea 一样，但不允许用户修改数据，可以用这个属性。
隐藏（hidden）：这个比较好理解，value 会被发送，并且用户看不到。
- /* :after和:before的妙用 */
配合 :after 或者 :before 在CSS中可以用attr()显示HTML属性值
[css]
@media print {
 a:after {
 content: &quot; (link to &quot; attr(href) &quot;) &quot;;
 }
[/css]
[html]
&lt;a href=&quot;http://example.com&quot;&gt;Visit our home page&lt;/a&gt;
[/html]

-/* css的函数方法 */
使用counter()在列表中自动添加序号
[css]
body {
 counter-reset: heading;
 }
h4:before {
 counter-increment: heading;
 content: &quot;Heading #&quot; counter(heading) &quot;.&quot;;
 }
[/css]

/* 使用calc()做算术 */
[css]
.parent {
 width: 100%;
 border: solid black 1px;
 position: relative;
 }
.child {
 position: absolute;
 left: 100px;
 width: calc(90% - 100px);
 background-color: #ff8;
 text-align: center;
 }
[/css]

- /* 关闭自动补全 */
关闭浏览器自动补全不是autocomplete="false" 而是autocomplete="off"，可防止自动补全插件与其的冲突
-
对em和ex的正确理解：em 是当前字体下 M 的宽度，而 ex 是当前自提下 x 的高度
- /*边距值单位的妙用*/
外边距单位的采用，margin-left和margin-right采用px而margin-top和margin-bottom采用em,能保证缩放时边距的自适应：
[css]
 p {font-size: 1em; margin: .75em 30px;} /* 缩放时边距自适应 */

[/css]

- /* 盒子模型的理解 */
盒子模型结论1：没有设定元素的宽度即width:auto;时，content、padding、margin、border的总宽度占满父元素宽度；
盒子模型结论2：有设定元素的宽度即width:400px;时，padding、margin、border的总宽度在原设定的宽度上增加；
即：盒子的width设定的只是盒子content区的宽度，而非盒子要占据的水平宽度
盒子模型结论对于布局的用法：
假定是三列等宽布局，nav、article、adise都是浮动且宽度设定后，若为其中任何一个添加padding、margin,则会让总宽度超出，解决方法是：
为每一个浮动的元素增加一个内部div.inner,给内部的div.inner设定padding、margin，这么以来，内部div的总宽度总是等于父元素最初设定的宽度了。
缺点是，这么一来，得跟极度赞成标签语义化的同行们有一番争吵了。
除此之外，可使用CSS3的box-sizing属性，设置width的计算方式：box-sizing ： content-box || border-box || inherit
content-box:Element Width/Height = border+padding+content width/height
border-box:Element Width/Height = 0 + 0 +content width/height
注：IE6/IE7不支持box-sizing,可使用一个polyfill（腻子脚本）borderBoxModel.js来兼容
建议使用：* {box-sizing: border-box} // 权衡利弊?
- /* 防止元素长度撑破容器 */
防止未来出现过大的元素（特别是在动态网站中，一些过长的url都可能撑破容器）的一种思路：
给子元素 img {max-width: 100%};
或父元素：
overflow: hidden; /* 截断超出的元素（而非缩放）*/
word-wrap: break-word; /* 让一些长的字符串如url换行显示 */
-/* 三栏布局的方法 */
三栏布局-中栏流动布局的思路：中栏改变大小时右栏使用负外边距
[html]

&lt;div id=&quot;threecolwrap&quot; style=&quot;width:100%; float:left;&quot;&gt;
&lt;div id=&quot;twocolwrap&quot; style=&quot;margin-right:-210px;float:left;width:100%;&quot;&gt;
&lt;nav style=&quot;float:left;width:150px;&quot;&gt;固定宽度&lt;/nav&gt;
&lt;article tyle=&quot;margin-right:210px;width:auto;&quot;&gt;给自身margi-right:210px;并给父元素margin-right:-210px&lt;/article&gt;
&lt;/div&gt;
&lt;aside id=&quot;onecolwrap&quot; width:210px;float:left;&gt;固定宽度为外边距210px&lt;/aside&gt;
&lt;/div&gt;

[/html[/html]

-
/* 一个更简单巧妙的办法实现三栏布局(推荐)*/
给三个栏都设定display:table-cell让他们具有表格的属性就行了：（缺点是IE7以下的不兼容,且没有polyfill脚本，觉悟吧！）
[html]
&lt;nav style=&quot;display:table-cell;width:150px;&quot;&gt;固定宽度&lt;/nav&gt;
&lt;article style=&quot;display:table-cell;width:auto;&quot;&gt;宽度自适应&lt;/article&gt;
&lt;aside style=&quot;display:table-cell;width:210px;&quot;&gt;固定宽度&lt;/aside&gt;
[/html]

-
清除浮动的几种方法
方法一：父元素overflow:hidden; //不能在下拉菜单中使用
方法二：父元素也浮动，父元素下的元素clear:both; // 不能对靠外边距居中的元素使用
方法三：父元素内最后放一个清楚浮动的元素div.clearFloat
方法四：父元素.clearfix添加伪类模拟方法三（推荐使用,见上文）
- /* 背景图片定位不同单位下的理解 */
使用关键字和百分比的情况下，理解background-position：33% 33%;时，是背景图片的33%处与元素的33%处重叠！
使用像素的绝对数值的情况下，理解background-position：33px 33px;时，图片左上角被放在(33px,33px)的地方！
- /* 添加水印的方法 */
background-attachment: fixed;常用给body中间添加水印（默认值是scroll）
- /* font-family常见字体系列 */
font-family字体栈中最后一个通常是字体类的名字，能保证最坏情况下文档能以正确的字体显示。常用的字体类有：
font-family: serif; // 衬线字体
font-family: sans-serif; // 无衬线字体
font-family: monospace; // 等宽字体
font-family: cursive; // 草书体或手写体
font-family: fanstasy; // 一般是奇怪的字体
- /* font-xxx属性的理解 */
font-weight中只有bold和normal才得到了浏览器的广泛支持，建议只使用这两个属性
font-varient中只有normal和small-caps（小型大写字母）
font的简写注意顺序和格式：
font: italic small-caps bold 12px/1.5em arial,verdana;
即顺序为：
font : font-style || font-variant || font-weight || font-size || line-height || font-family
默认值为：normal normal normal medium normal "Times New Roman" 。
font-size和font-family是必写项，font-size和line-height只能通过斜杠/组成一个值，不能分开写，且应该在font-family前。
- /* text-indet缩进是继承的 */
text-indent原来是有继承的，且是计算得出的结果父元素400px * 5% = 20px，则子元素默认缩进20px !
-
保持id和class最少却又能准确定位元素：正确的方法是设定一个id，以该id为hook（路标），选择它的子元素进行定位，同时提高HTML和CSS代码的易读性！// lxl:缺点是牺牲了权重
- /* 非首位子选择符 */
 ul li + li {border-top:1px solid #eee;}解决导航菜单不用给最后一个li添加类然后去掉最后一个li的下边框的问题
- /* 代码分离提高重用性 */ 
在写一个功能性的代码，如下拉菜单时，建议将功能代码和视觉代码分开写，可提高重用性
- /overflow:hidden对定位元素的影响/
overflow:hidden 会修剪相对定位（position:relative）的元素，但并不总是会隐藏绝对定位元素。
- /* zoom触发IE的hasLayout属性 */
zoom为ie私有css属性，一般用来触发ie的hasLayout属性，解决ie下的浮动，margin重叠等一些问题。
- /* 图片优化 */
减少颜色数可减小PNG图片的大小
- /*禁止修改文本框大小*/
[css]
textarea { resize: none; } /* 禁用textarea的大小改变 */
[/css]
- /* 自定义光标样式 */
cursor: url(cursor.png), default; /* 添加default才能在chrome激活cursor rule,否则不能显示*/
- /* 网页点击启动QQ */
[html]
&lt;a href=&quot;tencent://message/?uin=545183877&amp;Site=JooIT.com&amp;Menu=yes&quot;&gt;点击启动QQ和lxl聊天！&lt;/a&gt;
[/html]

- /* 顶部栏和底部栏留空 */
对footer或者header设定了position：fixed；要记得给他们腾出个地方来。对于footer就可在body中设定一个margin-bottom的值给它。
-
使用CSS的attr和content属性，改变title的样式：
[html]
&lt;p class=&quot;tooltip&quot; data-title=&quot;I'am data-title&quot;&gt;Hover me！&lt;/p&gt;
&lt;p class=&quot;tooltip&quot; data-title=&quot;I'am data-title&quot; title=&quot;I'am title&quot;&gt;注意，使用了自定义的data-title而没有写原属性title，可防止冲突产生&lt;/p&gt;
[/html]

[css]
.tooltip:hover:after {
     content: attr(data-title);
     background: #eee;
     border: 1px solid red;
     position: relative;
     top: -26px;
     left: -40px;
}
[/css]

- /* 快速检查a标签href属性 */
[html]
&lt;a id=&quot;show-href&quot; href=&quot;http://laispace.com&quot;&gt;来思碑&lt;/a&gt;
[/html]

[css]
#show-href:hover:after {
     content: attr(href);
     background: #eee;
     border: 1px solid red;
 }
[/css]

- /* IE6 margin加倍的问题 */
当box为float时，IE6中box左右的margin会加倍