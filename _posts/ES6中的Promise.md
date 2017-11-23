---
title: ES6中的Promise
date: 2017-02-22 18:44:20
categories: ES6
---

在需要多个操作的时候，会导致多个回调函数嵌套，导致代码不够直观，就是常说的回调地狱

# Promise的含义
所谓Promise，就是一个对象，用来传递异步操作的消息，它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的API，可供进一步处理

# Promise对象的特点
**Promise的状态不受外界影响**
Promise对象代表一个异步操作，有三种状态：
- Pending(进行中)
- Resolved(已完成)
- Rejected(已失败)
只有异步操作的结果才可以决定当前是哪一种状态，其他任何操作都无法改变这个状态
**一旦状态改变，就不会再变**
Promise对象的状态改变只有两种可能：
- Pending--->Resolved
- Pending--->Rejected
只要其中之一发生，状态就凝固了，不会再变

# 基本用法
ES6规定，Promise对象是一个构造函数，用来生成Promise实例
```js
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (err{//异步操作失败
    reject(error);
  } else {//异步操作成功
    resolve(value);
  }
});
```

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定Resolved状态和Reject状态的回调函数。
```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为Resolved时调用，第二个回调函数是Promise对象的状态变为Reject时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。

实例：
```js
var fs=require('fs');
//当创建promise实例的时候，里面的函数会立刻执行
var promise=new Promise(function (resolve,reject) {
    //可以在函数内写一个异步的任务
    fs.readFile('1.txt','utf8',function (err,result) {
        if(err){//如果错误对象有值，则认为是失败了，那么调用reject把此promise标记为失败
            reject(err);
        }else{//否则调用resolve表示此任务成功
            resolve(result);
        }
    })
});
```

```js
readFile('1.txt').then(function (data) {
    return readFile(data);
}).then(function (data) {
    return readFile(data);
});
```