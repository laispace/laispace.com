title: CSS属性扫盲笔记
categories:
  - CSS
date: 2014-08-13 17:40:49
tags: css3
---

- :before 和 ::before 的区别

单冒号表示 CSS3 伪类，双冒号表示 CSS3 伪元素
双冒号是 CSS3 新引入的属性, 而要兼容 IE8- 则需要使用单冒号
不需要兼容 IE8- 则可以放心的使用双冒号



- -webkit-appearance 设置如何显示元素的外观

http://ued.ctrip.com/webkitcss/demo/appearance.html


- -webkit-touch-callout 设置如何显示一个可触摸目标的样式

http://css-infos.net/property/-webkit-touch-callout

- -webkit-user-select 设置是否可以选择元素内容

http://ued.ctrip.com/blog/wp-content/webkitcss/prop/user-select.html

- -webkit-user-drag 设置是否可以拖动元素内容

http://ued.ctrip.com/blog/wp-content/webkitcss/prop/user-drag.html

- -webkit-flex 设置伸缩布局

http://ued.ctrip.com/blog/wp-content/webkitcss/prop/flex.html

安卓 4.4+ 才支持, 伤不起啊...

- -webkit-tap-highlight-color 设置元素的点击高亮颜色

http://ued.ctrip.com/webkitcss/prop/tap-highlight-color.html 
    
    /* 设置为透明, 则禁用该属性 */
    -webkit-tap-highlight-color: transparent;

    /* 场景: callout 和 hightlite 配合使用*/
    .nohighlight {
      -webkit-touch-callout: none;
      -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    }


- 以下属性与 display: -webkit-box; 配合使用

-webkit-box-sizing 设置对象的盒模型组成模式
        
        .selector {
            width: 100px;
            margin: 10px;
            padding: 10px;
            border: 1px solid #eee;
            // 设置为 border-box, 则 width 包含了 padding 和 border 
            -webkit-box-sizing: border-box;
        }

-webkit-box-flex 设置弹性盒模型对象的子元素如何分配*剩余*空间
    
    .selector-parent {
        width: 150px;
        display: -webkit-box;
    }
    .selector-child-fixed {
        width: 50px;
    }
    .selector-child-flex-1 {
        /* 占 40px */
        -webkit-box-flex: 2;
    }
    .selector-child-flex-2 {
        /*占 60px */
        -webkit-box-flex: 3;
    }


- -webkit-box-orient 设置弹性盒模型对象的子元素的排列方式

http://ued.ctrip.com/blog/wp-content/webkitcss/prop/box-orient.html

- -webkit-box-pack 设置弹性盒模型对象的子元素的对齐方式

http://ued.ctrip.com/blog/wp-content/webkitcss/prop/box-pack.html

- -webkit-box-align 设置弹性盒模型对象的子元素的对齐方式

http://ued.ctrip.com/blog/wp-content/webkitcss/prop/box-align.html

- -webkit-line-clamp 设置块元素显示文本的行数

http://www.css88.com/webkit/-webkit-line-clamp/

    .text-overflow-ellipsis {
        // 显示一行
        -webkit-line-clamp: 1;
        // 溢出隐藏
        text-overflow: ellipsis;
        overflow: hidden;
        // 和模型的子元素垂直排列
        display: -webkit-box;
        -webkit-box-orient: vertical;
       
    }

-  ::-webkit-input-placeholder 设置占位文字的样式  
http://css-tricks.com/snippets/css/style-placeholder-text/ 
    
    .selector::-webkit-input-placeholder {
        color: #eee;
    }


### 参考资料

- [携程CSS3-webkit-私有属性列表](http://ued.ctrip.com/blog/wp-content/webkitcss/index.html)