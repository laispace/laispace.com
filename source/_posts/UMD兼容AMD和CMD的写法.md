title: "UMD兼容AMD和CMD的写法"
category: Javascript
date: 2014-12-22 20:38:13
tags: 
---

UMD 规范 http://davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/

同时支持 AMD 和 CMD, 做一层封装即可

这里使用 jquery 和 underscore 做例子:

    (function (root, factory) {
        if (typeof define === 'function' && define.amd) {
            // AMD 格式
            define(['jquery', 'underscore'], factory);
        } else if (typeof exports === 'object') {
            // CMD 格式
            module.exports = factory(require('jquery'), require('underscore'));
        } else {
            挂载到浏览器 window 下
            root.returnExports = factory(root.jQuery, root._);
        }
    }(this, function ($, _) {
        //    定义几个方法
        function a{};    //    私有方法, 不暴露出去
        function b{};    //    公有方法
        function c{};    //    公有方法

        // 将公有方法暴露出去
        return {
            b: b,
            c: c
        }
    }));
