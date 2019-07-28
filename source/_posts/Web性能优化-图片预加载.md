---
title: Web性能优化-图片预加载
date: 2019-05-03 17:27:04
tags:
 - javaScript
 - web性能优化
---

### Web性能优化-图片预加载

当网站存在大量图片时，由于需要进行大量的请求，如果不对这部分进行优化，就会出现图片死区等影响用户体验的情况。

通过图片预加载技术对Web图片请求进行优化，可以提前加载所需图片，带给用户更好的用户体验。

图片预加载可分为：

- 无序预加载：图片无顺序要求，qq表情插件
- 有序预加载：有顺序要求，漫画

<!--more-->

#### 实例1：图片相册

!['图片演示']('/img/preload1.png')

实现效果：浏览相册之前，将相册内所有图片资源加载完成

**jquery**

```js
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

```javascript
var imgs = ["https://cn.bing.com/th?id=OHR.RuffLek_ZH-CN8485019267_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://cn.bing.com/th?id=OHR.MargaretRiverVineyards_ZH-CN8547374435_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://cn.bing.com/th?id=OHR.may1_ZH-CN8582006115_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp",
					"https://cn.bing.com/th?id=OHR.GlenfinnanViaduct_ZH-CN8400951216_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp"
					];
var index = 0,
	len = imgs.length,
	count = 0,
	$progress = $('.progress');
		//不采用插件,关键代码
$.each(imgs,function(i,src){
			var imgObj = new Image();
			$(imgObj).on("load error",function(){
				$progress.html(Math.round((count+1)/len * 100)+'%');
				if(count>=len-1){
					$('.loading').hide();
					document.title = '1/'+len;
				}
				count++;
			});
			imgObj.src = src;
		});
//按钮事件
$(".btn").on("click",function(){
			if($(this).data('control')==='prev'){
				index = Math.max(0,--index);
			}else{
				index = Math.min(len-1,++index);
			}
			document.title = (index+1)+'/'+len;
			console.log(imgs[index]);	
			$('#img').attr('src',imgs[index]);
		});
```

**完成为jquery插件，复用**

```js
(function($){
	function PreLoad(imgs,options){
		this.imgs = (typeof imgs === 'string')?[imgs]:imgs;
		this.opts = $.extend({},PreLoad.DEFAULTS,options);
		this._unordered();
    }
    PreLoad.DEFAULTS = {
		each:null,//每张图片加载完成后执行
		all:null,//所有图片加载完成后执行
		order:'unordered'
	};
    PreLoad.prototype._unordered = function(){ //无序预加载
		var imgs = this.imgs,
			count = 0,
			opts = this.opts,
			len = imgs.length;

		$.each(imgs,function(i,src){
			if(typeof src!='string')return;
			var imgObj = new Image();
			$(imgObj).on("load error",function(){
				opts.each && opts.each();
				if(count>=len-1){
					opts.all && opts.all();
				}
				count++;
			});
			imgObj.src = src;
		})
	};
	$.extend({
		preload:function(imgs,opts){
			new PreLoad(imgs,opts);
		}
	})
})(jQuery);
```

**改写为插件形式**

```js
$.preload(imgs,{
			each:function(count){
				$progress.html(Math.round((count+1)/len * 100)+'%');
			},
			all:function(){
				$('.loading').hide();
		 		document.title = '1/'+len;
			}
		});
```

#### 案例2：无序加载QQ表情插件

[实现效果](https://www.helloweba.net/demo/qqface/)

点击表情可出现qq表情，但要求在qq表情完全加载结束前，不出现表情

**完整代码** 

```js
<!DOCTYPE html>
<html>
<head>
	<title>图片预加载无序加载-QQ表情</title>
	<style type="text/css">
		body,p,li,ul{
			padding: 0;
			margin: 0;
		}
		body{
			background-color: #eee;
		}
		a{
			text-decoration: none;
		}
		.box{
			margin:150px 0 0 200px;		
		}
		#face-btn{
			display: block;
			background: url('source/qq/icon.gif') no-repeat 0 4px;
			color: #333;
			text-indent: 20px;
		}
		#face-btn:hover{
			background-position: 0 -26px;
		}
		.panel{
			display: none;
			width: 390px;
			padding: 2px;
			border:1px solid #ccc;
			background-color: #fff;
		}
		.loading{
			text-align: center;
		}
		li{
			list-style: none;
		}
		.list li{
			border: 1px solid #fff;
			display: inline-block;
			width: 24px;
			height: 24px;
			margin-bottom: 5px;
			cursor: pointer;
		}
		.list li:hover{
			border-color: #333;
		}
	</style>
