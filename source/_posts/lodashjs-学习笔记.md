title: lodash.js 学习笔记
tags:
  - lodash
categories: []
date: 2014-07-13 10:28:00
---
underscore.js 提供了一系列工具函数，而 lodash.js 可以认为是 underscore.js 的一个超集。

简单示例：

```
var _ = require('lodash');

// 去掉falsy值后的数组
_.compact([0, 1, false, 2, '', 3]);
// → [1, 2, 3]

// 找出数组中不同的值
_.difference([1, 2, 3, 4, 5], [5, 2, 10]);
// → [1, 3, 4]

// 根据条件找出数组元素的索引值，未找到则返回 -1
var characters = [
  { 'name': 'barney',  'age': 36, 'blocked': false },
  { 'name': 'fred',    'age': 40, 'blocked': true },
  { 'name': 'pebbles', 'age': 1,  'blocked': false }
];
_.findIndex(characters, function(chr) {
  return chr.age < 20;
});
// → 2
// using "_.where" callback shorthand
_.findIndex(characters, { 'age': 36 });
// → 0
// using "_.pluck" callback shorthand
_.findIndex(characters, 'blocked');
// → 1




// 找出数组中相同的值
_.intersection([1, 2, 3], [5, 2, 1, 4], [2, 1]);
// → [1, 2]

//   找出数组的前 n 个元素 
_.first([1, 2, 3]);
// → 1

_.first([1, 2, 3], 2);
// → [1, 2]

_.first([1, 2, 3], function(num) {
  return num < 3;
});
// → [1, 2]

// 找出数组中最后 n 个元素
_.last([1, 2, 3]);
// → 3
_.last([1, 2, 3], 2);
// → [2, 3]
_.last([1, 2, 3], function(num) {
  return num > 1;
});
// → [2, 3]

// 找出数组中某个元素的索引
_.indexOf([1, 2, 3, 1, 2, 3], 2);
// → 1
_.indexOf([1, 2, 3, 1, 2, 3], 2, 3);
// → 4
_.indexOf([1, 1, 2, 2, 3, 3], 2, true);
// → 2
_.lastIndexOf([1, 2, 3, 1, 2, 3], 2);
// → 4
// 从 第三个元素开始
_.lastIndexOf([1, 2, 3, 1, 2, 3], 2, 3);
// → 1

// 移除数组中指定的值
var array = [1, 2, 3, 1, 2, 3];
_.pull(array, 2, 3);
console.log(array);
// → [1, 1]

// 返回一个范围数组
// _.range([start=0], end, [step=1])
_.range(4);
// → [0, 1, 2, 3]
_.range(1, 5);
// → [1, 2, 3, 4]
_.range(0, 20, 5);
// → [0, 5, 10, 15]
_.range(0, -4, -1);
// → [0, -1, -2, -3]
_.range(1, 4, 0);
// → [1, 1, 1]
_.range(0);
// → []

// 移除数组中匹配条件的值
var array = [1, 2, 3, 4, 5, 6];
var evens = _.remove(array, function(num) { return num % 2 == 0; });
console.log(array);
// → [1, 3, 5]
console.log(evens);
// → [2, 4, 6]

// 切割数组，默认切割 1
// _.rest(array, [callback=1], [thisArg])
_.rest([1, 2, 3]);
// → [2, 3]
_.rest([1, 2, 3], 2);
// → [3]
_.rest([1, 2, 3], function(num) {
  return num < 3;
});
// → [3]


// 将多层嵌套的数组变成一层
_.flatten([1, [2], [3, [[4]]]]);
// → [1, 2, 3, 4];
var characters = [
  { 'name': 'barney', 'age': 30, 'pets': ['hoppy'] },
  { 'name': 'fred',   'age': 40, 'pets': ['baby puss', 'dino'] }
];
// using "_.pluck" callback shorthand
_.flatten(characters, 'pets');
// → ['hoppy', 'baby puss', 'dino']



// 更多实用函数见 http://lodash.com/docs

```
<!-- more -->

### 参考链接

- [underscorejs.org](http://underscorejs.org/)
- [lodash.com](http://lodash.com/)
- [http://blog.fens.me/nodejs-underscore/](http://blog.fens.me/nodejs-underscore/)
- [http://learningcn.com/underscore/](http://learningcn.com/underscore/)
- [Underscore.js 中文](http://javascript.ruanyifeng.com/library/underscore.html)
- [Say "Hello" to Lo-Dash](http://kitcambridge.be/blog/say-hello-to-lo-dash/)
-  [Differences between lodash and underscore](http://stackoverflow.com/questions/13789618/differences-between-lodash-and-underscore)