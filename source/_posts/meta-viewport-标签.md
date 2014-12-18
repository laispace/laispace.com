title: meta viewport 标签
categories:
  - HTML
date: 2014-07-25 17:05:42
tags:
  - meta
  - viewport
  
---

viewport 可以控制页面的原始宽度，限制用户的缩放行为。

```
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
```

width: 控制 viewport 的宽度，可指定为数字如 800，或特殊值如 device-width，即设置为 100%。

height: 控制 viewport 的高度。

initial-scale: 页面第一次加载时的缩放比例。

maximum-scale: 最大缩放比例，取值从 0 到 10。

minimum-scale: 最小缩放比例，取值从 0 到 10。

user-scaleble: 是否允许用户缩放，取值为 yes/true 或 no/false。


<!-- more -->

### 参考链接

- [Using the viewport meta tag to control layout on mobile browsers](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag)