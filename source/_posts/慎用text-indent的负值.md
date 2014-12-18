title: 慎用text-indent的负值
categories:
  - CSS
date: 2014-09-25 15:15:02
tags:

---

为了语义化，我们可能会利用图片替换文字的方式来给我们的站点增色，举个栗子：
            
            <p>文字文字</p>

            p {
                text-indent: -2500px;        // 小，在高分辨率宽屏下文字隐藏失败
                //text-indent: -99999px;     // 大，但可能存在性能问题，甚至被搜索引擎屏蔽
                background: url(logo.png);
            }
            
  这段代码中我们希望隐藏文字，提升 SEO，所以使用 logo.png 这个图片进行替换，这时会对文字设置一个负缩进值。
  
  这里的 -2500px 在以前基本可以解决隐藏文字的问题，但目前发现高分辨率浏览器下这个值已经在浏览器可视范围内了，造成文字隐藏失败的问题。
  
  而如果将这个值设置为更大，如 -99999px 时，又会造成浏览器的性能问题：浏览器需要生成一个宽度为 99999px 的盒模型，所以也要限制这个值的大小。
  
  还有人指出，不少人滥用这个属性为了提升 SEO ，而搜索引擎可能会反过来屏蔽这里的文字。
  
  除此之外，在从右到左读的语言环境中，这个负值可能会造成很长的横向滚动条，所以可以添加 direction 规则来避免：

             p {
                text-indent: -9999px; // 万一日后用户屏幕宽度达到1万肿么办？（这好像不可能。。。）
                background: url(logo.png);
                direction: ltr; // 设置为从左到右读的方向，避免 rtl 语言环境下出现横向滚动条
            }
                        
 一个比较好的可选方案：

            p { 
                text-indent: 100%; 
                white-space: nowrap; 
                overflow: hidden; 
                background: url(logo.png);
            }

 参考链接：
 
 - [Disallow negative text indent](https://github.com/CSSLint/csslint/wiki/disallow-negative-text-indent)
 - [Stop Using the text-indent:-9999px CSS Trick](http://luigimontanez.com/2010/stop-using-text-indent-css-trick/)
 - [CSS Image Replacement](http://css-tricks.com/examples/ImageReplacement/)