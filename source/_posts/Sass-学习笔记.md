title: Sass 学习笔记
categories:
  - Node
date: 2014-08-13 17:40:49
tags:
---

# 安装 Sass

```
$ gem install sass
// 或
$ sudo gem install sass 

// 查看 sass 版本
$ sass -v  
```

# 快速入门 sass 语法

## Variables | 变量
```
// test.scss
$lai-font: Roboto, sans-serif;
$lai-color: #eee;

body {
  color: $lai-color;
  font-family: $lai-font;
}
```

## Nesting | 嵌套
```
nav {
  ul {
    margin: 0 auto;
    padding: 0;
    list-style: 0;
  }

  li {
    display: inline-block;
  }

  a {
    display: block;
    padding: 5px 10px;
    text-decoration: none;
  }
}
```

## Partials | 模板
```
// _reset.scss
html,
body,
ul,
ol {
  margin: 0;
  padding: 0;

}
// 使用 partial
// base.scss
@import 'reset'
body {
  backgrount: #333;
}
```


## Mixins | 混入
```
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}
// 使用 mixin
.box {
  @include border-radius(10px);
}
```

## Inheritance | 继承
```
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}
.success {
  @extend .message;
  border-color: green;
}
.error {
  @extend .message;
  border-color: red;
}
.warning {
  @extend .message;
  border-color: yellow;
}
```

## Operators | 运算符
.container {
  width: 100%;
}
article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}
article[role="sub"] {
  float: right;
  width: 300px / 960px * 100%;
}

# 编译 .scss 为 .css
  
nested：嵌套缩进的css代码，它是默认值。
　   　 
expanded：没有缩进的、扩展的css代码。
　   　 
compact：简洁格式的css代码。
　　    
compressed：压缩后的css代码。
            
```
// 编译风格默认为 --style nested
$ sass test.scss test.css
// 编译风格设置为 --style compressed
$ sass --style compassed test.scss test.css

// 查看编译后的 test.css    
$ cat test.css
```

# 监听文件变化

一旦某个文件/目录发生变化，Sass 就自动编译出新的版本

```
// 监听文件
$ sass --watch test.scss:test.css
// 监听目录，一旦 src/scss 下有文件发生变化，就编译到 dist/css 目录
$ sass --watch src/scss:dist/css
```


### 参考链接

- [Sass 官网](http://sass-lang.com/guide)
- [Sass 文档](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)