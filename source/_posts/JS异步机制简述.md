---
title: JS异步机制简述
date: 2019-06-22 17:43:02
tags:
- javaScript
- 面试
---

### JS异步机制简述

众所周知，javaScript引擎是单线程运行的，这就带来一个问题，当遇到一些执行时间较长的事件或者某些需要在特定时间点触发的事件时，就需要阻塞正在执行的代码，这显然是不可以的。

所以，在js引擎当中，同步任务，会在执行栈中，由主线程顺序执行，而异步任务，如ajax、定时器、回调函数等，会添加至异步队列当中，当条件满足时，该任务会进入主线程中执行。

<!--more-->

![](https://upload-images.jianshu.io/upload_images/5452485-99f606742b17f745.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/444/format/webp)

不同的异步操作，会交由浏览器内核的webcore来执行，如DOM Binding、timer、network模块

######  添加到主线程任务队列的时机

不久前，经历了一次头条的技术面试，与面试官相谈还算欢，面了我大概一个小时四十分钟，虽然没了后文（哭，不知道那里不满意），但也有了很多收获。

比如本篇文章，就是因为面试中遇到的一个问题，所以才记录的，现在将它放在这里：

```js
new Promise((resolve, rej) => {
            console.log(1)
            resolve()
    }).then(() => {
        console.log(2)
    })

    setTimeout(() => {
        console.log(3)
    }, 0)

    console.log(4)

```

请回答出该问题的输出：

这道题，我回答结果是错误的，面试官也指出了这是宏任务和微任务的区别，所以今天来捋一捋。

首先，我们将代码中的同步、异步任务分开：

同步有：

- new Promise(fun)

- console.log(4)

异步有：

- resolve、reject回调函数
- setTimeout函数

由于同步任务必然在异步任务之前，所以会输出 1、4

关键是要搞清楚setTimeout与promise回调的输出顺序。

在javascript当中，异步任务队列又被分为微任务与宏任务两种

**宏任务队列**包括setTimeout、setInterval、**requestAnimationFrame**、NodeJS中的I/O等。

**微任务队列**包括独立回调（如Promise）和复合回调（即不同状态回调在同一函数体内）两类。

JavaScript完整执行顺序：

1. 顺序执行所有同步代码
2. 检查宏任务队列，若有触发的宏任务，则取第一个并调用其事件处理函数，然后转至第三步，若没有触发的宏任务，直接跳转至第三步
3. 检查微任务队列，执行该队列中所有已触发的异步任务，然后跳转至第二步，若无已触犯的异步，则直接返回第二步
4. 最后返回第二步，继续检查宏任务队列
5. 循环往复，直到所有的异步任务处理完成。

所以上面的异步任务会先执行宏任务setTimeout，再执行微任务resolve

![](https://user-gold-cdn.xitu.io/2018/8/12/1652ed68a7bda9df?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

[参考链接](https://juejin.im/post/5b7057b251882561381e69bf)