</head>
<body>
	<div class="box">
		<a href="javascript:;" id="face-btn">表情</a>
		<div class="panel">
			<p class="loading"> 表情加载中。。。</p>
		</div>
	</div>
	<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
	<script type="text/javascript" src="js/preload.js"></script>
	<script type="text/javascript">
		var $btn = $('#face-btn'),
			$panel = $('.panel');
		var imgs = [];
		for(var i=0;i<75;i++){
			imgs[i] = 'source/qq/'+(i+1)+'.gif';
		}
		var len = imgs.length;
		$btn.on('click',function(e){
			e.stopPropagation();//禁止事件冒泡
			$panel.show();
			$.preload(imgs,{
				all:function(){
					var html = '';
					html+='<ul class="list">';
					for(var i=0;i<len;i++){
						html+='<li><img src="'+imgs[i]+'" alt=""></li>';
					}
					html+="</ul>";
					$panel.html(html);
				}
			})
		});
		$(document).on('click',function(){
			$panel.hide();
		})
	</script>
</body>
</html>
```

```python
#爬取所有emoij图片
import urllib.request
import re
import os
if __name__ == '__main__':
	os.chdir("F:\\备份\\JS\\特效\\source\\qq");
	url = "https://www.helloweba.net/demo/qqface/face/"
	for i in range(75):
		newurl = url+str(i+1)+'.gif'
		print(newurl)
		urllib.request.urlretrieve(newurl,str(i+1)+'.gif',None)
```

#### 案例3：漫画图片有序加载

**关键代码** 

```js
<div class="box">
		<img src="https://images.dmzj.com/img/chapterpic/20588/56056/14752173317542.jpg"  id="img" alt="pic">
		<p>
			<button data-control="prev" class="btn">上一页</button>
			<button data-control="next" class="btn">下一页</button>
		</p>
</div>
<script>
//不使用插件形式
var images = [
			"https://images.dmzj.com/img/chapterpic/20588/56056/14752173317542.jpg",
			"https://images.dmzj.com/img/chapterpic/20588/56056/14752173323614.jpg",
			"https://images.dmzj.com/img/chapterpic/20588/56056/14752173329716.jpg"
];//漫画图片
var index = 0,//按钮
	len = images.length,count=0;//count记录当前下载的漫画页数
load();
function load(){
	var imObj = new Image();
    //图片加载成功则继续load下一张
	$(imObj).on('load error',function(){
		if(count>=len){
					//图片都已加载完毕 
			}else{
				load();
			}
			count++;
		});
		imObj.src = images[count];//将url赋给src
	}
	$(".btn").on("click",function(){
			if($(this).data('control')==='prev'){
				index = Math.max(0,--index);
			}else{
				index = Math.min(len-1,++index);
			}
			document.title = (index+1)+'/'+len;
			console.log(images[index]);	
			$('#img').attr('src',images[index]);
		});
</script>
```

**插件内容添加有序加载** 

```js
function PreLoad(imgs,options){
		this.imgs = (typeof imgs === 'string')?[imgs]:imgs;
		this.opts = $.extend({},PreLoad.DEFAULTS,options);
		//根据参数选择加载方式
		if(this.opts.order==='ordered'){
			this._ordered();
		}else{
			this._unordered();
		}
	}
PreLoad.DEFAULTS = {
		each:null,//每张图片加载完成后执行
		all:null,//所有图片加载完成后执行
		order:'unordered'//标记有序还是无序加载，默认无序
	};
//添加一个原型私有方法	
PreLoad.prototype._ordered = function(){
			//有序预加载
		var opts = this.opts,
			count = 0,
			imgs = this.imgs,
			len = imgs.length;
		load();
		function load(){
			var imObj = new Image();
			$(imObj).on('load error',function(){
				opts.each&&opts.each();
				if(count>=len){
					//图片都已加载完毕 
					opts.add&&opts.all();
				}else{
					load();
				}
				count++;
			});
			imObj.src = images[count];
		}
	}
```

*上述代码是第一个案例中插件的修改部分，未写出部分则代表无修改*

**以上内容参考自慕课教程**

