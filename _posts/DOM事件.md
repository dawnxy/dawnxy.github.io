---
title: DOM事件对象，DOM0&DOM2，事件兼容性处理
date: 2017-01-20 18:44:20
categories: JavaScript
---

# 事件对象及其属性
## 事件对象e
当事件被触发的时候，在绑定函数中打印arguments，浏览器默认给我们传了一个事件对象
```js
oDiv.onclick=function (e) {
    console.log(arguments);//[MouseEvent]
    console.log(e);//MouseEvent {isTrusted: true, screenX: 88, screenY: 126, clientX: 88, clientY: 60…}
}
```

在IE低版本中，这个事件对象不在arguments上，而是在window.event上

## 事件对象e的属性
### e.type
> 事件类型  例如："click"，注意没有前缀"on"

### e.target
> 事件源  ie6-8不兼容

**兼容性写法**：
```js
e.target=e.target||e.srcElement
```

### e.stopPropagation()
> 阻止事件冒泡  ie6-8不兼容

**兼容性写法**：
```js
//方法一：
e.stopPropagation?e.stopPropagation():e.cancelBubble=true;

//方法二：
e.stopPropagation=e.stopPropagation||function () {
    e.cancelBubble=true;
}
```

### e.preventDefault()
> 阻止默认行为，例如a链接的跳转行为  ie6-8不兼容

**兼容性写法**：
```js
//方法一：
e.preventDefault?e.preventDefault():e.returnValue=false;

//方法二：
e.preventDefault=e.preventDefault||function () {
    e.returnValue=false;
}

//方法三：
return false;
```

**默认行为有哪些**？
- a标签的默认跳转
```js
//阻止a标签的默认跳转行为(3种方式)
<a href="javascript:;"></a>
<a href="javascript:void 0;"></a>
<a href="javascript:void 1;"></a>
```

- form表单的提交
- F1-F12部分功能键

**兼容性写法**：
```js
e=e||window.event;
```

**注意**：
- 事件对象e只存在真正绑定的那个函数中

## 鼠标事件对象（MouseEvent）
## e.clientX/e.clientY
> 鼠标点击距离"浏览器"左上角的X/Y坐标值。

## e.pageX/e.pageX
> 鼠标点击位置距离"页面"的X/Y坐标。在ie6-8中有兼容性问题

**兼容性写法**：
```js
e.pageX=e.pageX||(e.clientX+document.documentElemnt.scrollLeft||document.body.scrollLeft);
e.pageY=e.pageX||(e.clientY+document.documentElemnt.scrollTop||document.body.scrollTop)
```

## 键盘事件对象(KeyboardEvent)
## e.keyCode
Enter:13
Left:37
Top:38
Right:39
Bottom:40
Space:32
Backspace:8
0-9数字:48-57

# 事件传播
事件发生的顺序：
- **捕获阶段**：从外向里查找元素  html-->body-->...
- **目标阶段**：当前事件源本身的操作
- **冒泡阶段**：从内到外依次触发相关的行为（常用）....-->body-->html

什么是冒泡？
> 当事件发生在子元素上，那么在触发子元素的事件时，还会继续向上，把子元素所有的祖先元素的事件分别触发。

**冒泡传播实例**：
```html
<div id="outer">
    <div id="inner"></div>
</div>
```

```js
inner.onclick=function () {
    alert("inner");
};
outer.onclick=function () {
    alert("outer");
};
document.body.onclick=function () {
    alert("body");
};
document.documentElement.onclick=function () {
    alert("document");
};
```

结果：
如果点击inner，会弹出:'inner','outer','body','document'

# DOM0级与DOM2级事件
## DOM0级事件
> 把函数的引用地址赋值给dom的事件属性

特点：
- 给同一个元素，只能绑定一个事件，如果重复绑定，会覆盖。
- DOM0级事件绑定都是在冒泡阶段发生

```js
    inner.onclick=function () {
        alert(1);
    };
    inner.onclick=function () {
        alert(2);
    };
```

结果：
只会弹出一次，弹出2

## DOM2级事件
> 元素调用事件原型上的addEventListener方法

特点：
- 可以给同一个元素绑定几个相同的事件，重复绑定相同事件，不会覆盖。

**语法**:ele.addEventListener([type],[fn],[true/false])
type:事件类型
fn:要绑定的函数
false:函数在冒泡阶段发生（默认值）
true:函数在捕获阶段发生

