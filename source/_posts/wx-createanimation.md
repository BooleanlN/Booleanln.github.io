---
title: wx.createAnimation微信小程序动画接口与CSS3对比
url: 46.html
id: 46
categories:
  - 微信小程序
  - 编程收录
date: 2018-04-28 11:37:43
tags:
---

### **wx.createAnimation微信小程序动画接口**

首先创建一个动画实例：animation="{ {animation对象}}"

<image src='../images/bglottery.png'style='height:200px;width:200px;position:absolute;top:17%;left:25%;' animation="{ {an_zhuanp}}"></image>

调用微信小程序实例方法，描述动画，初始化animation

 var animation = wx.createAnimation({
 duration: 1000,
 timingFunction: "ease",
 });
 animation.rotate(360 * n).step();

wx.createAnimation参数列表：

*   duration Integer 默认值 400ms —动画持续时间
    
*   timing Function String 默认值“linear"
    
*   delay Integer 默认值 0 动画延迟时间
    
*   transformOrigin String 默认值”50% 50% 0“ 旋转的基点位置
    

timingFunction有效值：

*   linear 动画从头到尾速度是相同的
    
*   ease 动画从低速开始，加快、变慢
    
*   ease-in 从低速开始
    
*   ease-in-out 低速开始，低速结束
    
*   ease-out 低速结束
    
*   step-start 动画第一帧就跳至结束状态直到
    
*   step-end 动画一直保持开始状态，最后一帧跳至结束状态
    

animation方法列表： 样式：

*   opacity
    
*   backgroundColor
    
*   width
    
*   height
    
*   top
    
*   left
    
*   bottom
    
*   right
    

旋转：rotate 参数 deg（-180~180）

*   rotateX
    
*   rotateY
    
*   rotateZ
    
*   rotate3d
    

缩放：scale (sx,\[sy\])一个参数X轴缩放sx，两个参数：x轴缩放sx，y轴缩放sy

偏移：translate(tx,\[ty\])

倾斜：skew(ax,\[ay\]) 矩阵变形：matrix(a,b,c,d,tx,ty) 调用操作方法后需要调用step()方法来表示一组动画的完成。（step 可以传入一个跟 `wx.createAnimation()` 一样的配置参数用于指定当前组动画的配置。）

旋转实例：

 animation.rotate(360 * n).step();
 _this.setData({
 an_zhuanp: animation.export()
 });

例子：setInterval与setTime实现一个选择轮盘

var timer=setInterval(function(){
 var animation = wx.createAnimation({
 duration: 1000,
 timingFunction: "ease",
 });
 animation.rotate(360 * n).step();
 _this.setData({
 an_zhuanp: animation.export()
 });
 n++;
 },100);
 setTimeout(function(){
 clearInterval(timer);
 timer=null;
 var animation = wx.createAnimation({
 duration:1000,
 timingFunction:"ease",
 });
 var deg = 0;
 wx.request({
 url: 'https://www.booleanln.cn/net/getPrice.php',
 data:{
 phone:xtele,
 stunum:xh,
 name :xm,
 openid:openid
 },
 header: { "content-type": "application/www-form-urlencode" },
 method: 'get',
 success: function (res) {
 deg = res.data;
 console.log(deg);
 animation.rotate(deg).step();
 _this.setData({
 an_zhuanp: animation.export(),
 start:null
 });},3000}