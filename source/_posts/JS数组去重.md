---
title: JS数组去重
date: 2019-06-24 21:56:29
tags:
- javaScript
- 面试
---

废话不多说，直接上代码！

```js
let arr = [1,34,21,41,22,23,7,3,9,10,1,1,1,1]
// 排序去重
arr.sort((a,b)=> {return a-b})
console.log(arr)
let tmp = [arr[0]]
for(let i=1;i<arr.length;i++){
	if (arr[i] !== tmp[tmp.length-1] ) {
		tmp.push(arr[i])
	}
}
console.log(tmp)
//indexOf去重
arr = [1,34,21,41,22,23,7,3,9,10,1,1,1,1]
let res = []
for(let i = 0;i<arr.length;i++){
	if(res.indexOf(arr[i]) === -1){
		res.push(arr[i])
	}
}
console.log('index',res)
//splice去重
arr = [1,34,21,41,22,23,7,3,9,10,1,1,1,1]
for(let i = 0;i<arr.length;i++){
	for(let j=i+1;j<arr.length;j++){
		if(arr[i] === arr[j]){
			arr.splice(j,1)
			j-- // splice后，数组向后缩
		}
	}
}
console.log('splice',arr)
//indexOf+filter
arr = [1,34,21,41,22,23,7,3,9,10,1,1,1,1]
let result  = arr.filter((item,index,arr)=>{
	return arr.indexOf(item,0) === index
})
console.log('index+filter',result)
//ES6去重
arr = [1,34,21,41,22,23,7,3,9,10,1,1,1,1]
let result = Array.from(new set(arr))
console.log('set',result)
```