**DOM2级事件实例**：
```js
inner.addEventListener("click",function () {
    alert("inner");
},false);//false是冒泡阶段，true是捕获阶段
outer.addEventListener("click",function () {
    alert("outer");
},true);//在捕获阶段就触发了
document.body.addEventListener("click",function () {
    alert("body");
},false);
document.documentElement.addEventListener("click",function () {
    alert("document");
},false);
```

结果：
点击inner，会弹出:'outer','inner','body','document'

注意：
- 只有DOM2级事件绑定才能设置函数是在冒泡阶段发生还是捕获阶段发生
- 一般来说，DOM2级事件都是绑定实名函数（为了移除事件的时候能找到对应的函数）

# 事件委托
> 事件委托主要是借用事件的冒泡传递特性，在给多个元素循环绑定事件的情况下，可以给它们的父级元素绑定事件，来达到我们想要的效果，这样可以节省性能。

**事件委托案例**
```html
//html
<ul class="list" id="list">
    <li>1<span class="close">x</span></li>
    <li>2<span class="close">x</span></li>
    <li>3<span class="close">x</span></li>
    <li>4<span class="close">x</span></li>
</ul>
```

```css
//css
*{ margin:0; padding:0;}
.ul{ list-style: none;}
.list{ padding:0 10px;}
.list li{ position: relative; height:150px; line-height:150px; font-size: 30px; background: lightcyan; border:2px solid lightsalmon; margin-bottom:5px; text-align: center; margin-bottom: 10px;}
.list li .close{ position: absolute; right:0; width:30px; height:30px; line-height:24px; color: lightsalmon; border:2px solid lightsalmon; border-width:0 0 2px 2px; cursor: pointer;}
```

```js
//js
var list=document.getElementById("list");
list.onclick=function (e) {
    e=e||window.event;
    e.target=e.target||e.srcElement;
    if(e.target.nodeName=="SPAN"&&e.target.className=="close"){
        this.removeChild(e.target.parentNode);
    }
}
```

# 事件兼容性
**标准浏览器中的DOM2级事件**：
- 同一个事件绑定多个函数，事件触发的时候，按照绑定顺序执行
- 同一个事件，同一个函数，同一个阶段不能重复绑定
- 绑定事件函数中的this仍然和DOM0绑定一样

**IE低版本浏览器中的DOM2级事件**：
- type:on+"click"  事件类型需要在前面加"on"
- 不能设置冒泡还是捕获，默认都是冒泡阶段发生
- 同一个事件绑定多个函数，事件触发的时候，不会按照绑定顺序执行
- 同一个事件，同一个函数，同一个阶段能重复绑定
- 绑定事件函数中的this是window

标准浏览器绑定事件：addEventListener
标准浏览器移除事件：removeEventListener
IE低版本浏览器绑定事件：attachEvent
IE低版本浏览器绑定事件：detachEvent

**兼容性解决**：
```js
//绑定事件
function on(ele,type,handler) {
    if(ele.addEventListener){
        ele.addEventListener(type,handler,false);
        return;
    }
    if(ele.attachEvent){
        if(!ele["ev"+type]){
            ele["ev"+type]=[];
            ele.attachEvent("on"+type,function () {
                run.call(ele);
            });
        }
        var ary=ele["ev"+type];
        for(var i=0;i<ary.length;i++){
            if(ary[i]==handler){
                return;
            }
        }
        ary.push(handler);
    }
}
function run(e) {
    e=window.event;
    e.target=e.srcElement;
    e.pageX=e.clientX+(document.documentElement.scrollLeft||document.body.scrollLeft);
    e.pageY=e.clientY+(document.documentElement.scrollTop||document.body.scrollTop);
    e.preventDefault=function () {
        e.returnValue=false;
    };
    e.stopPropagation=function () {
        e.cancelBubble=true;
    };
    var ary=this["ev"+e.type];
    if(ary&&ary.length){
        for(var i=0;i<ary.length;i++){
            if(typeof ary[i]=="function"){
                //ary[i]();
                ary[i].call(this,e);
            }else{
                ary.splice(i,1);
                i--;
            }
        }
    }
}
//移除事件
function off(ele,type,handler) {
    if(ele.removeEventListener){
        ele.removeEventListener(type,handler);
        return;
    }
    if(ele.detachEvent){
        var ary=ele["ev"+type];
        if(ary&&ary.length){
            for(var i=0;i<ary.length;i++){
                if(ary[i]==handler){
                    ary[i]=null;
                    break;
                }
            }
        }
    }
}
//处理this
function processThis(callback,context) {
    return function (e) {
        callback.call(context,e);
    }
}
```