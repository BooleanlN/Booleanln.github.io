---
title: box-shadow详解
tags:
  - Html
url: 151.html
id: 151
categories:
  - Html是程序语言乎？
date: 2018-07-13 14:38:03
---

###### box-shadow详解

语法：

box-shadow:none|inset(可选值，不设置，为外投影，inset，为内投影) x-offset(阴影水平偏移量，正方向为right）y-offset（阴影垂直便宜量，正方向为bottom）blur-radius(阴影模糊半径，为正，0为无模糊效果，值越大，越模糊) spread-radius(阴影拓展半径) color)

1.  阴影宽度：spread-radius主要控制阴影的大小即宽度，四条边的阴影宽度都由他与阴影偏移量的和决定。当不存在阴影偏移量时，仅由spread-radius直接控制阴影的大小即宽度。当存在水平偏移量时，left与right边的阴影宽度为spread-radius与偏移量的和。当存在垂直偏移量时，同理可推。
    
2.  内外阴影：当不存在inset值得时候，阴影仅在box外部表现。且阴影宽度由spread-radius与对应方向上的阴影偏移量的和决定。存在inset时，阴影在Box内部表现。其余规则相同。
    
3.  阴影偏移量：x-offset正方向为right。y-offset 正方向为bottom。当spread-radius 为0时，设置偏移量仍可表现出shadow，我理解为，浏览器会自动填充box-shadow最外围与border 之间的空隙。这样也可以解释，为什么有了偏移量后，为什么阴影宽度会改变。
    
4.  blur-radius：具体表现规则不清楚，但是不占据shadow的空间。（因为设置多层shadow时，会与其他shadow颜色叠加，而不是挤走）。
    
5.  多层阴影：最内层优先级最高，之后依次降低。使用逗号“，”隔开