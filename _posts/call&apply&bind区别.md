---
title: call&apply&bind区别
date: 2016-12-14 18:44:20
categories: JavaScript
---

# call
> 修改函数中的this，并让函数执行，第二个参数开始，是依次传递给要执行的函数的

**语法**：fn.call(obj,num1,num2...);

**执行过程**：
- fn通过自己的__proto__属性找到定义在Function.prototype上的call方法
- 把fn函数中的this修改成了call的第一个参数
- fn执行

```js
function fn() {
    console.log(this);
}
function fn2(){
    console.log(2);
}
var obj={
    name:'xy',
    fn:fn
}
fn();//this-->window
obj.fn();//this-->obj
fn.call(obj);//fn通过call方法将fn中的this改为obj，并且fn执行
fn.call.call(document.body);//--->将fn.call方法中的this修改为document.body==>报错：fn.call.call is not a function
fn.call.call(fn2);//2  ===>fn2执行
```

**一个call与多个call**：
- 一个call是fn执行
- 如果是多个call方法连用，那么是最后一个call方法的第一个参数执行，如果这个参数不是函数会报错
- 能不能找到call，就看是不是一个函数

# apply
> 与apply方法基本相同，都是用来修改this关键字的
不同点：传给调用apply的那个函数实例参数的方式不同，apply第二个参数是一个数组，是把数组里的每一项当做参数传给调用apply的函数的

**语法**：fn.apply(obj,[num1,num2...]);
```js
function sum(num1,num2){
    console.log(num1+num2);
}
sum.apply(null,[1,2]);//3
```

# bind
> 返回一个修改了this的没有执行的新函数，需要自己执行。不兼容ie6-8

**语法**：fn.bind(obj,num1,num2....);
```js
function sum(num1,num2){
    console.log(num1+num2);
}
var res=sum.bind(null,1,2);//什么也不输出，只是修改了this而已  返回结果：调用了它的那个函数
console.log(res);//function sum(num1,num2){console.log(num1+num2);}
```

# 开启严格模式解析代码
- call,apply,bind修改this的时候，在非严格模式下，如果修改成null或undefined，浏览器会默认修改成window，在严格模式下，null和undefined不变。
```js
"use strict";
var obj={
    name:'xy',
    fn:function () {
        console.log(this);
    }
};
obj.fn();//obj
obj.fn.call(null);//null  非严格模式是windwo
obj.fn.call(undefined);//undefined  非严格模式是windwo
obj.fn.call();//undefined  非严格模式是windwo
```

- 在严格模式下，自运行函数中的this不是window，而是undefined
```js
"use strict";
(function () {
    console.log(this);
})();
```

- 在严格模式下，函数执行的时候如果前面没有.，那么函数中的this默认是undefined，在非严格模式下是window
```js
"use strict";
function fn() {
    console.log(this);
}
fn();//undefined
```

# call,apply,bind的区别？
**相同点**：
- 都是修改this

**不同点**：
- call,apply函数执行完了，bind返回没有执行的函数
- call和apply传参方式不同，call传的是参数列表，apply传的是数组