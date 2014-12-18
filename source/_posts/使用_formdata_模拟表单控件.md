title: 使用 FormData 模拟表单控件

date: 2014-05-09 21:19:42

categories: HTML

tags: [FormData] 

---


- 创建一个新的 FormData 对象，然后使用 append() 添加字段：

		var form = new FormData();
		form.append('name', 'xiaolai');
		form.append('age', 18);
		// 使用 ajax 发送
		var req = new XMLHttpRequest();
		req.open('POST', 'http://laispace.com/test');
		req.send(form);

	FormData.append(key, value)中 value 可以是string/Blob对象/File对象
	
- 利用已有的 form 创建 FormData 对象进行格式化

		var myForm = document.getElementById('myForm');
		var form = new FormData(myForm);

	继续使用 FormData.append() 添加字段，或使用 ajax 发送表单
	
- 异步上传文件

	若 myForm 中有用户选择的文件，要进行异步上传：
	
		var req = new XMLHttpRequest();
		req.open('POST', 'http://laispace.com/test', true);
		req.onload = function (e) {
			if (req.status === 200) {
				console.log('文件已成功上传！')
			} else {
				console.log('啊噢~ 文件上传失败！')
			}
		}	
		
<!-- more -->


	
	
	
	
[参考资料](https://developer.mozilla.org/zh-CN/docs/DOM/XMLHttpRequest/FormData/Using_FormData_Objects)

