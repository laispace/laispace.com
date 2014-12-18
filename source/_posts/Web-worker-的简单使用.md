title: Web worker 的简单使用
categories:
  - HTML5
date: 2014-11-29 14:06:34
tags: worker
---

# Main.js

   注意在 chrome 下  worker 不能正常加载，需要在服务器环境下
   
        var worker = new Worker('Worker.js');

        // 给 worker 发送消息
        worker.postMessage('Hi, I am a message from Main');

        // 监听来自 worker 的消息
        worker.onmessage  = function (e) {
            console.log(e);
            console.log('Main had receive a message: ', e.data);

            // 接收 worker 传回的指令
            if (e.data.command) {
                eval('(' + e.data.command + ')')
            }
        };
        
        // 中止 worker
        // 立即杀死 worker
        // worker.terminate();
        
# Worker.js

	    console.log('Worker: I am working!');

        // 发送消息给主线程
        postMessage('Hi, I am a message from Worker!');

        // 监听主线程传来的消息
        onmessage = function (e) {
            console.log(e);
            console.log('Worker had receive a message: ', e.data);
        };


        // 注意： 通常来说，后台线程 – 包括 worker – 无法操作 DOM。
        // 如果后台线程需要修改 DOM，那么它应该将消息发送给它的创建者，让创建者来完成这些操作。
        // document.write('I will not be executed :(');  => 报错：alert is not defined


        //  所以为了操作 DOM, 可以通过传递命令的方式来让主线程来执行
        postMessage({
            command: "document.write('hello I am from Worker')"
        });    
        
        // 除了在 Main.js 杀死线程，也可以自己杀死自己
        // self.close();  