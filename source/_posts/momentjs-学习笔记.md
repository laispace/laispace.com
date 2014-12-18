title: moment.js 学习笔记
categories:
  - Javascript
date: 2014-07-18 03:59:03
tags:
- moment

---

moment.js 是一个专注于处理时间的工具库。

<!-- more -->

```

var moment = require('moment');

// moment 创建了一个 Date 对象的容器，使用 moment() 来获取这个对象
// moment().format();

// # Now
// 获取当前时间 Moment 对象
console.log(
    moment()
);
// => 输出一个 Moment 对象，属性如下：
/*{ _isAMomentObject: true,
*    _i: undefined,    // input
*    _f: undefined,    // format
*    _l: undefined,
*   _strict: undefined,
*    _isUTC: false,
*    _pf:
*    { empty: false,
*        unusedTokens: [],
*        unusedInput: [],
*        overflow: -2,
*        charsLeftOver: 0,
*        nullInput: false,
*        invalidMonth: null,
*        invalidFormat: false,
*        userInvalidated: false,
*        iso: false },
*    _d: Mon Jul 14 2014 21:32:18 GMT+0800 (CST)
}*/

// 将字符串转化为 Moment 对象
console.log(
    moment('July 14, 2014')
);

// 判断字符串是否为时间字符串
console.log(
    moment('I\'m not a date string').isValid()
);

// 已知时间字符串的格式，格式化为 Moment 对象
console.log(
    moment('07-14-2014', 'MM-DD-YYYY')
);
// 会忽略非数字字母以外的字符，所以以下输入也可以：
console.log(
    moment('07.14.2014', 'MM-DD-YYYY')
);

// 未指定时区，创建的 Moment 对象将基于本地时间
console.log(
    moment('07-14-2014 21:30', 'YYYY-MM-DD HH:mm') // => Tue Feb 20 7 21:30:00 GMT+0800
);
// 指定时区
console.log(
    moment('07-14-2014 21:30 +0000', 'YYYY-MM-DD HH:mm Z') // => Wed Feb 21 7 05:30:00 GMT+0800
);

// 若 返回的 Moment 对象对应的时间不存在时，其 isValid() 将返回 false
console.log(
    moment('15-14-2014', 'MM DD YYYY').isValid()  // => false  不存在15月
);
console.log(
    moment('06-31-2014', 'MM DD YYYY').isValid()  // => false  6月可没有31日！
);

// moment() 第三个参数可指定是否严格匹配，默认为 false
console.log(
    moment('Today is 2014-07-14', 'YYYY-MM-DD').isValid() // => true
);
// 严格匹配
console.log(
    moment('Today is 2014-07-14', 'YYYY-MM-DD', true).isValid() // => false
);

// 创建一个 Moment 对象
console.log(
    // 传入一个对象
    moment({year: 2014, month: 6, day: 14, hour: 19, minute: 5, second: 4, millisecond: 321}),
    // 传入一个数组
    // [year, month, day, hour, minute, second, millisecond]
    moment([2014, 6, 14, 19, 5, 4, 321])
);



// 或传入一个已有的 Date 对象
var date = new Date(2014, 6, 14, 19, 5, 4, 321);
console.log(
    moment(date)
);

// 判断错误位置
// 0 years
// 1 months
// 2 days
// 3 hours
// 4 minutes
// 5 seconds
// 6 milliseconds
console.log(
    moment("2014-07-14T21:13:90").invalidAt() //=> 5 即 90 秒是错误的
);

// 设置 或 读取
// 这里的 second 可以替换为
// minute/hour/date/day/weekday/isoWeekday/dayOfYear/
// week/isoWeek/month/quarter/year/weekYear/isoWeekYear/weeksInYear/isoWeeksInYear/
moment().second(Number);
moment().second(); // Number
moment().seconds(Number);

// 读取
moment().get('year');
moment().get('month');  // 0 to 11
moment().get('date');
moment().get('hour');
moment().get('minute');
moment().get('second');
moment().get('millisecond');

// 设置
moment().set('year', 2014);
moment().set('month', 6);  // 七月
moment().set('date', 17);
moment().set('hour', 19);
moment().set('minute', 5);
moment().set('second', 4);
moment().set('millisecond', 321);

// 日期的增、减和比较
var a = moment().subtract(1, 'day');
var b = moment().add(1, 'day');
moment.max(a, b);  //=> b
moment.min(a, b);  //=> a

// 链式调用更新
moment().add('days', 1).subtract('months', 1).year(2014).hours(19).minutes(5).seconds(5);

// 计算时间间隔
moment([2011, 6, 17]).fromNow(); //=> '3 years ago''
var a = moment([2009, 6, 17]);
var b = moment([2014, 8, 18]);
a.from(b)  //=> '5 years ago''
a.diff(b) //=> -163209600000 单位是 ms
a.diff(b, 'days') //=> -1889
a.diff(b, 'years', true); //=> -5.169398907103825 第三个参数为false表示输出浮点数

// 判断时间是否相同
moment('2014-06-17').isSame('2014 06 17'); //=> true

// 判断时间前后
moment('2014-06-18').isAfter('2014-06-17'); //=> true

// 判断是否为闰年
moment([2014]).isLeapYear(); //=> true

// 判断是否为 Moment 对象
moment.isMoment() //=> false
moment.isMoment(new Date()) //=> false
moment.isMoment(moment()) //=> true

```


### 参考链接

- [momentjs.com](http://momentjs.com/docs/#/parsing/)
