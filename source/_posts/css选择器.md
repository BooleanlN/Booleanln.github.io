---
title: css选择器
date: 2019-05-05 22:21:38
tags:
- CSS
---

#### CSS选择器整理

CSS选择器是CSS的基础，但有很多开发者并没有充分使用它的功能，虽然可以通过类，ID以及标签选择器完成很多工作，但还有更多更好的方式可供我们选择。

学会合理使用CSS2、CSS3中的选择器可以帮助我们编写出更加整洁的HTML代码。它可以让你尽可能少用不必须的类属性和少添加多余的div和span标签。

选择器未能充分利用归因于IE对其不能有效的支持，但其他浏览器都有很好的支持，好消息是从IE7+开始，选择器得到了更好的支持。

废话不说（不翻译）了，Let's get it。

*（以上参考自[该网站](https://www.456bereastreet.com/archive/200509/css_21_selectors_part_1/)）*

<!--more-->

##### 基本选择器

| 选择器 | 含义                 |
| ------ | -------------------- |
| *      | 通配                 |
| E      | 匹配所有标签E        |
| .class | 匹配所有class类      |
| #id    | 匹配所有id为id的元素 |

```css
*{
	margin:0;
	padding: 0;
}
body{
	color: black;
}
.box{
	width: 100%;
}
#section1{
	color: red;
}	
p.para{
    color:red;
}
p#para{
    color:red;
}
```

##### 多元素组合选择器

| 选择器 | 含义                                         |
| ------ | -------------------------------------------- |
| E,F    | 匹配所有E，F标签元素，不同标签用逗号隔开     |
| E F    | 后代元素选择器，匹配所有属于E元素后代的F元素 |
| E > F  | 子元素选择器，匹配所有E元素的子元素F         |
| E + F  | 毗邻选择器，匹配所有紧随E元素之后的同级元素F |

```css
.title h3{
	color:blue;
}
.title > h3{
 font-style: italic;
}
.paper,.footer{
	background-color: orange;
}
.header + div{
	font-style: italic;
}
```

##### 属性选择器

| 选择器       | 含义                                                         |
| ------------ | ------------------------------------------------------------ |
| E[att]       | 匹配所有具有att属性的E元素，不考虑它的值                     |
| E[att=val]   | 匹配所有att属性等于val的E元素                                |
| E[att~=val]  | 匹配所有att属性具有多个空格分割的值、其中一个值等于”val“的E元素 |
| E[att\|=val] | 匹配所有att属性具有多个连字号分隔的值，其中一个值以”val“开头的E元素 |

