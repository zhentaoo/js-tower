---
title: SVG的能力
date: 2017-05-08
categories: 数据可视化
---

看到这篇文章的标题，大家可能会想，现在都17年了，还用SVG？肯定上canvas，WebGL啊！！
没错，我以前也是这么想的，SVG这种老古董学他做甚。
但是最近打算做“绘制流程图”的工具软件时，发现市面上：bpmn.js，百度脑图等用户体验极好的此类产品，
都用的SVG技术。那我的想法是不是有些问题？是不是之前太低估SVG了？

### 那么首先来讨论SVG，Canvas区别
- canvas: 简单，快速，Cocos-JS就是基于canvas的游戏框架。不言而喻，canvas十分适合大量游戏对象的场景。相对于SVG而言，canvas的用户交互更颗粒化，是更为强大底层的API。然而，canvas绘制的某个图形，无法单独绑定事件，只能通过其他一些比较麻烦的手段处理（有兴趣自行百度）。
- SVG: 内置线，圆，矩形，多边形等形状，可以单独绑定事件，用户的交互实现更加方便。SVG也常用于保存矢量图。
- 更加细致的比较：https://msdn.microsoft.com/zh-cn/library/gg193983(v=vs.85).aspx

基于上述这些原因，至少在“工作流引擎”这个领域，SVG还是更为合适的。

### SVG基础知识

``` html
  <?xml version="1.0" standalone="no"?>
  <svg width="200" height="250" version="1.1" xmlns="http://www.w3.org/2000/svg">

    <rect x="10" y="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>
    <rect x="60" y="10" rx="10" ry="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>

    <circle cx="25" cy="75" r="20" stroke="red" fill="transparent" stroke-width="5"/>
    <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" fill="transparent" stroke-width="5"/>

    <line x1="10" x2="50" y1="110" y2="150" stroke="orange" fill="transparent" stroke-width="5"/>
    <polyline points="60 110 65 120 70 115 75 130 80 125 85 140 90 135 95 150 100 145"
        stroke="orange" fill="transparent" stroke-width="5"/>

    <polygon points="50 160 55 180 70 180 60 190 65 205 50 195 35 205 40 190 30 180 45 180"
        stroke="green" fill="transparent" stroke-width="5"/>

    <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue" stroke-width="5"/>
  </svg>
```

上诉代码效果如图：
<img src="./img/svg0.png" width = "100" height = "260" align=center />


那么，可以清楚的看到SVG绘图的基本能力。
### 未完待续....
