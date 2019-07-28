---
title: 跨域解决
tags:
  - Html
url: 148.html
id: 148
categories:
  - Html是程序语言乎？
date: 2018-07-13 14:34:41
---

###### 跨域

浏览器执行javascript脚本时，会检查这个脚本属于哪个页面，如果不是同源页面，就不会被执行。

解决方法： **1.JSONP：**原理：ajax请求受同源策略影响，不允许进行跨域请求，而script标签src属性中的链接却可以访问跨域的js脚本，利用这个特性，服务端不再返回JSON格式的数据，而是返回一段调用某个函数的js代码，在src中进行了调用，这样实现了跨域。[https://blog.csdn.net/u011897301/article/details/52679486](https://blog.csdn.net/u011897301/article/details/52679486) **2.代理** 3.**PHP端修改header（XHR2方式）** 在php接口脚本中加入以下两句即可： header('Access-Control-Allow-Origin:*');//允许所有来源访问 header('Access-Control-Allow-Method:POST,GET');//允许访问的方式 **4.CORS** 　CORS(Cross-Origin Resource Sharing, 跨源资源共享)是W3C出的一个标准，其思想是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。因此，要想实现CORS进行跨域，需要服务器进行一些设置，同时前端也需要做一些配置和分析。 服务器端对于`CORS`的支持，主要就是通过设置`Access-Control-Allow-Origin`来进行的。如果浏览器检测到相应的设置，就可以允许`Ajax`进行跨域的访问。 分为简单请求（GET,HEAD.POST；且头信息只包含Accept，Accept-Lanuage，Content-Language，Content-Type（x-www-form-urlencoded，form-data，plain））和非简单请求 简单请求，浏览器与服务器只进行一次

非简单请求，要先进行预检请求