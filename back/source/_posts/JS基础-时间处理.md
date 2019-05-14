---
title: 时间处理
date: 2016-04-10
categories: JavaScript语言专题
---

计算机世界里的日期都是从1970年1月1日开始计算，时间戳是指**1970年1月1日**至今的毫秒数
关于时间/日期的处理，JS中有个一使用频率很高的库：__moment__
npm install moment --save即可使用。
官方文档：[https://momentjs.com/](https://momentjs.com/)
这个库我就不多介绍了，也有中文文档，有兴趣和强需求的同学可以看看。

#### 接下来我将介绍JS中原生的日期处理方法：Date
Date是JavaScript提供的日期和时间的操作接口。它表示的时间范围是:1970年1月1日00:00:00前后各1亿天（单位毫秒）。

我们经常会 new Date()打印当前时间，一般会得到如下结果
Wed Feb 01 2017 08:00:00 GMT+0800 (CST)
开始解读这个这个字符串，分别是周3，2月，1日，2017年，8点,至于GMT CST
```
GMT(Greenwich Mean Time)代表格林尼治标准时间
CST同时代表如下4个不同的时区：
Central Standard Time (USA) UT-6:00
Central Standard Time (Australia) UT+9:30
China Standard Time UT+8:00
Cuba Standard Time UT-4:00
```

下面是常用的获取当前时间的方法，需要的话可以封装一下用来返回不同风格的日期

```js
var date = new Date();

date.getTime();//获取当前时间戳

//注：getMonth返回的是（0-11）需要加1
console.log(date.getFullYear() + '年' + (date.getMonth()+1) + '月' + date.getDate() + '日');

console.log(date.getHours() + '时' + date.getMinutes() + '分' + date.getSeconds() + '秒');
```

如果想把某个时间戳转化成日期
```js
var date = new Date(1488094640437);
//同时支持不同形式的入参
// new Date('2017-02-01')
// new Date(year, month, day, hours, minutes, seconds, milliseconds)

// 然后可以获取这个时间戳对应的日期
console.log(date.getFullYear() + '年' + (date.getMonth()+1) + '月' + date.getDate() + '日');
console.log(date.getHours() + '时' + date.getMinutes() + '分' + date.getSeconds() + '秒');

```
