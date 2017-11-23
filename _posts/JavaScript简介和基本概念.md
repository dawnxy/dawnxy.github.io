---
title: JavaScript简介和基本概念
date: 2016-10-01 18:44:20
categories: JavaScript
---

# JavaScript简介
什么是JavaScript?
> JavaScript是一种面向对象的基于客户端的弱类型脚本编程语言，简称JS

 **JavaScript组成**：
- **DOM**（文档对象模型）
- **BOM**（浏览器对象模型）
- **ECMAScript**（JavaScript的规范）

# JavaScript引入方式
- 行内式
将js写到标签上
```javascript
<button onclick="javascript:alert(1)"></button>
```

- 内嵌式
将js写到`<script>`标签中
```javascript
<script>
	alert(1)
</script>
```

- 外链式
引入一个外部的js文件
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js.js"></script><!--位置1：引入到head中-->
</head>
<body>
<script src="js.js"></script><!--位置2：引入到<body>下面-->
<div>hello</div>
<script src="js.js"></script><!--位置3：引入到</body>上面（比较推荐这种方式）-->
</body>
<script src="js.js"></script><!--位置4：引入到</body>下面（不推荐，可能会被屏蔽）-->
</html>
```

**注意**：
- 内嵌式和外链式一般我们写在`</body>`上面，因为浏览器渲染页面是从上往下的，这样不会导致获取不到页面中的元素。
- 外链式标签中间的代码块不生效。

# JavaScript注释
**单行注释**
```js
//单行注释
```

**多行注释**
```js
/*多行注释*/
```

# JavaScript命名规范
**1.严格区分大小写**
在ie6,ie7不区分大小写，所以取名时，不要用大小写来进行区分。
**2.变量由字母、数字、下划线，$符号组成，不能以数字开头**
**3.推荐使用驼峰命名法**
多个单词组合的时候，第一个有意义的单词首字母小写，其余单词首字母大写。例如：var oDiv
**4.不能使用关键字和保留字，以及js内置的对象、属性、方法名**
**5.变量匈牙利命名类型**
是由Microsoft公司的匈牙利程序员查尔斯西蒙尼（Charles Simonyi）首创的。
匈牙利命名规则：由`类型`+`属性`+`对象描述`组成
例如：iWidthMax 表示最大宽度，是个整型

**关键字**：
 break、else、new、var、 case、  finally 、 return、 void 、 catch  、for  、switch 、 while 、 continue、  function  、this 、 with 、default 、 if 、 throw 、 delete 、 in 、  try 、do 、 instranceof、  typeof

 **保留字**（将来可能被用作关键字）：
 abstract 、 enum   、int 、 short 、 boolean  、export  、interface、  static、  byte  、extends 、 long 、 super 、 char 、 final  、native  、synchronized 、 class  、float 、 package  、throws 、 const  、goto  、private 、transient 、 debugger 、 implements  、protected 、 volatile 、 double  、import  、public

# 严格模式
严格模式是js为了定义一种不同的解析与执行模式。
要在整个脚本中启用严格模式，可以在顶部添加以下代码：
```javascript
'use strict'//它是一个编译指示，用于告诉支持的js引擎切换到严格模式
```

也可以指定函数在严格模式下执行
```javascript
function fn(){
	'use strict'
	//函数体
}
```

# JavaScript输出
## 文档中输出
它可以识别html标签
```javascript
document.write()
```

**注意**：在`window.onload`中写入document.write()会覆盖页面原有内容

## 弹出框
```javascript
alert();//警告消息框  只有`确认`按钮
prompt();//提示消息框  提示用户输入，有`确认`和`取消`按钮
confirm();//确认消息框  有`确认`和`取消`按钮
```

## 控制台输出
```javascript
console.log();//普通信息
console.error();//错误信息
console.warn();//警告信息
console.info();//提示性信息
console.dir();//显示一个对象的所有属性和方法
console.debug();//调试信息
console.clear();//清空控制台  快捷键ctrl+l
```

## innerHTML / innerText
```html
<div id="div"></div>
```

```javascript
var oDiv=document.getElementById("div");
oDiv.innerText="<p>我是文字</p>";//oDiv中的内容：`<p>我是文字</p>`
oDiv.innerHTML="<p style='color:red'>我是文字</p>";//oDiv中的内容：`我是文字`，并且文字会变红
console.log(oDiv.innerHTML);//<p>我是文字</p>
console.log(oDiv.innerText);//我是文字
```

**innerHTML与innnerText的区别**：
- 赋值内容时：innerHTML会解析标签，而innerText不会。
- 获取内容时：innerHTML包括元素内的标签和文字，而innerText只包括文字。

# JavaScript变量
什么是变量？
> 变量是用来存储值和代表值的

**声明变量**

我们通过`var`关键字来声明变量
```javascript
var a;//声明不赋值，默认值是undefined
```

变量可以一次声明，多次创建
```javascript
var a=1,b=2,c=3;//相当于var a=1;  var b=2;  var c=3;
```

```javascript
var a=b=2;//相当于var a=2;  b=2;
```

声明也可横跨多行
```javascript
var a=1,
	b=2,
	c=3;
```

在js中，我们把创建变量叫做`声明`，给变量赋值叫做`定义`。
赋值什么类型的值，这个变量就是什么类型的，所以说，js是弱类型语言。