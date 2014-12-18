title: NODE入门-简单爬虫
tags:
  - spider
categories:
  - Node
date: 2014-02-26 14:26:13

---

nodejs真是太酷了！

使用 http.get() 抓取页面，然后使用 jsdom 来解析页面（简单配置+jquery语法），简单代码如下：

    var http = require('http');
    // 使用 jsdom 来解析html https://www.npmjs.org/package/jsdom
    var jsdom = require('jsdom');
    // 抓取 V2EX 最新话题
    var url = 'http://www.v2ex.com';

    // 获取一个页面
    http.get(url, function(res) {
        var body = '';
        console.log('状态码：', res.statusCode);
        res.on('data', function(chunk) {
            console.log('数据传输中...');
            body += chunk;
        });
        res.on('end', function() {
            console.log('数据传输完成:');
            // 使用 jsdom 解析抓取到的html
            jsdom.env(
                body,
                'http://code.jquery.com/jquery.js',
                function(errors, window) {
                    var $ = window.$;
                    console.log('数据传输完成');
                    var len = $('.cell.item').length;
                    console.log('找到最新主题', len);
                    for (var i = 0; i & lt; len; i++) {
                        var title = $('.cell.item').eq(i).find('.item_title a').html();
                        var link = url + $('.cell.item').eq(i).find('.item_title a').attr('href');
                        console.log(title + '(' + link + ')');
                    }
                }
            );
        });
    }).on('error', function(e) {
        console.log( & quot; 发生错误: & quot; + e.message);
    });

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/02/Screen-Shot-2014-02-27-at-10.29.44-AM.png)](简单小爬虫)

<!-- more -->

另一个例子，抓取知乎的一个问答：

    var http = require('http');
    // 使用 jsdom 来解析html https://www.npmjs.org/package/jsdom
    var jsdom = require('jsdom');
    // 抓取 知乎话题『你认为自己最好的照片是哪张？』里的图片
    var url = 'http://www.zhihu.com/question/20937691';
    // 获取一个页面
    http.get(url, function(res) {
            var body = '';
            console.log('状态码：', res.statusCode);
            console.log('数据传输中...');
            res.on('data', function(chunk) {
                body += chunk;
            });
            res.on('end', function() {
                    console.log('数据传输完成:');
                    // 使用 jsdom 解析抓取到的html
                    jsdom.env(
                        body, [ & quot;http: //code.jquery.com/jquery.js&quot;],
                            function(errors, window) {
                                var $ = window.$;
                                console.log('数据传输完成');
                                // 知乎问题
                                var question = $('title').text();
                                console.log(question);
                                // 知乎回答
                                var len = $('#zh-question-answer-wrap').find('.zm-item-answer ').length;
                                console.log('共有', len, '个回答');
                                for (var i = 0; i & lt; len; i++) {
                                    var author = $('.zm-item-answer').eq(i).find('.zm-item-answer-author-wrap a:nth-child(2)').text();
                                    var author_link = 'http://www.zhihu.com' + $('.zm-item-answer').eq(i).find('.zm-item-answer-author-wrap a:nth-child(2)').attr('href');
                                    var avatar = $('.zm-item-answer').eq(i).find('.zm-list-avatar').attr('src');
                                    var vote_count = $('.zm-item-answer').eq(i).find('.zm-item-vote-count').text();
                                    author = author ? author : '匿名用户';
                                    // 输出每个答案下的 作者 票数等信息
                                    console.log('第' + i + '位', vote_count + '票', '作者:' + author);
                                    console.log('(主页:' + author_link + '头像:' + avatar + ')')
                                    var imgs = $('.zm-item-answer').eq(i).find('.zm-item-rich-text img');
                                    var imgLen = imgs.length;
                                    for (var j = 0; j & lt; imgLen; j++) {
                                        var imgSrc = imgs.eq(j).attr('src');
                                        if (imgSrc) {
                                            console.log('图片' + j + ': ' + imgSrc);
                                        }
                                    }
                                    console.log('\n')
                                }
                            }
                        );
                    });
            }).on('error', function(e) {
            console.log( & quot; 发生错误: & quot; + e.message);
        });

[![](http://xiaolai-wordpress.stor.sinaapp.com/uploads/2014/02/Screen-Shot-2014-02-27-at-1.48.00-PM.png ")](简单爬虫)

此外，可以试试用简化的 request 模块代替 http.get()：

    // $ npm install request (https://www.npmjs.org/package/request)
    var request = require('request');
    request('http://laispace.github.io', function (error, response, body) {
         if (!error && response.statusCode == 200) {
             console.log(body)
        }
    });

接下来就只要把解析出的数据存入自己的数据库，就可以拿来用了。

