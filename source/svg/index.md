title: 'SVG 学习笔记'
date: 2014-12-31 10:54:29
---
# SVG入门学习
    
    + stroke 描边
    + stroke-width 描边粗细
    + stroke-linecap 描边端点样式, 可为 butt, round, square, inherit
    + stroke-linejoin 描边转角样式, 可为 miter, round, bevel, inherit
    + stroke-miterlimit 描边相交的样式, 默认为4
    + stroke-dasharray 描边为虚线
    + stroke-dashoffset 虚线的起始偏移
    + stroke-opacity 表示描边透明度。默认是1
    + 除了 stroke 表示描边外, fill 表示填充

实现 SVG 路径的动画效果的原理:
    设置 stroke-dasharray 足够大, 比如 2000
    使用 CSS3 动画设置 stroke-dashoffset 从最大转化到最小, 比如从 2000 到 0 
    例子: http://jsfiddle.net/laiqs2011/3ysrtmn5/1/

用 JS 获取 path 的实际长度:

        var path = document.querySelector('.path');
        var length = path.getTotalLength();

快速入门后, 总结 SVG 原理就是:

- 画点成线 
- 连线成面
- 动态改变一些属性的值, 进而形成动画

难点: 一个面有无数条线, 一条线有无数个点(特别是曲线),怎么去画出优雅的点和线?

接下来就深入学习下原生的 `svg` 提供了哪些能力.

# SVG深入学习

- SVG 即可伸缩的矢量图形, 不随放大而失真

- Content-Type 为 "image/svg+xml"

- 默认单位为 px, 可为 em, ex, px, pt, pc, cm, mm, in

- svg 根元素的写法:

注意这里有两个命名空间要写对.

    <svg  xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">

- svg 嵌套

嵌套的 svg 定位将相对于父级 svg

    <svg xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink">
      <svg x="10">
        <rect x="10" y="10" height="100" width="100"
            style="stroke:#ff0000; fill: #0000ff"/>
      </svg>
      <svg x="200">
        <rect x="10" y="10" height="100" width="100"
            style="stroke:#009900; fill: #00cc00"/>
      </svg>
    </svg>

- g 标签, 即 group 组标签
    
上面的 svg 嵌套, 可以做整体偏移(move), 但不能做整体旋转(transform), 而把多个 svg 属性通过 `g` 标签组合到一起, 当作一个集合处理, 就可以做到:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        
        <g>
          <line x1="10" y1="10" x2="85" y2="10"
              style="stroke: #006600;"/>

          <rect x="10" y="20" height="50" width="75"
              style="stroke: #006600; fill: #006600"/>

          <text x="10" y="90" style="stroke: #660000; fill: #660000">
            Text grouped with shapes</text>
        </g>

    </svg>

接着我们只需要旋转 g 元素就可以实现组合中所有的元素旋转:
    
    g {
        transform: rotate(45deg);
    }


注意只能旋转 `g` 而不能旋转 `svg`

此外, `g` 中设置的属性, 将会被继承:
    
    <g style="stroke: #0000ff; stroke-width: 4px; fill: #ff0000">
        <rect    x="10"  y="10" width="100" height="50" />
        <circle cx="150" cy="35" r="25" />
        <circle cx="250" cy="35" r="25"
               style="stroke: #009900; fill: #00ff00;"/>
    </g>

需要注意的是, `g` 标签并没有 `x` 和 `y` 属性.

为了移动 `g` 可以使用 CSS3 的方法:
    
    g {
        transform: translateX(50%);
    }

或者再加一层 `svg` 标签将 `g` 包起来:

    <svg x="100">
        <g style="stroke: #0000ff; stroke-width: 4px; fill: #ff0000">
            <rect    x="10"  y="10" width="100" height="50" />
            <circle cx="150" cy="35" r="25" />
            <circle cx="250" cy="35" r="25"
                   style="stroke: #009900; fill: #00ff00;"/>
        </g>
    </svg>
    

- rect 标签

画一个矩形:
    
    <svg xmlns="http://www.w3.org/2000/svg"
         xmlns:xlink="http://www.w3.org/1999/xlink">
        <rect x="10" y="10" height="100" width="100"
            style="stroke:#006600; fill: #00cc00"/>
    </svg>

`x` 和 `y` 指定矩形左上角所在的位置.
`width` 和 `height` 指定矩形的宽高.
`style` 中的 `stroke` 指定描边颜色, `fill` 指定填充颜色.


画一个带圆角的矩形:

    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <rect x="10" y="10" height="50" width="50"
              rx="5" ry="5"
              style="stroke:#006600; fill: #00cc00"/>
        <rect x="70" y="10" height="50" width="50"
              rx="10" ry="10"
              style="stroke:#006600; fill: #00cc00"/>
        <rect x="130" y="10" height="50" width="50"
              rx="15" ry="15"
              style="stroke:#006600; fill: #00cc00"/>
    </svg>

`rx` 和 `ry` 指定水平和垂直方向的圆滑度.


使用 `stroke` 指定了描边颜色, 使用 `stroke-width` 指定描边粗细:

    <rect x="20" y="20" width="100" height="100"
          style="stroke: #009900;
                 stroke-width: 3;
                 fill: none;
          "
    />

使用 `fill` 指定了填充颜色, 使用 `fill-opacity` 指定填充透明度:

    <rect x="50" y="50" width="100" height="100"
          style="stroke: #000099;
             fill: #3333ff;
             fill-opacity: 0.5;
            "
    />

