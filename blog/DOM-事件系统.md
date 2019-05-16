---
title: DOM-事件系统
date: 2016-05-01 20:11:15
categories: BOM、DOM
---
#### 事件是DOM中一个很有用的模型，但是被现代框架封装之后，一般的开发人员可能很少直接接触到。
#### 事件是一种异步编程的实现，是程序各个组成部分之间的一种通信方式，也是系统解耦的一种不错的方式。

DOM的事件操作（监听和触发），都定义在EventTarget接口。Element节点、document节点和window对象，都实现了这个接口。此外，XMLHttpRequest、AudioNode、AudioContext等浏览器内置对象，也部署了这个接口。
该接口有三个方法。

```
addEventListener：绑定事件的监听函数
removeEventListener：移除事件的监听函数
dispatchEvent：触发事件
```

废话不说，直接上代码：
```js
var test = new Event('test') //自定义test事件
var html = document.querySelector("html") //获取html和body元素
var body = document.querySelector("body")

html.addEventListener('test',()=>{console.log('test html')},true) //在html，body元素上都绑定test事件,并且设置为true
body.addEventListener('test',()=>{console.log('test body')},true)

// html.dispatchEvent(test)
body.dispatchEvent(test)
```
运行之后，body dispatch事件，由于事件冒泡，会触发body和html绑定的时间
html dispatch，不会触发body绑定的事件
至此，自定义事件也就实现完毕
