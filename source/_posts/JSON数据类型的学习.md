title: JSON数据类型的学习
id: 349
categories:
  - Javascript
date: 2013-03-28 02:00:41
tags:
  - json
---

<div>

初学AJAX，今天写一个表单验证，发现$.ajax()的success回调函数总是不会执行，而error毁掉函数则总是执行。

我是先POST表单账号密码给php文件，该php文件返回数据（我直接返回了常量供自己测试），然后在JS里获得data.login_email。

发现data.login_email得到的总是undefined，为什么呢？由于对JSON格式不是很熟悉，所以不断调试不断查资料，最后发现：

**原来自1.4版本的jQuery开始,JSON写的不规范可能导致错误!**

"json": Evaluates the response as JSON and returns a JavaScript object. In jQuery 1.4 the JSON data is parsed in a strict manner; any malformed JSON is rejected and a parse error is thrown. (See[ json.org](http://www.json.org) for more information on proper JSON formatting.

查资料学习了JSON格式后，修改后台php代码为：

[php]

&lt;?php

header('Content-type: text/json');

  $results = array (

    &quot;login_email&quot; =&gt; &quot;0&quot;,

    &quot;login_password&quot; =&gt;&quot; 0&quot;

);

echo json_encode($results);

?&gt;

[/php]
<div>再修改前台代码则为：</div>
<div>[javascript]

$('#login-btn').click(function(){

var emailVal = $('#login-email').val();

  var passwordVal = $('#login-password').val();

//点击登陆按钮时进行验证

  $.ajax({

   type: &quot;POST&quot;,

   data:{&quot;emailVal&quot;:emailVal,&quot;passwordVal&quot;:passwordVal},

   dataType: &quot;JSON&quot;,//原来声明了json数据类型就必须严格书写JSON！

   url: &quot;login_check.php&quot;,

success: function(data){

if(data.login_email==0){//邮箱错误

$('#login #login-email').prev().show();

$('#login #login-email').parent().parent().addClass('warning');

}

                if(data.login_password==0){//密码错误

                    $('#login #login-password').prev().show();

$('#login #login-password').parent().parent().addClass('error');

}

  },error:function(XMLResponse){

            alert(&quot;出错！错误信息为&quot;+XMLResponse.responseText)}

            });

return false;//禁止登陆按钮的默认行为

});//click（）结束

[/javascript]

</div>
<div></div>
<div>总结出一点JSON规范为：</div>
<div>　　1）名称：用双引号引起，如[javascript]

    data:{&quot;emailVal&quot;:emailVal,&quot;passwordVal&quot;:passwordVal}

[/javascript]

</div>
<div>　　2）字符串：用用双引号引起，如下：</div>
<div>[php]

&lt;?php

  header('Content-type: text/json');

  $results=array(&quot;a&quot; =&gt; &quot;apple&quot;, &quot;b&quot; =&gt; &quot;banana&quot;, &quot;o&quot; =&gt; &quot;orange&quot;);

  echo json_encode($results);

?&gt;

[/php]

</div>
<div>        3）boolean类型不加引号，其他都要加（包括数字）</div>
<div>        4）前后台使用的数据类型应该一致，如后台使用json_encode($results);前台使用dataType:JSON;来声明</div>
</div>
<div>        5）若定义了dataType为JSON就必须使用严格语法的JSON，否则success回调函数就不执行！</div>
<div></div>
#-- 2013-10-05 更新---
注意JSON字符串与JSON对象的区分：

[javascript]
// 这是JSON字符串
var foo = '{ &quot;prop&quot;: &quot;val&quot; }';
// 这是对象字面量
var bar = { &quot;prop&quot;: &quot;val&quot; };
[/javascript]

JSON有非常严格的语法，在string上下文里{ "prop": "val" } 是个合法的JSON，但{ prop: "val" }和{ 'prop': 'val' }确实不合法的。所有属性名称和它的值都必须用双引号引住，不能使用单引号。另外，即便你用了转义以后的单引号也是不合法的。
参考：[根本没有"JSON"对象这回事](http://www.cnblogs.com/TomXu/archive/2012/01/11/2311956.html)