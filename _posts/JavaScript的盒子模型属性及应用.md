---
title: JavaScript的盒子模型属性及应用
date: 2016-11-14 18:44:20
categories: JavaScript
---

# js盒子模型属性
## client系列
clientWidth：内容宽度+左右padding

clientHeight：内容宽度+上下padding

clientLeft：左边框的宽度

clientTop：上边框的宽度

## offset系列
offsetWidth：clientWidth+左右border

offsetHeight：clientWidth+上下border

offsetParent：返回最近的祖先定位元素。offsetParent的值，要取决于它的上一级节点是否包含定位属性。这个定位属性的值必须是relative,absolute,fixed中的一种，如果上一级不包含定位属性，那么再继续向上查找，如果查找到body仍然没有，那么默认就是body

offsetLeft：当前元素的外边框距离offsetParent的内边框的偏移量（水平方向）

offsetTop：当前元素的外边框距离offsetParent的内边框的偏移量（垂直方向）

ps:由于offsetParent会随着定位属性变化，那么offsetLeft/offsetTop的值也会随之变化
    body.offsetParent=null
    document.parentNode=null

## scroll系列
scrollWidth：如果有内容溢出，==>左padding+内容宽度，没有内容溢出，==》内容宽度+左右padding

scrollHeight：如果有内容溢出，==>上padding+内容高度，没有内容溢出，==》内容宽度+上下padding

ps：如果没有内容溢出，和clientWidth/clientHeight相同

scrollLeft：内容区域滚出去的宽度

scrollTop：内容区域滚出去的高度

总结：所有盒子模型属性，只有scrollLeft和scrollTop支持赋值

# js盒子模型属性的应用
注意：以下两个案例都要引入utils-1.0.js文件
## 回到顶部
```html
//html
<a href="javascript:void 0;" id="goTop">GO</a>
```

```css
//css
*{ margin:0; padding:0;}
html,body{ width:100%; height:300%;}
body{ background: -webkit-linear-gradient(top,lightgreen,lightsalmon,lightblue,lightgoldenrodyellow,lightskyblue);}
a,a:hover{ text-decoration: none;}
#goTop{ position: fixed; bottom:50px; right:30px; display: block; width:60px; height:60px; background: #000; color: #fff; font:bold 24px/60px "微软雅黑"; border-radius: 50%; text-align: center; display: none;}
```

```js
//js
var goTop=document.getElementById("goTop");
goTop.onclick=function () {
    this.timer&&clearInterval(this.timer);
    var curTop=utils.win("scrollTop"),_this=this;
    this.timer=window.setInterval(function () {
        if(curTop<=0){
            clearInterval(_this.timer);
            window.onscroll=fn;
            return;
        }
        curTop-=100;
        utils.win("scrollTop",curTop);
    },10);
    this.style.display="none";
    window.onscroll=null;
};
function fn() {
    var curTop=utils.win("scrollTop");
    var screenHeight=utils.win("clientHeight");
    if(curTop>screenHeight){
        goTop.style.display="block";
    }
}
window.onscroll=fn;
```

## 瀑布流
```html
//html
<ul></ul>
<ul></ul>
<ul></ul>
```

```css
//css
*{ margin:0; padding:0;}
ul,li{ list-style: none;}
ul{ float: left; width:30%;}
ul:nth-child(2){ margin:0 5%;}
```

```js
//js
var oUls=document.getElementsByTagName("ul");
oUlsAry=utils.listToArray(oUls);
function liappendToUl() {
    for(var i=0;i<50;i++){
        oUlsAry.sort(function (curUl,nextUl) {
            return utils.getCss(curUl,"height")-utils.getCss(nextUl,"height");
        });
        var shortUl=oUlsAry[0];
        var li=createLi();
        shortUl.appendChild(li);
    }
}
liappendToUl();
//创建59个随机高度和随机背景色的li
function createLi() {
    var li=document.createElement("li");
    li.style.height=utils.getRandom(100,250)+"px";
    li.style.background="rgb("+utils.getRandom(0,255)+","+utils.getRandom(0,255)+","+utils.getRandom(0,255)+")";
    return li;
}
window.onscroll=function () {
    var curTop=utils.win("scrollTop");
    var winH=utils.win("clientHeight");
    var conH=utils.win("scrollHeight");
    if(curTop>conH-winH-100){
        liappendToUl();
    }
}
```