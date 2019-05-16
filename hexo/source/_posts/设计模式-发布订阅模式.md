---
title: 设计模式-发布订阅模式
categories: 设计模式、算法
date: 2016-08-29
---
前端事件的发布订阅模式十分普遍，
包括原生的addEventListener，dispatchEvent，或者angular vuejs中的emit on都属于发布订阅模式
下面我将简单的实现一下订阅发布模式，更多的设计模式可以再我的[github design-magic](https://github.com/zhentaoo/design-magic)项目中看到

代码如下，首先构造一个消息队列Queue，订阅者往Queue中添加自己需要订阅的事件，当发布者发布一个事件时，遍历整个Queue,如果找到就触发订阅者订阅的事件
```js
var Queue = {};

var events = {
  subscribe: function(event, fn) {
    if (Queue[event]) {
      throw new Error('this event has been regist!')
      return;
    }
    Queue[event] = fn;
  },
  publish: function(event) {
    Queue[event]();
  }
}

var p1 =  events.subscribe('click',function() {
  console.log('p1');
});

events.publish('click');
```

用原生dom的api实现如下
```js
var test = new Event('test') //自定义test事件
var html = document.querySelector("html") //获取html和body元素
var body = document.querySelector("body")

html.addEventListener('test',()=>{console.log('test html')},true) //在html，body元素上都绑定test事件,并且设置为true
body.addEventListener('test',()=>{console.log('test body')},true)

// html.dispatchEvent(test)
body.dispatchEvent(test)
```
