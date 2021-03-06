---
title: 异步模型
categories: JavaScript语言专题
date: 2016-11-01
---

异步模型：
1. 同步模型：遇到网络IO等耗时较长的操作时，进程会一直等待，导致CPU空闲，整个系统资源被浪费
2. 多线程模型：在上述情况下，可能会同时等待多个IO请求，导致系统资源被严重浪费
3. 异步模型：主线程会正常执行同步代码，遇到异步任务时交给工作线程，等异步任务执行完成，工作线程通知主线程，主线程后触发callback

JS中的异步模型使用事件实现
java的异步实现，是多个线程同时工作，用同步锁的方式实现同步资源的访问

JS 运行机制
1. 由于JavaScript是单线程的，因此所有任务都需要排队
2. 同步任务会在主线程中执行，形成一个执行栈
3. 如果遇到异步任务，则把它放置在task queue中
4. 等所有同步任务执行完成后（才会执行异步任务）
5. event loop会不停循环遍历task queue中的异步任务，如果发现某个异步任务完成，就会执行对应的callback
6. （所谓Event Loop机制，指的是一种内部循环，用来一轮又一轮地处理消息队列之中的消息，即执行对应的回调函数)
