---
title: vue_1_数据绑定和第一个Vue应用
date: 2019-03-26 17:27:40
tags:
 - javaScript
 - vue.js
---

### 数据绑定和第一个Vue应用

- el:指定页面中已存在的DOM元素来挂在Vue实例，可以说htmlele，也可以是css选择器
- v-model:数据双向绑定

```js
<!DOCTYPE html>
<html>
<head>
	<title>hello world</title>
</head>
<body>
	<div id="app">
		<input type="text" v-model="name" placeholder="yourname">
		<h1>你好{{name}}</h1>
	</div>
	<script  src="https://unpkg.com/vue@2.6.10/dist/vue.min.js"></script>
	<script>
		var app = new Vue({
			el:'#app',
			data:{
				name: ''
			}
		})
		console.log(app.name);
	</script>
</body>
</html>
```

<!--more-->

#### 生命周期

- created：实例创建完成后调用，完成了数据的观测，但**尚未挂载**,$el还不可用。
- mounted：el挂载到实例上后调用
- beforeDestory：实例销毁前调用，主要用于解绑监听事件等
- 这些钩子作为选项写入Vue实例内，且钩子的this指向Vue实例

#### 插值与表达式

```js
<!DOCTYPE html>
<html>
<head>
	<title>插值与表达式</title>
</head>
<body>
	<div id="app">
		{{ date }}
		<span v-html="link">这是一个连接</span>
		<span v-pre>{{不编译}}</span>
		<div clear='both'></div>
		<span>{{10/2}} {{isOk?'确定':'取消'}}{{text.split(',').reverse().join(',')}}</span>
	</div>
	<script  src="https://unpkg.com/vue@2.6.10/dist/vue.min.js"></script>
	<script>
		var app=new Vue({
			el:'#app',
			data:{
				date:new Date(),
				link:'<a href="www.baidu.com">百度</a>',
				isOk:true,
				text:'123,456'
			},
			mounted:function(){
				var _this = this;
				this.timer = setInterval(function(){
					_this.date = new Date();
				},1000);

			},
			beforeDestory:function(){
				if(this.timer){
					clearInterval(this.timer);
				}
			}
		})
	</script>
</body>
</html>
```

#### 过滤器

```
支持在{{}}插值的尾部添加一个管道符“|”对数据进行过滤，常用于格式化文本等功能，通过给Vue实例添加选项filters来设置。
```

```js
<!DOCTYPE html>
<html>
<head>
	<title>过滤器管道符</title>
</head>
<body>
	<div id='app'>
		{{date|formatDate}}
	</div>
		<script  src="https://unpkg.com/vue@2.6.10/dist/vue.min.js"></script>
		<script>
			var padDate = function(value){
				return value < 10?'0'+value:value;
			}
			var app = new Vue({
				el:'#app',
				data:{
					date:new Date()
				},
				filters:{
					formatDate:function(value){
						var date = new Date(value);
						var year = date.getFullYear();
						var month = padDate(date.getMonth()+1);
						var day = padDate(date.getDate());
						var hours = padDate(date.getHours());
						var minutes =padDate(date.getMinutes());
						var second = padDate(date.getSeconds());
						return year+'-'+month+'-'+day+' '+hours+':'+minutes+':'+second;
					}
				},
				mounted:function(){
					var _this = this;
					this.timer = setInterval(function(){
						_this.date = new Date();
					},1000);
				},
				beforeDestory:function(){
					if(this.timer){
						clearInterval(this.timer);	
					}
				}
			})
		</script>
</body>
</html>
```

#### 指令与事件

```js
<!DOCTYPE html>
<html>
<head>
	<title>指令与事件</title>
</head>
<body>
	<div id="app">
		<p v-if="show">显示这段文本</p>
		<button v-on:click="handleClose">点击隐藏</button>
			<button @click="handleClose">点击隐藏</button>
		<p>数据驱动DOM是VUE.js的核心理念，万不得已不要主动操作DOM，只需要维护好数据</p>
		<p>v-bind，动态更新html元素上的属性</p>
		<p>v-bind，可以省略v-bind,直接写一个冒号</p>
		<a v-bind:href="url">链接</a>
		<img :src="imgUrl">
		<p>v-on，绑定事件监听器</p>
		<p>v-on，可以省略v-on，直接写一个@</p>
	</div>
	<script  src="https://unpkg.com/vue@2.6.10/dist/vue.min.js"></script>
	<script type="text/javascript">
		var app = new Vue({
			el:'#app',
			data:{
				show:true,
				url:'https://www.github.com',
				imgUrl:'https://raw.githubusercontent.com/vuejs/vue-devtools/dev/media/screenshot-shadow.png'
			},
			methods:{
				handleClose:function(){
				  this.show?this.show = false:this.show =true;
				}
			}
		})
	</script>
</body>
</html>
```

