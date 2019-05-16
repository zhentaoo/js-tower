---
title: 闭包
date: 2016-05-13 20:10:36
categories: JavaScript语言专题
---

#### 闭包的定义
《JavaScript权威指南》的定义：“闭包是指有权访问另一个函数作用域中的变量的函数”。解释一下，如果一个函数能访问另一个函数作用域中的变量，那么就可以称这个函数是闭包。如下，inner可以访问out中的a变量，那么inner就可以称作闭包。

[JavaScript秘密花园－函数闭包的解释](http://bonsaiden.github.io/JavaScript-Garden/zh/#function)

其实核心只有两点：
**1.闭包是一个函数**
**2.闭包可以访问其他函数作用域中的变量**

``` js
function out () {
    var a = 1;

    function inner () {
        console.log(1);
    }
    inner();
}

out();
```

#### 闭包的用处
**1.模拟块级作用域**
JavaScript中没有块级作用域的概念，所以在for、if中定义的变量，会一直存在。
如果需要模拟块级作用域，用以下方式，匿名函数在执行之后马上销毁，就可以达到类似块级作用域的效果。
```js
function output() {

  (function() {
    for (var i = 0; i < count.length; i++) {
      alert(i)
    }
  })();

  alert(i); //导致一个错误
}
```

**2.模拟私有变量**
JavaScript中没私有成员的概念，所有对象属性都是公开的。
但是任何在函数中定义的变量，都可以认为是私有变量。
所以可以通过闭包的形式创建特权方法，访问私有变量。
```js

var Foo = function() {
    var name = 'fooname';
    var age = 12;
    console.log('foo');
    this.getName = function() {
        return name;
    };
    this.getAge = function() {
        return age;
    };
};

var foo = new Foo();

foo.name;        //  => undefined
foo.age;         //  => undefined
foo.getName();   //  => 'fooname'
foo.getAge();    //  => 12

```

#### 闭包的副作用
1. 闭包有可能造成内存泄漏

2. 闭包只能获取父函数中任一变量的最后一个值
```js
// 以下代码打印出的都会是10
for(var i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);  
    }, 1000);
}

// 为了正确的获得循环序号，最好使用自执行匿名函数
for(var i = 0; i < 10; i++) {
    (function(e) {
        setTimeout(function() {
            console.log(e);  
        }, 1000);
    })(i);
}

```
