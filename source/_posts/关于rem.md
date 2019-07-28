---
title: 关于rem
date: 2019-05-09 15:38:54
tags:
- CSS
- web性能优化
---

### 关于rem

rem(font size of the root element)

- 字体单位

  值根据html根元素大小而定，也可作为宽度、高度等单位

- 适配原理

  将px修改为rem，动态修改html的font-size做适配

- 兼容性

  基本覆盖所有流行的手机系统

1 rem默认情况下大小等于浏览器默认font-size(16px)

通过设置html即root ele的font-size可达到动态修改rem的效果

```html
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
	<style>
		html{
			font-size: 17px;
		}
		.box{
			width: 10rem;
			height: 10rem;
			background-color: red;
		}
		.text{
			color: #fff;
		}
		/*
		* 1rem = 16px(default)===html font-size
		* 10rem = 160px
		*/
	</style>
</head>
```

**mediaQuery与rem**

```css
@media screen and (max-width: 320px){
			html{
				font-size: 18px;
			}
		}
@media screen and (max-width: 360px) and (min-width: 321px){
				html{
					font-size: 20px;
				}
}
```

**js与rem** 

```js
		//视窗宽度
		let htmlWidth = document.documentElement.clientWidth||document.body.clientWidth;
		console.log(htmlWidth);
		//获取视窗高度
		let htmlDOM = document.getElementsByTagName('html')[0];

		htmlDOM.style.fontSize = htmlWidth / 10 +'px'
```

**sass** 

```css
@function px2rem($px){
    $rem : 37.5px; //ipone6
    @return ($px / $rem ) + rem;
}
```

