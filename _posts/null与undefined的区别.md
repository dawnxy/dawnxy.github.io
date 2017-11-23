---
title: null与undefined的区别
date: 2016-11-13 18:44:20
categories: JavaScript
---

null和undefined都代表没有，但是：
- null是个空对象指针，表示属性存在，值不存在
- 而undefined表示未定义，连属性都不存在

```js
console.log(document.parentNode);//null  因为一个页面中的document已经是最顶级元素了,它没有父亲
console.log(document.parentnode);//undefined  因为没有parentnode属性
```

# null出现的场景
- 设定一个变量,后期需要使用,那么前期我们设置默认值为null
```javascript
var timer=null;
timer=setInterval(function(){
	console.log(1);
},1000);
```

- 在JS内存释放中，我们想释放一个堆内存，就让其值变为null即可
```javascript
var obj={name:"xy"};
obj=null
```

- 我们通过DOM中提供的属性和方法获取页面中的某一个元素标签，如果当前这个标签不存在，获取的结果是null，而不是undefined
```javascript
var oDiv=document.getElementById("div");
console.log(oDiv);//如果这个元素不存在，返回null
```

- 在正则的exec捕获，或者字符串的match捕获中,如果当前要捕获的字符串和正则不匹配的话，捕获到的结果为null
```javascript
var reg=/\d+?/g;
var str="xy";
console.log(reg.exec(str));//null
console.log(str.match(reg));//null
```

- 原型链的终点是null
```javascript
console.log(Object.prototype.__proto__);//null
```

# undefined出现的场景
- 在JS预解释的时候，只声明未定义，默认的值是undefined
```javascript
var a;
console.log(a);//undefined
```

- 在一个函数中，如果没有写return，或者return后啥都没返回，默认的返回值是undefined
```javascript
function fn(){
}
var res=fn();
console.log(res);//undefined
```

- 函数中设置了形参，但是执行的时候如果没有传递参数值，那么形参默认值是undefined
```javascript
function say(a,b,c) {
	console.log(a,b,c);//1 2 undefined
}
say(1,2);
```

- 获取一个对象的属性名对应的属性值，如果当前的这个属性名不存在的话，属性值默认是undefined,我们也应用这个道理来检测当前的浏览器是否兼容某一个方法
```javascript
var obj={};
console.log(obj.name);//undefined
```