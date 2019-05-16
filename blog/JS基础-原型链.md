---
title: 原型链
date: 2016-07-03
categories: JavaScript语言专题
---

#### 原型
`prototype`：显示原型; `__proto__`：隐式原型;

每个function都有prototype属性(指针)，指向原型对象(包含所有实例共享的属性和方法)
每个对象(包括function)都具有属性`__proto__`，指向构造它的 构造函数原型
```js
var Father = function () {}
var f1 = new Father()
var f2 = new Father()

f1.__proto__ === Father.prototype; //true
f2.__proto__ === Father.prototype; //true
f1.__proto__ === f2.__proto__; //true
```

#### 继承
思路：让子类prototype指向父类的实例，然后再添加子类独有的方法、属性
注意：如果子类prototype直接指向父类prototype，则父子共享，造成污染，不可行....

**实现继承本质是重写原型对象**

```js
function Father() {}
Father.prototype.say = function () {
  console.log('Father can say');
}
var f = new Father();

function Son() {}
Son.prototype = new Father();
Son.prototype.eat = function () {
  console.log('son can eat');
}

var s = new Son();

f.say() // success
f.eat() // error

s.say() // success
s.eat() // success

```
