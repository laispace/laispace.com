title: 'JavaScript 碎片'
categories: Javascript
tags:
  - javascript
date: 2014-12-18 14:29:22
---

- Object.defineProperty

		var A = {};
        Object.defineProperty(A, 'attrName', {
            set: function(val) { 
                this.__attrName__ = val; 
                console.log('A.attrName 被设置为: ', val);
            },
            get: function() { 
                console.log('A.attrName 被获取到: ', this.__attrName__);
                return this.__attrName__; }
            });
        });    