使用 `stroke-dasharray` 指定虚线间隙:
    
    <rect x="20" y="20" width="100" height="100"
          style="stroke: #009900;
                 stroke-width: 3;
                 stroke-dasharray: 10 5;
                 fill: none;
                "
    />    

其中 `10` 表示每个虚线条的长度, `5` 表示虚线条之间的间隔.


- circle 标签

画一个圆形:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <circle cx="40" cy="40" r="24" style="stroke:#006600; fill:#00cc00"/>
    </svg>

其中, `cx` 和 `cy` 指定圆心所在的位置, `r` 指定圆的半径.

同时. `circle` 也有 `fill`/`stroke`/ 等属性:
    
    <circle cx="40" cy="40" r="24"
        style="stroke:#006600;
               stroke-width: 3;
               stroke-dasharray: 10 5;
               fill:#00cc00;
               fill-opacity: 0.5;
        "
    />

- ellipse 标签

画一个椭圆形:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
      <ellipse cx="40" cy="40" rx="30" ry="15"
               style="stroke:#006600; fill:#00cc00"/>
    </svg>

其中, `cx` 和 `cy` 指定圆心, `rx` 和 `ry` 指定长短半径.

同时. `ellipse` 也有 `fill`/`stroke`/ 等属性. 

- line 标签

画几条线段:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <line x1="0"  y1="10" x2="0"   y2="100" style="stroke:#006600;"/>
        <line x1="10" y1="10" x2="100" y2="100" style="stroke:#006600;"/>
        <line x1="20" y1="10" x2="100" y2="50"  style="stroke:#006600;"/>
        <line x1="30" y1="10" x2="110" y2="10"  style="stroke:#006600;"/>
    </svg>

其中, `x1` 和 `y1` 指定起点所在位置, `x2` 和 `y2` 指定终点所在位置, `stroke` 指定线条颜色.

- polyline 标签

画多个点, 然后连成线, 线连成面:

画三个点, 连成三角形:

    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <polyline points="0,0  30,0  15,30"
            style="stroke:#006600;"/>
    </svg>

默认的 `fill` 填充颜色为黑色, 重新修改填充和描边:

    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <polyline points="0,0  30,0  15,30"
            style="stroke:#006600; stroke-width: 2;
                   fill: #33cc33;"/>
    </svg>

注意, 第一个点(10,2)与第二个点(30,0)连线, 第二个点与第三个点(15,30)连线了, 但第三个点并未与第一个点连线, 所以正确闭合图形的方法是, 起点和终点坐标一致:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <polyline points="0,0  30,0  15,30  0,0"
            style="stroke:#006600; stroke-width: 2;
                   fill: #33cc33;"/>
    </svg>


- polygon 标签

画一个多边形.

画一个三角形:

    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
      <polygon points="10,0  60,0  35,50"
             style="stroke:#660000; fill:#cc3333;"/>
    </svg>

注意这里用 `polygon` 后, 使用三个坐标点就画出了一个三角形, 而使用`polyline` 则需要四个坐标点.

画一个八边形:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
      <polygon points="50,5   100,5  125,30  125,80 100,105
                       50,105  25,80  25, 30"
              style="stroke:#660000; fill:#cc3333; stroke-width: 3;"/>
    </svg>


- path 标签

画路径:
    
    <svg xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink">
        <path d="M50,50
                 A30,30 0 0,1 35,20
                 L100,100
                 M110,110
                 L100,0"
              style="stroke:#660000; fill:none;"/>    
    </svg>

其中,`d` 表示 draw 指定绘画属性, `M` 表示 move 即移动, 'A' 表示 Arc 即画一个弧线, `L` 表示  line 即画一条线.

这里使用了两个 `M`, 第二个 `M` 指定了新起点开始画线, 所以两个线条并不连续.

注意这里使用到指令字母都是大写(M,A,L), 表示使用绝对坐标; 若使用小写(m,a,l)则表示使用相对坐标.举个例子:

 `L` 与 `l` 都是连线的指令, 但大写的 `L` 指定绝对坐标, 小写的 `l` 指定相对坐标. 举个例子, 若画线的起点为 (50, 50):

    'L100,100' 指定连线 (50,50) 与 (100,100)
    'l100,100' 指定连线 (50,50) 与 (150,150)

`A` 表示 Arc 画一个弧线:
    
    <path d="M40,20  A30,30 0 0,0 70,70"
        style="stroke: #cccc00; stroke-width:2; fill:none;"/>

`Q` 表示 Quadratic 画一个二次方程曲线:

    <path d="M50,50 Q50,100 100,100" 
          style="stroke: #006666; fill:none;"/> 

`C` 表示 Cubic 画一个三次方程曲线:
    
    <path d="M50,50 C75,80 125,20 150,50"
          style="stroke: #006666; fill:none;"/> 


画完后, 若想闭合路径, 可以使用 `Z` 指令:
    
    <path d="M50,50 L100,50 L100,100 Z"
        style="stroke: #006666; fill:none;"/>


- maker 标签

**未完待续...** http://tutorials.jenkov.com/svg/marker-element.html

## 总结

## 发现

SVG 介绍 
http://jakearchibald.com/2013/animated-line-drawing-svg/
http://css-tricks.com/svg-line-animation-works/

SVG 教程
https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial
http://tutorials.jenkov.com/svg/index.html

SVG 在线生成工具 
http://svg-edit.googlecode.com/svn/trunk/editor/svg-editor.html

SVG 库
http://raphaeljs.com/
http://www.svgjs.com/
http://snapsvg.io/

云端编程, 超赞!
https://c9.io/

## TODO 
翻译 http://24ways.org/2013/animating-vectors-with-svg/
使用 D3 来操作 SVG