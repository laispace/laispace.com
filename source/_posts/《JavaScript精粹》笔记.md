title: 《JavaScript精粹》笔记
id: 86
categories:
  - Javascript
date: 2012-08-26 00:35:06
tags:
---

#2.1空白

用空格来分隔字符序列,使代码美观
<pre class="brush:javascript">var that = this;</pre>
javascript的两种注释，注释一定要精确地描述代码。

javascript中的/*注释*/对代码来说并不安全，故用//来代替。

&nbsp;

#2.2标示符

javascript不允许用保留字来命名变量、参数和属性。

即标示符用于语句、变量、参数、属性名和标记。

&nbsp;

#2.3数字

1.javascript只有单一的数字类型（64位的浮点数），避免了短整数溢出的问题和音数字类型导致的错误。

NaN不等于任何值，可用函数isNaN(number)来检测。

2.数字拥有方法。Math包含一套作用于数字的方法。

&nbsp;

#2.4字符串

1.javascript创建的字符都是16位的（Unicode字符集）。

2.javascript没有字符类型，要显示一个字符，只需要创建一个仅包含字符的字符串。

3.转义字符允许把正常情况下不被允许的字符插入到字符串中，如"\u0041"代表十六进制的数字

4.字符串是不可变的，一旦被创建就无法改变，但能通过 + 运算符去连接其他字符串而得到新的字符串

'c' + 'a' + 't' === 'cat'是true（包含完全相同的字符且字符顺序相同）

5.字符串存在一些方法，如：
<pre class="brush:javascript">' cat '.toUpperCase() === 'CAT'</pre>
&nbsp;

#2.5语句

web浏览器缺少链接器，故javascript把语句抛入一个公共的全局名字空间中。

1.var语句呗用在函数内部时，定义了这个函数的私有变量。

2.switch、while、for和do语句允许有一个可选的前置标签（label），配合break使用

3.javascript的代码块不会创建新的作用域，因此变量应该被定义在函数的顶端而不是代码块中

4.以下值为假值（falsy）:

false/null/undefined/空字符串 ' ' /数字 0 /数字 NaN

其余所有值当做真值，包括true/"false"以及所有的对象

#2.6表达式

&nbsp;

#2.7字面量

&nbsp;

#2.8函数

&nbsp;

&nbsp;

#3对象

数字/字符串/布尔值拥有方法,但都是不可变的.

javascript中的对象是可变的键控集合(keyed collections)

javascript中的对象是无类别的(class-free),对新属性的名字和值没有约束.

对象适合收集和管理数据,可包含其他对象,容易表示成树形或图形结构.

javascript包括一个原型链特性,允许对象继承.正确使用它能减少对象初始化的时间和内存消耗.

#3.1对象字面量

1.对象字面量就是包围在一对花括号中的 " 名/值 " 对.

2.对象字面量中，若属性名是一个合法的标示符且不是保留字，并不强制要求用引号括住属性名。故用“first-name”是必须的，但是否括住first_name则是可选的。

3.逗号用来分隔多个"名/值"对.

4.属性的值可以从包括另一个对象字面量在内的任意表达式中获得，对象是可嵌套的：
<pre class="brush:javascript">var flight = {

airline: "Oceanic",

number: 815,

departure: {

IATA: "SYD",

time: "2012-08-25 11:12",

city: "Guangzhou"

},

arrial: {

IATA: "LAX",

time: "2012-08-25 23:24",

city: "Nanjing"

}

};</pre>
&nbsp;

#3.2检索

1.采用在[ ]后缀中括住一个字符串表达式的方式。

2.优先考虑  . 表示法（更紧凑且可读性好）。
<pre class="brush:javascript">stooges["first-name"]

flight.departure.IATA</pre>
&nbsp;

3.检索一个并不存在的成员元素的值，则返回undefined值。

4.||运算符可用来填充默认值
<pre class="brush:javascript">var middle = stooges["middle-name"] || "(none)";

var status = flight.status || "unknown";</pre>
&nbsp;

5.检索一个undefined值会导致TypeError异常，但可用 &amp;&amp; 运算符来避免错误
<div>
<pre class="brush:other">flight.equipment //undefined

flight.equipment.model  //throw "TypeError"

flight.equipment &amp;&amp; flight.equipment.model  //undefined</pre>
&nbsp;

</div>
#3.3更新

对象中的值通过赋值语句来更新。

&nbsp;

#3.4引用

对象通过引用来传递，但永远不会被拷贝。

&nbsp;

&nbsp;

#3.5原型

1.所有通过对象字面量创建的对象都连接到Object.prototype这个对象。

2.javascript提供的实现机制复杂而杂乱，但其实可以被明显地简化 。

3.给Object增加一个beget方法，创建一个使用原对象作为其原型的新对象。

4.原型连接在更新时是不起作用的，当对某个对象做出改变时，不会触及该对象的原型。

5.原型连接只在检索值的时候才被用到。

6.若想要的属性完全不存在于原型链中，结果就是undefined值，这个过程称为【委托】

7.原型关系是动态的，若将新属性添加到原型中，该属性会立刻对所有基于该原型创建的对象可见。

&nbsp;

#3.6反射

1.typeof操作确定属性的类型：
<div>
<pre class="brush:other">typeof flight.number // ' number '

typeof flight.status // ' string '

typeof flight.arrival // ' object '

typeof flight.manifest // ' undefined '</pre>
&nbsp;

</div>
2.做反射的目标是数据，故应意识到一些值可能会是函数

3.hasOwnProperty方法：若对象拥有独有的属性，将返回true。该方法不会检查原型链。
<div>
<pre class="brush:other">flight.hasOwnProperty( 'number' )  //true

flight.hasOwnProperty('constructor') //false</pre>
&nbsp;

</div>
#3.7枚举

