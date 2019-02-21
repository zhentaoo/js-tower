---
title: 数组
date: 2016-07-03 20:10:20
categories: JavaScript语言专题
---
虽然在 JavaScript 中数组是是对象，但是没有好的理由去使用 for in 循环 遍历数组。
相反，有一些好的理由不去使用 for in 遍历数组。

注意: JavaScript 中数组不是 关联数组。 JavaScript 中只有对象 来管理键值的对应关系。但是关联数组是保持顺序的，而对象不是。
由于 for in 循环会枚举原型链上的所有属性，唯一过滤这些属性的方式是使用 hasOwnProperty 函数， 因此会比普通的 for 循环慢上好多倍。

##### 遍历:为了达到遍历数组的最佳性能，推荐使用经典的 for 循环。

```js
var list = [1, 2, 3, 4, 5, ...... 100000000];
for(var i = 0, l = list.length; i < l; i++) {
    console.log(list[i]);
}
```
上面代码有一个处理，就是通过 l = list.length 来缓存数组的长度。

虽然 length 是数组的一个属性，但是在每次循环中访问它还是有性能开销。 可能最新的 JavaScript 引擎在这点上做了优化，但是我们没法保证自己的代码是否运行在这些最近的引擎之上。

实际上，不使用缓存数组长度的方式比缓存版本要慢很多。

##### length属性:length的getter 方式会简单的返回数组的长度，而 setter 方式会截断数组。

``` js
var foo = [1, 2, 3, 4, 5, 6];
foo.length = 3;
foo; // [1, 2, 3]

foo.length = 6;
foo; // [1, 2, 3]
```

译者注： 在 Firebug 中查看此时 foo 的值是： [1, 2, 3, undefined, undefined, undefined] 但是这个结果并不准确，如果你在 Chrome 的控制台查看 foo 的结果，你会发现是这样的： [1, 2, 3] 因为在 JavaScript 中 undefined 是一个变量，注意是变量不是关键字，因此上面两个结果的意义是完全不相同的。

##### 结论:为了更好的性能，推荐使用普通的 for 循环并缓存数组的 length 属性。 使用 for in 遍历数组被认为是不好的代码习惯并倾向于产生错误和导致性能问题。
