---
title: CSS-Position-Float
date: 2016-06-29
categories: CSS
---
如果要掌握、运用好Position、Float属性必须要对HTML的两个基本点有清晰的了解。

1. 盒子模型（box model）
2. HTML的普通流（normal flow）

[盒子模型](/2017/03/18/CSS-盒模型/)

> 在HTML中元素的盒子模型分为两种：块状元素、行内元素，请注意这里的块状元素（Block）和行内元素（Inline）与Display属性中的inline、block两个属性值并不等同。盒子模型中的Inline、Block类似于是Display属性的父类，例如：Display属性中的list-item属性值是属于块状（Block）类型的。

> 我们直观的上看两种盒子模型的__区别__

> 块状（Block）类型的元素可以设置width、height属性，而行内（Inline）类型设置无效。
块状（Block）类型的元素会独占一行（直观的说就是会换行显示，无法与其他元素在同一行内显示，除非你主动修改元素的样式），而行内（Inline）类型的元素则都会在一行内显示。
块状（Block）类型的元素的width默认为100%，而行内（Inline）类型的元素则是根据自身的内容及子元素来决定宽度。
列举出一些大家常见的元素的分类

>块状元素：P、DIV、UL、LI、DD、DT...
行内元素：A、IMG、SPAN、STRONG...

HTML的普通流

> 浏览器在读取HTML源代码的时候是根据元素在代码出现的顺序读取，最终元素的呈现方式是依据元素的盒子模型来决定的。
行内元素是从左到右，块状元素是从上到下。（如下图）

> 如果你不改变元素的默认样式前提下，元素在HTML的普通流中会“占用”一个位置，而“占用”位置的大小、位置则是由元素的盒子模型来决定。因此，在后续讲的Position、Float属性是否会使元素脱离这个普通流是一个关键点。


Position:(static、relative、absolute、fixed)：
我们首先来谈谈Position属性，因为Position属性能够很好的体现HTML普通流这个特征。重点在于应用了不同的position值之后是否有脱离普通流和改变Display属性这两点。

  1. Static
  所有元素在默认的情况下position属性均为static，而我们在布局上经常会用到的相对定位和绝对定位常用的属性top、bottom、left、right在position为static的情况下无效。其用法为：在改变了元素的position属性后可以将元素重置为static让其回归到页面默认的普通流中。

  2. Relative
  俗称的相对定位，重点在于对相对理解。我们此前说过每个元素在页面的普通流中会有“占用”一个位置，这个位置可以理解为默认位置，而相对定位就是将元素偏离元素的默认位置，但普通流中依然保持着原有的默认位置，__并没有脱离普通流__，只是视觉上发生的偏移。
  我们先用块状元素来做个示例：

  3. Absolute
  俗称的绝对定位，绝对定位是相对而言的，怎么理解呢？应用了position: absolute的元素会循着节点树中的父（祖）元素来确定“根”，然后相对这个“根”元素来偏移。如果在其节点树中所有父（祖）元素都没有设置position属性值为relative或者absolute则该元素最终将对body进行位置偏移。应用了position: absolute的元素会脱离页面中的普通流并改变Display属性（重点）！

  4. Fixed
  会改变行内元素的呈现模式，使display之变更为block。
  会让元素脱离普通流，不占据空间。
  默认会覆盖到非定位元素上。

Float:(none、left、right)
特点:
  - 只有横向浮动，并没有纵向浮动。
  - 当元素应用了float属性后，将会脱离普通流，其容器（父）元素将得不到脱离普通流的子元素高度。
  - 会将元素的display属性变更为block。
  - 浮动元素的后一个元素会围绕着浮动元素（典型运用是文字围绕图片），与应用了position的元素相比浮动元素并不会遮盖后一个元素。
  - 浮动元素的前一个元素不会受到任何影响（如果你想让两个块状元素并排显示，必须让两个块状元素都应用float）。

清除浮动的方法有两种：
  1. 通过在容器中添加一个标签，设置该标签的样式为 clear: both
  2. 容器设置overflow: hidden
  3. :after属性，实际还是clear both


  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title></title>
    </head>
    <body>
      <div class="out">
        <div class="in">
          in1in1in1in1in1in1in1in1
        </div>
        <div class="in">
          in2in2in2in2in2in2in2in2
        </div>
        <div class="in">
          in3in3in3in3in3inin3in3
        </div>
        <div class="clear">

        </div>
      </div>
    </body>
  </html>

  <style media="screen">
    .out {
      border: 1px solid red;
      /*overflow: auto;*/

    }
    .out::after {
      clear:both;
      content:'.';
      display:block;
      width: 0;
      height: 0;
      visibility:hidden;
    }
    .in {
      float: left;
      height: 100px;
      border: 1px solid gray;
    }

    .clear {
      /*clear: both;*/
    }
  </style>
  ```
