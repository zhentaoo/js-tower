---
title: Promise
categories: JavaScript语言专题
date: 2016-9-23
---
Promise对象是CommonJS工作组提出的一种规范，目的是为异步操作提供统一接口。:[阮老师博客](http://javascript.ruanyifeng.com/advanced/promise.html)

__什么是Promises？__
首先，它是一个对象，也就是说与其他JavaScript对象的用法，没有什么两样；其次，它起到代理作用（proxy），充当异步操作与回调函数之间的中介。它使得异步操作具备同步操作的接口，使得程序具备正常的同步运行的流程，回调函数不必再一层层嵌套。

简单说，它的思想是，每一个异步任务立刻返回一个Promise对象，由于是立刻返回，所以可以采用同步操作的流程。这个Promises对象有一个then方法，允许指定回调函数，在异步任务完成后调用。

比如，异步操作f1返回一个Promise对象，它的回调函数f2写法如下。
`(new Promise(f1)).then(f2);`
相比回调方式，代码可读性更强
```js
// 传统写法
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // ...
      });
    });
  });
});

// Promises的写法
(new Promise(step1))
  .then(step2)
  .then(step3)
  .then(step4);
```
ES6提供了__原生的Promise__构造函数，用来生成Promise实例。
__Promise构造函数接受一个函数作为参数__，该函数的两个参数分别是resolve和reject。它们是两个函数，由__JavaScript引擎提供__，不用自己部署。
__resolve__函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。
__reject__函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。


```js
var promise = new Promise(function(resolve, reject) {
  // 异步操作的代码

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise对象只有三种状态。
  1. 异步操作“未完成”（pending）
  1. 异步操作“已完成”（resolved，又称fulfilled）
  1. 异步操作“失败”（rejected）

这三种的状态的变化途径只有两种。
  1. 异步操作从“未完成”到“已完成”
  1. 异步操作从“未完成”到“失败”。

Promise对象的最终结果只有两种
  1. 步操作成功，Promise对象传回一个值，状态变为resolved。
  1. 异步操作失败，Promise对象抛出一个错误，状态变为rejected。


 #### 如何实现一个Promise
 可以看到它的api并不多，其实规范也不多，我觉的大致抓住几个重要的点就够了，

  1. promise有三种状态， 等待（pending）、已完成（fulfilled）、已拒绝（rejected）
  2. promise的状态只能从“等待”转到“完成”或者“拒绝”，不能逆向转换，同时“完成”和“拒绝”也不能相互转换
  3. promise必须有一个then方法，而且要返回一个promise，供then的链式调用，也就是可thenable的
  4. then接受俩个回调(成功与拒绝),在相应的状态转变时触发，回调可返回promise，等待此promise被resolved后，继续触发then链
