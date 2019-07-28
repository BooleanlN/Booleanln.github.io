---
title: vue-router实现原理
date: 2019-06-20 13:34:03
tags:
- vue
- javaScript
---

### vue-router实现原理简述

了解vue-router实现原理，就要先明白前端路由的两种常见方式：

- Hash路由
- History

##### hash路由

hash路由，是前端实现路由的方式之一，被应用于vue、react的路由当中

hash路由的实现主要借助onhashchange的监听方法，window.location.hash可以对锚值进行设置读取

如果浏览器不支持onhashchange，可通过拿到location的href、hash，并通过setInterval对该属性进行监听，若发生变化则通过自定义方法进行处理

<!--more--->

###### hash路由实例

```js
const routes = [{
			path: '/0',
			template: '<div>0000</div>'
		},{
			path: '/1',
			template: '<div>11111</div>'
		}, {
			path: '/2',
			template: '<div>222222</div>'
		}, {
			path: '/3',
			template: '<div>3333333</div>'
		}
		]
		var mount = document.getElementById('root')
		console.log(mount.parentNode)
		var list = document.getElementsByClassName('route')
		for (var i = 0; i < list.length ; i++) {

			list[i].onclick = (function(i){
				return function(){
					window.location.hash = `/${i}`
				}
			})(i)
		}
		window.addEventListener('hashchange', e => {
			console.log(e.newURL.split('#')[1])
			var path = e.newURL.split('#')[1]
			var item = routes.find(item => {
				return item.path == path
			})
			mount.innerHTML = item.template
		})
```

###### 处理兼容性

https://developer.mozilla.org/zh-CN/docs/Web/API/HashChangeEvent

##### History对象

History对象包含用户在浏览器窗口内访问过的URL，Histroy对象是window对象的一部分，可通过window.histroy进行访问，是BOM对象

- history.forward()前进一步
- history.back() 后退一步
- history.go(n)前进n步，n为负值时，相当于后退
- history.pushState(state,title,url)保存state值，该方法会向history中push一条记录
- history.replaceState(state,title,url) 替换当前历史记录

onpopstate方法在浏览器前进、后退操作时会触发，执行下载也会触发，pushstate、replacestate不会触发

```js
	window.addEventListener('popstate', e =>{
                console.log(e)
		})
```

##### vue-router实现

vue-router支持三种模式的路由方式，分别是history、hash以及abstract

history模式通过H5的History对象实现，主要通过监听popstate，实现路由

hash模式通过监听hashchange实现路由

abstract主要用于非浏览器环境当中，如node

