---
title: 事件循环
categories: JavaScript语言专题
date: 2016-11-01
---
参考：[http://blog.csdn.net/alex8046/article/details/51914205](http://blog.csdn.net/alex8046/article/details/51914205)
消息队列：是一个先进先出的队列，它里面存放着各种消息。（消息就是注册异步任务时添加的回调函数。）
__事件循环__：事件循环是指主线程重复从消息队列中取消息、执行的过程。

用代码表示如下：
```js
while(true) {
    var message = queue.get();
    execute(message);
}
```
<img src="./img/eventloop.png" width = "580" height = "350" align=center />

异步的过程 ：
1. **主线程**发起一个**异步请求**
2. 相应的**工作线程**接收请求并告知主线程已收到(异步函数返回)；
3. 主线程可以继续执行后面的代码，同时工作线程执行异步任务；
4. 工作线程完成工作后，**通知**主线程；
5. 主线程收到通知后，执行一定的动作(调用回调函数)。

从生产者，消费者角度来看异步的过程：
1. 工作线程是生产者，主线程是消费者(只有一个消费者)。
2. 工作线程执行异步任务，执行完成后把对应的回调函数封装成一条消息放到消息队列中；
3. 主线程不断地从消息队列中取消息并执行，当消息队列空时主线程阻塞，直到消息队列再次非空。

JS异步过程：
1. 由于JavaScript是单线程的，因此所有任务都需要排队
2. 同步任务会在主线程中执行，形成一个执行栈，栈底是全局上下文，栈顶是当前执行的函数上下文
3. 主线程如果遇到异步任务，则把该异步任务放置在工作线程(net/fs)中，该异步任务完成后放到task queue中
4. 等所有同步任务执行完成后,才会执行异步任务
5. 主线程的event loop会循环遍历task queue，如果发现有异步任务，就会把这个异步任务对应的call back至于执行中并执行

异步模型：
1. 同步模型：遇到网络IO等耗时较长的操作时，进程会一直等待，导致CPU空闲，整个系统资源被浪费
2. 多线程模型：在上述情况下，可能会同时等待多个IO请求，导致系统资源被严重浪费
3. 异步模型：会先把同步任务执行完，把异步任务丢到task queue中，等异步任务完成之后再执行对应的callback

参考：[https://cnodejs.org/topic/57d68794cb6f605d360105bf](https://cnodejs.org/topic/57d68794cb6f605d360105bf)
当Node.js启动时会初始化event loop, 每一个event loop都会包含按如下顺序六个循环阶段，
```bash
   ┌───────────────────────┐
┌─>│        timers         │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │
│  └──────────┬────────────┘      ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │<─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └───────────────┘
│  │        check          │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │
   └───────────────────────┘
```
- timers 阶段: 这个阶段执行setTimeout(callback) and setInterval(callback)预定的callback;
- I/O callbacks 阶段: 执行除了 close事件的callbacks、被timers(定时器，setTimeout、setInterval等)设定的callbacks-setImmediate()设定的callbacks之外的callbacks;
- idle, prepare 阶段: 仅node内部使用;
- poll 阶段: 获取新的I/O事件, 适当的条件下node将阻塞在这里;
- check 阶段: 执行setImmediate() 设定的callbacks;
- close callbacks 阶段: 比如socket.on(‘close’, callback)的callback会在这个阶段执行.


```js
console.log(‘1’);

setImmediate(function () {
console.log(‘2’);
});

setTimeout(function () {
console.log(‘3’);
},0);

process.nextTick(function () {
console.log(‘4’);
});
```
```1–4--2–3```
