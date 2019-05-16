---
title: This详解
categories: JavaScript语言专题
date: 2016-06-23

---
先说核心：**JS中this永远指向属性或方法所属的对象,或者说指向调用它的对象,this是在运行时绑定的**
### JavaScript中this的绑定分四种：

#### 1. 默认绑定：
这是最直接的一种方式，就是不加任何的修饰符直接调用函数

#### 2. 隐式绑定
这种情况会发生在调用位置存在「上下文对象」的情况，如

#### 3. 显式绑定

这种就是使用 Function.prototype 中的三个方法 call(), apply(), bind() 了。
这三个函数，都可以改变函数的 this 指向到指定的对象，
不同之处在于，call() 和 apply() 是立即执行函数，并且接受的参数的形式不同：

call(this, arg1, arg2, ...)
apply(this, [arg1, arg2, ...])
而 bind() 则是创建一个新的包装函数，并且返回，而不是立刻执行。

bind(this, arg1, arg2, ...)
apply() 接收参数的形式，有助于函数嵌套函数的时候，把 arguments 变量传递到下一层函数中。

#### 4. new 绑定

最后一种，则是使用 new 操作符会产生 this 的绑定。
在理解 new 操作符对 this 的影响，首先要理解 new 的原理。
在 JavaScript 中，new 操作符并不像其他面向对象的语言一样，而是一种模拟出来的机制。
在 JavaScript 中，所有的函数都可以被 new 调用，这时候这个函数一般会被称为「构造函数」，实际上并不存在所谓「构造函数」，更确切的理解应该是对于函数的「构造调用」。
使用 new 来调用函数，会自动执行下面操作：
1. 创建一个全新的对象。
2. 这个新对象会被执行 [[Prototype]] 连接。
3. 这个新对象会绑定到函数调用的 this。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象

### Example

```js

function foo() {
    console.log(this.a);
}

var a = 0;

var obj1 = {
    a: 2,
    foo,
}

foo() //0, 默认绑定
obj1.foo();     // 2, 隐式绑定
obj1.foo.call({a: 1});  // 1, 显式绑定

var f = new foo();
```