1.for in 语句用来遍历对象中的所有属性名。

2.for in 语句会枚举所有属性，包括原型中的属性

3.用hasOwnProperty方法做过滤器过滤掉不想要的值，用typeof来排除函数：
<pre class="brush:javascript">var name;

for (name in another_stooge) {

if (typeof another_stooge[name] !== 'function') {

document.writeln(name + ': ' + another_stooge[name]);

}

}</pre>
&nbsp;

4.属性名出线的顺序是不确定的，故要对任何可能出线的顺序有所准备。

5.要以特定顺序出线，则避免使用for in 语句，而是创建一个数组，在其中以正确的顺序包含属性名：
<pre class="brush:javascript">var i;

var properties = ['first-name','middle-name','last-name','profession'];

for (i=0;i&lt;properties.length; i +=1) {

document.writeln(properties[i] + ': ' +another_stooge[properties[i]]);

}</pre>
&nbsp;

#3.8删除

delete运算符用来删除对象的属性，移除对象中确定包含的属性，不会触及原型链中的任何对象。

删除对象的属性可能会让来自原型链中的属性浮现出来:
<pre class="brush:javascript">another_stooge.nickname //'nickname1'

//删除another_stooge中的nickname属性，从而暴露出原型的nickname属性

delete another_stooge.nickname;

another_stooge.nickname //'nickname0'</pre>
&nbsp;

#3.9减少全局变量污染

全局变量削弱的程序的灵活性，故应该避免。

1.最小化使用全局变量的一个方法是在应用中值创建唯一一个全局变量：
<pre class="brush:javascript">var MYAPP = {};</pre>
&nbsp;

该变量此时变成应用的容器，只要把多个全局变量都整理在一个名称空间下，将能降低一其他应用程序、组建或类库之间产生糟糕的相互影响。

&nbsp;

2.用闭包来进行信息隐藏的方式，是另一个减少全局污染的方法。

&nbsp;

&nbsp;

&nbsp;

#4

#4.1函数对象

1.对象是"名/值"对的集合并拥有一个连到原型对象的隐藏连接。

2.对象字面量产生的对象连接到Object.prototype,函数对象连接到Function.prototype.

3.每个函数在创建时附有两个隐藏属性：函数的上下文和实现函数行为的代码。

4.每个函数对象创建时也附带有一个prorotype属性，它的值是一个拥有construction属性并且值为该函数的对象。

5.注意函数是对象，拥有对象的一些性质如方法等。

6.函数的与众不同之处在于它们可以被调用。javascript在创建一个函数对象时，会设置一个“调用”属性。

&nbsp;

#4.2函数字面量

1.通过函数字面量来创建函数对象：
<pre class="brush:javascript">var add = function (a,b) {

return a + b;

};//创建一个名为add得到变量并把两个数字相加的函数赋值给它。</pre>
&nbsp;

2.通过函数字面量创建的函数对象包含一个连到外部上下文的连接，这称为闭包----javascript强大表现力的根基！

&nbsp;

#4.3调用

1.调用一个函数将暂停当前函数的执行，传递控制权和参数给新函数。

2.每个函数接受两个附加的参数：this 和 argument 。

3.this 的四种调用模式：方法调用模式、函数调用模式、构造器调用模式和 apply 调用模式。

4.当实参个数与形参个数不匹配时并不导致运行错误，而是忽略超出的实参或将缺失的实参替换为undefined。

3.1)方法调用模式（函数是对象的一个属性时，即称为方法）

1.当方法被调用时，this被绑定到该对象。

2.方法可以使用this去访问对象，故它能从对象中取值或修改该对象。

3.this到对象的绑定发生在调用的时候，使得函数可以对this高度复用。通过this可取得它们所属对象的上下文的方法称为【公共方法】。

3.2）函数调用模式（函数并非一个对象的属性时，当做一个函数来调用）

var sum = add (3,4);

1.当函数以此模式调用时，this被绑定到全局对象。（这是语言设计上的一个错误）

这个错误的后果是方法不能利用内部函数来帮助它工作。

解决方案：该方法定义一个变量并赋值为this，则内部函数可以通过那个变量访问到this。

&nbsp;

3.3）构造器调用模式

1.在一个函数前面带上new来调用，将创建一个隐藏连接到该函数的prototype成员的新对象，同时this将会绑定到那个新对象上。

2.new前缀也会改变return语句的行为

3.4）Apply调用模式

1.javascript是一门函数式的面向对象编程语言，故函数可以拥有方法。

2.apply方法让我们构建一个参数组并用其去调用函数，允许选择this的值。

3.apply方法接收两个参数，第一个是将被绑定给this的值，第二个是一个参数数组：

&nbsp;

#4.4参数

1.当函数被调用时，会得到一个附带参数，即argument数组。通过它函数可以访问所有被它调用时传递给它的参数列表，包括那么没有被分配给函数声明时定义的形式参数的多余参数。这使得可以编写一个无须指定参数个数的函数。

&nbsp;

2.arguments并不是一个真正的数组，只是一个类似数组（array-like）的对象，它拥有一个length的属性，但缺少所有的数组方法。

&nbsp;

#4.5返回

1.return语句可使函数提前返回，return被执行时函数立即返回而不执行余下的语句。

2.一个函数总会返回一个值，若没有指定返回的值，则返回undefined。

3.若函数以在前面加上new前缀的方式来调用，且返回值不是一个对象，则返回this（该新对象）。

&nbsp;

#4.6异常

&nbsp;

throw语句中断函数的执行，抛出一个exception对象，该对象包含可识别异常类型的name属性和一个描述性的message属性。

该exception对象将被传递到一个try语句的catch从句：

&nbsp;

try代码块中抛出异常，控制权就会跳转到catch从句。

未完待续。。。