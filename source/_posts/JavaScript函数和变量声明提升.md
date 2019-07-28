---
title: JavaScript函数和变量声明提升
date: 2019-05-13 22:05:05
tags:
	- javaScript
---

### JavaScript函数和变量声明提升

javascript面试题常常会考这个知识点的题

```javascript
a = 6;
var a = 3,b = 7;
function fn(){
	alert(a);
	function a(){
		a = 5;
	}
	a();
	alert(a);
}
fn();
alert(a);
```

js在运行过程中通常会经过三个阶段

- 语法解析——检查语法是否有错误
- 预编译
- 解释运行

函数和变量声明提升就发生在预编译阶段

<!--more-->

#### 预编译步骤

###### 函数

1. 创建AO（Active Object）对象;
2. 找形参和变量声明，将形参和变量名作为AO对象的属性，初始值为undefined
3. 将形参和实参统一
4. 在函数体内找函数声明，将函数名添加到AO对象的属性中，如果与第2步变量名重复，则覆盖

如在执行函数fn();时：

```js
//创建AO对象
AO = {}
//2
AO = {
    a:undefined
}
//3
AO = {
    a:undefined
}
//4
AO = {
    a:function a(){a = 5;}
}
```

###### 全局

1. 创建GO（global object）对象
2. 找变量声明
3. 找函数声明

```JS
GO = {}
GO = {
    a:undefined,
    b:undefined
}
GO = {
    a:undefined,
    b:undefined, 
    fn:function fn(){alert(a);
	function a(){
		a = 5;
	}
	a();
	alert(a);}
}
```

了解了两类执行环境预编译后，我们可以看下该题的整个执行流程

1. 首先创建GO对象，内容就是上面分析的那样

   ```js
   GO = {
       a:undefined,
       b:undefined,
       fn:function fn(){alert(a);
   	function a(){
   		a = 5;
   	}
   	a();
   	alert(a);}
   }
   ```

2. 然后开始解释执行

   `a = 6;`

   GO.a = 6;

   `var a=3,b=7;`

   GO.a = 3,GO.b = 7;

   `fn();` 

3. 执行fn()之前，会触发函数预编译

   创建AO对象，内容就是上面分析那样

   ```
   AO = {
       a:function a(){}
   }
   ```

4. 然后逐行解释执行fn函数体

   ```js
   alert(a); // function a(){a = 5;}
   a();
   alert(a);//a = 5
   ```

5. 执行完fn之后，返回全局环境

   ```js
   alert(a)//GO.a = 3;
   ```

这就是JS执行时的过程，总结起来为两点：

- 函数声明提升，且优先级高
- 变量声明提升，赋值不提升



