---
title: 'cookies,sessionStorage和localStorage'
tags:
  - Html
url: 146.html
id: 146
categories:
  - Html是程序语言乎？
date: 2018-07-13 14:32:53
---

###### cookies,sessionStorage和localStorage

sessionStorage用于本地存储一个会话中的数据，这些数据只有在同一个会话的页面才能访问，并且会话结束后，数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。而localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。

webStorage的概念和cookie相似，区别是它是为了更大容量存储设计的。Cookie的大小是受限的，并且每次你请求一个新的页面的时候Cookie都会被发送过去，这样无形中浪费了带宽，另外cookie还需要指定作用域，不可以跨域调用。

除此之外，Web Storage拥有setItem,getItem,removeItem,clear等方法，不像cookie需要前端开发者自己封装setCookie，getCookie。但是Cookie也是不可以或缺的：Cookie的作用是与服务器进行交互，作为HTTP规范的一部分而存在 ，而Web Storage仅仅是为了在本地“存储”数据而生。