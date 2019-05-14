---
title: VueJS-项目搭建
date: 2017-02-27
categories: React、Vue
---
- 想系统学习vue的同学，可以看下Vue官方教程[Vue2官方文档](https://cn.vuejs.org/v2/guide/)
- 为了降低学习成本，我在这里提供一个搭建Vue seed的脚手架工具[poke-ball](https://www.npmjs.com/package/poke-ball)，文档可以在poke-ball的README.md中看到
- 假设你本地安装了node，也执行命令`npm install -g poke-ball`全局安装了poke-ball,并用`kk new yourprojectname`会初始化一个VUE项目seed，那么恭喜你可以看接下来的教程

1. #### 首先介绍在搭建vue seed中最常用的模块
  - **Vue router:** 文档:[VUE router](https://router.vuejs.org/zh-cn/?q=)一个标准VUE项目基本遵循着:路由->模块->视图的流程
    这个过程中，路由一般用来做权限控制，所以路由的各种用法最好还是通览一遍

  - **Vue resource:** 在与后端通信过程中常用模块。也是建议文档通览一遍，有需要的时候进行深究。

2. #### 接下来解读这个SEED项目结构，外层的mock，node_modules到不赘述
src
├── assets        -----------------资源
│   └── xxx.png  
├── components    -----------------自定义组件
│   ├── index.js
│   ├── leo.vue
│   └── zt.vue
├── constants     -----------------常量
│   └── index.js
├── directive     -----------------自定义指令
│   └── index.js
├── filters       -----------------自定义过滤器
│   └── index.js
├── index.html    -----------------项目模版，给webpack html plugin使用
├── main.js       
├── resource      -----------------后端服务
│   ├── index.js
│   └── mock.js
├── scss          -----------------样式库
│   └── app.scss
├── util          -----------------工具库
│   └── index.js
└── views         -----------------具体的页面
    ├── common        -----------------通用：定义在不同响应状态下的页面
    │   ├── 403.vue
    │   ├── 404.vue
    │   ├── 502.vue
    │   ├── index.js
    │   └── welcome.vue
    ├── first         -----------------使用poke-ball生成的模块
    │   ├── cli
    │   │   └── test
    │   │       ├── index.vue
    │   │       └── test.js
    │   └── index.js
    ├── layout.vue    -----------------项目布局文件
    ├── menus.config.js -----------------导航栏配置
    └── routes.js     -----------------项目路由
