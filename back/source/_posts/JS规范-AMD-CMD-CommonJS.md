---
title: 模块化-AMD-CMD-CommonJS
categories: 前端工程化
date: 2016-9-11
---
#### JS模块系统:AMD，CMD，CommonJS规范
#### 同步策略：
#### CommonJS
  - 代码如下，使用require ，exports是nodejs服务端常使用的模块化机制，
    由于服务端的模块都是本地文件加载很快，使用同步模块加载方式不会出现问题
    ```js
    //add.js
    exports.add = function(){...};

    //calculate.js
    var cc = require('add');

    exports.add = function(n){
       return cc.add(val,n);
    };
    ```

  - 然而CommonJS规范并**不适用于浏览器端**,由于浏览器端的瓶颈在于带宽/网络,
  如果客户端同步加载一个个文件，然后执行，那么就会出现假死情况
  因此诞生了AMD，CMD，异步模块加载机制

<hr/>

#### 异步策略：AMD和CMD都属于异步策略（注:CMD和commonjs是不同的规范）
1. #### AMD：
    AMD属于依赖前置，把项目中需要用到的依赖提前声明，等获取之后执行callback

    ```js
    // 先创建一个 check_amd.js 的文件
    define(['check'], function(){
        var flag = true;
        function check(){
            return flag;
        }

        return {
            check: check
        };
    });

    // 在我们需要用到的页面加载模块
    require(['check_amd'], function (check){
        if(check.check()){
            console.log("哈哈哈");
        }
    });
    ```

2. #### CMD:
  详细文档 [https://github.com/seajs/seajs/issues/242](https://github.com/seajs/seajs/issues/242)
   代码在运行时，首先是不知道依赖的，需要遍历所有的require关键字，找出后面的依赖。具体做法是将function toString后，用正则匹配出require关键字后面的依赖。显然，这是一种牺牲性能来换取更多开发便利的方法。
   ```js
   // 先创建一个 check_cmd.js 的文件
    define(function(require, exports, module) {
       var a = require('a');//这里就不举例再创建a文件了
       function check(){
          return a.flag;
       }
       exports.check = check;
    });

    // 在我们需要用到的页面加载模块
    seajs.use(['check_cmd.js'], function(check){
       if(check.check()){
           console.log("哈哈哈");
       }
    });
  ```

3. #### AMD CMD 比较
   AMD是加载所有你所需要的文件
   而CMD会分析依赖，当你需要那个文件的时候他才加载

<hr/>
#### webpack commonjs
> 使用webpack可以很轻松的使用CommonJS规范，同时它会很好的支持AMD/CMD规范，方便代码迁移

#### 什么是系统？
> 系统泛指由一群有关连的个体组成，根据预先编排好的规则工作，能完成个别元件不能单独完成的工作的群体。系统分为自然系统与人为系统两大类。

#### 什么是模块系统？
> 在前端开发领域，一个模块，可以是JS 模块，也可以是 CSS 模块，或是 Template 等模块。
>
