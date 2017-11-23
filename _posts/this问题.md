---
title: this问题
date: 2016-12-14 18:44:20
categories: JavaScript
---

什么是this？
> js中的this代表的是当前行为执行的主体

# 如何区分this?
this是谁和函数在哪定义的和在哪执行的都没有任何关系
- 函数执行，首先看函数名之前是否有点，有的话，点前面是谁，this就是谁，没有的话，this就是window
```js
var obj={
    fn:function(){
        console.log(this);//this -->obj
    }
};
obj.fn();
```

- 自执行函数中的this永远是window
```js
;(function(){
    console.log(this);//this -->window
})();
```

- 给元素的某一个事件绑定方法，当事件触发的时候，执行对应的方法，方法中的this是当前的元素
```js
var oDiv = document.getElementById('div');
[DOM零级事件绑定]
  oDiv.onclick=function(){
     //this->oDiv
  };
[DOM二级事件绑定]
  oDiv.addEventListener("click",function(){
     //this->oDiv
  },false);
//在IE6~8下使用attachEvent
  oDiv.attachEvent("click",function(){
       //this->window
  });
```

- 在构造函数模式中,我们的this.xxx=xxx中的this是当前的类的一个实例
```
function Fn(){
    this.x=100;//this->f x是给当前实例f增加的私有的属性
}
Fn.prototype.getX=function(){
    console.log(this.x);
};
var f=new Fn;
f.getX();//getX中的this->f
f.__proto__.getX();//getX中的this->Fn.prototype
```

- call和apply还有bind都可以强制改变this
一般情况下,我们执行call方法第一个传递的参数值是谁,那么fn中的this就是谁
[在非严格模式下]
第一个参数没有传递值或者传递的是null/undefined，fn中的this都是window
[严格模式下]
第一个参数传递的是谁，this就是谁,传递null/undefined，fn中的this都是对应的null/undefined,不传递值默认也是undefined
- 定时器中的this指window
- 回调函数中的this指window