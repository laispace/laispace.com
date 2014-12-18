title: CSS最佳实践
categories:
  - CSS
tags:
  - 最佳实践
date: 2013-08-13 17:33:15
---

> 使用最短最优最语义化的css代码对提升工作效率帮助非常大，小赖对常见的好方法总结在这里。（最后更新于 2014/08/13）

- 使用 [normalize.css](http://necolas.github.io/normalize.css/) 而不是 [reset.css](http://meyerweb.com/eric/tools/css/reset/) 

    后者清零了所有浏览器的样式，而前者则是统一设置了所有浏览器的样式，省去不少重写样式的时间。

- 使用clearfix来清除浮动，减少不必要的 html 标签：
```
/* 现代浏览器 */
.clearfix:before,
.clearfix:after {
     content: '';
     display: table;
}
.clearfix:after {
     clear: both;
}
/* IE6/7 触发hasLayout */
.clearfix {
     zoom: 1;
}     
```

- 如果不想使用 clearfix 来清除浮动，可用 overflow 来清除：
```
.container {
     overflow: auto; /* 清除浮动 */
     zoom: 1;      /* IE触发hasLayout */
     display: block;      /* 保证容器是块元素 */
}
```

- 使用 hr 元素加上样式来做分隔线，更加语义化：
```
    <hr class="divider">
```
```
.divider {
     border-top: 1px solid #eee;
     clear: both;
}
```

- text-indent 隐藏文字不要设定为类似 999999em 这么大，以提高移动设备上的性能：
```
.hide-text {
     text-indent: 100%;
     white-space: nowrap;
     overflow: hidden;
}
```

- 使用 box-sizing 属性解决盒模型问题

既然 IE8+ 支持[这个属性](http://caniuse.com/#search=box-sizing)，那我们就大胆的用吧，设置后元素就不会因为被设定的内边距或者边框而挤爆容器了。

```
* {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
          -ms-box-sizing: border-box;
             -o-box-sizing: border-box;
                  box-sizing: border-box;
}
```


