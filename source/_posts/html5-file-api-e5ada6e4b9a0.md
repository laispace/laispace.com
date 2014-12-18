title: 'HTML5-file API 学习'
tags:
  - fileupload
id: 529
categories:
  - HTML
date: 2013-11-19 15:42:50
---

为实现图片上传前预览并限制图片大小，貌似只能通过flash和HTML5的API来解决了吧？

今天学习了HTML5的file API，将预览和限制大小的功能应用到了项目里。

练习代码先贴到这，方便自己查阅，日后比较空闲时再回来好好研究那些API的机制吧：）

[html]

&lt;p&gt;请选择图片(可多选)：&lt;/p&gt;
 &lt;input id=&quot;file_input&quot; type=&quot;file&quot; multiple style=&quot;display:none;&quot; onchange=&quot;handleFiles(this.files)&quot;&gt;
 &lt;button id=&quot;select-btn&quot;&gt;请选择图片&lt;/button&gt;
 &lt;div id=&quot;dropbox&quot; style=&quot;width:300px;height:200px;background:#eee;&quot;&gt;
 或拖动图片到此处
 &lt;/div&gt;
 &lt;ol id=&quot;preview-img-list&quot;&gt;

&lt;/ol&gt;
 &lt;button id=&quot;send-btn&quot;&gt;发送！&lt;/button&gt;

[/html]

[javascript]

// 获取文件
 var file_input = document.getElementById('file_input');
 var select_btn = document.getElementById('select-btn');
 var preview_img_list = document.getElementById('preview-img-list');
 var dropbox = document.getElementById('dropbox');
 var send_btn = document.getElementById('send-btn');

// 选择文件
 select_btn.onclick = function(){
 file_input.click();
 };

send_btn.onclick = function(){
 sendFiles();
 };

// 监听文件改变
 file_input.addEventListener('change', handleFiles, false);
 // 监听拖拽事件
 dropbox.addEventListener('dragenter', dragenter, false);
 dropbox.addEventListener('dragover', dragover, false);
 dropbox.addEventListener('drop', drop, false);

// 接受并预览图片
 function handleFiles(files){
 var len = files.length;
 var imageType = /image.*/;
 window.URL = window.URL || window.webkitURL;
 if (len) {
 preview_img_list.innerHTML = '';
 for (var i = 0; i &lt; len; i++) {
 if( ! files[i].type.match(imageType)) {
 // 不是图片 跳过
 continue;
 }

var li = document.createElement('li');
 var img = document.createElement('img');
 var p = document.createElement('p');

p.innerHTML = 'name: ' + files[i].name + &quot; &lt;br /&gt;size: &quot; + files[i].size + ' Bytes' + &quot;&lt;br/&gt;type: &quot; + files[i].type;

// 将file对象存在当前图片中,用于后续创建上传任务
 img.file = files[i];
 // 添加一个类，方便选择
 img.classList.add('obj');
 /*方法一：使用URL对象预览图片*/
 // 使用window.URL.createObjectURL创建 blob URL
 img.src = img.src = window.URL.createObjectURL(files[i]);
 img.onload = function(e) {
 // 使用window.URL.revokeObjectURL释放URL对象，因为图片加载完成后不再需要这个对象
 window.URL.revokeObjectURL(this.src);
 }
 /*方法一结束*/

/*方法二：使用FileReader对象预览图片 */
 // var reader = new FileReader();
 // reader.onload = (function(aImg) {
 // return function(e) {
 // aImg.src = e.target.result;
 // };
 // })(img);
 // reader.readAsDataURL(files[i]);
 /*方法二结束 */

li.appendChild(img);
 preview_img_list.appendChild(li);

};
 }
 }

// 创建上传任务
 function sendFiles() {
 var imgs = document.querySelectorAll('.obj');
 var len = imgs.length;
 for (var i = 0; i &lt; len; i++) {
 // 第二个参数用于读取图片数据
 new FileUpLoad(imgs[i], imgs[i].file);
 };
 }

function FileUpLoad(img, file) {
 var reader = new FileReader();
 // 创建一个 throbber 用于显示进度信息
 this.ctrl = createThrobber(img);
 // 创建一个XMLHttpRequest对象用来上传数据
 var xhr = new XMLHttpRequest();
 this.xhr = xhr;

var self = this;
 // 监听数据上传，更新throbber
 this.xhr.upload.addEventListener('progress', function(e){
 if (e.lengthComputable) {
 var percentage = Math.round((e.loaded * 100) / e.total);
 self.ctrl.update(percentage);
 }
 }, false);
 // 上传完成，更新进度到100%，移除throbber 因为不再需要
 xhr.upload.addEventListener('load', function(e) {
 self.ctrl.update(100);
 var canvas = self.ctrl.ctx.canvas;
 canvas.parentNode.removeChild(canvas);
 }, false);
 // 使用POST方式发送数据
 xhr.open(&quot;POST&quot;, &quot;http://demos.hacks.mozilla.org/paul/demos/resources/webservices/devnull.php&quot;);
 // 使用一个通用的MIME类型
 xhr.overrideMimeType('text/plain; charset=x-user-defined-binary');
 reader.onload = function(evt) {
 // 以二进制形式发送
 xhr.sendAsBinary(evt.target.result);
 };
 // 将文件转化为二进制字符串形式
 reader.readAsBinaryString(file);
 }

&amp;nbsp;

function dragenter(e) {
 e.stopPropagation();
 e.preventDefault();
 }
 function dragover(e) {
 e.stopPropagation();
 e.preventDefault();
 }
 function drop(e) {
 e.stopPropagation();
 e.preventDefault();

var dt = e.dataTransfer;
 var files = dt.files;

handleFiles(files);
 }

[/javascript]

&nbsp;

这个File API 使得浏览器支持预览、图片大小/类型限制、拖拽上传、多图上传。

&nbsp;