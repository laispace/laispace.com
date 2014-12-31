title: 'SVG 学习笔记'
date: 2014-12-31 10:54:29
---
# 首先先了解几个属性

    stroke 描边
    stroke-width 描边粗细
    stroke-linecap 描边端点样式, 可为 butt, round, square, inherit
    stroke-linejoin 描边转角样式, 可为 miter, round, bevel, inherit
    stroke-miterlimit 描边相交的样式, 默认为4
    stroke-dasharray 描边为虚线
    stroke-dashoffset 虚线的起始偏移
    stroke-opacity 表示描边透明度。默认是1


# 实现 SVG 路径的动画效果的原理:

    设置 stroke-dasharray 足够大, 比如 2000
    使用 CSS3 动画设置 stroke-dashoffset 从最大转化到最小, 比如从 2000 到 0 
    例子: http://jsfiddle.net/laiqs2011/3ysrtmn5/1/

用 JS 获取 path 的实际长度:

        var path = document.querySelector('.path');
        var length = path.getTotalLength();

### 参考资料

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

## TODO 
翻译 http://24ways.org/2013/animating-vectors-with-svg/