---
title: 设计模式-DI依赖注入
type: 基础语法
date: 2016-08-03
categories: 设计模式、算法
---
在绝大多数后端框架，以及前端的angular框架中都有实现依赖注入
下面我将简单的实现一下依赖注入模式，更多的设计模式可以再我的[github design-magic](https://github.com/zhentaoo/design-magic)项目中看到

好处：参考代码，tom模块依赖 person／animal模块，如果不使用依赖注入那么在tom模块中，我就需要手动引入person／animal并进行初始化，我就需要关心这两个模块的路径，传参，如何初始化等一系列问题，而且后期测试时非常不友好。但是使用di之后，我只需要引入我要的依赖，至于如何查找，初始化，交给di去做就可以了，测试时我只需要关注tom模块是否正确

简明实现思路：1:注册服务，2:解析模块的依赖，3:查找依赖并返回给模块

```js
var DI = function() {
  this.module = {};
}

// 注入依赖
DI.prototype.inject = function(k, v) {
  this.module[k] = v;
}

// 解析一个模块的依赖
DI.prototype.parseDependency = function(funcStr) {
  if (typeof funcStr == 'function') {
    funcStr = funcStr.toString();
  }

  var regexp = /^function\s*[^\(]*\(\s*([^\)]*)\)/m;
  var args = funcStr.match(regexp)[1];
  args = args.replace(/ /g, '').split(',');
  return args;
}

// 返回一个func运行结果
DI.prototype.resolve = function(func) {
  if (typeof func != 'function') {
    return func;
  }

  var dependencies = this.parseDependency(func);
  var args = [];

  for (var i = 0, lenth = dependencies.length; i < lenth; i++) {
    var dependencyValue = this.module[dependencies[i]];
    var arg = this.resolve(dependencyValue);

    args.push(arg);
  }

  return func.apply(this, args);
}

module.exports = DI;
```
