---
title: ES6常用语法
date: 2017-02-17 18:44:20
categories: ES6
---

# ECMAScript 6简介
> ECMAScript 6（以下简称ES6）是JavaScript语言的下一代标准。因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。

也就是说，ES6就是ES2015。

# babel
在我们正式讲解ES6语法之前，我们得先了解下Babel。

Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。

# let与const
## let
> let操作符是ES6中新增的一种声明变量的方式，它不存在变量提声

### var
var没有块级作用域，定义后在当前闭包中都可以访问，如果变量名重复，就会覆盖前面定义的变量，并且也有可能被其他人更改。
```js
if (true) {
     var a = 123; // 期望a是某一个值
 }
 a=23;
console.log(a);//23
```

```js
for (let i = 0; i < 3; i++) {
     setTimeout(function () {
         console.log(i)
     }, 0);
}
```

运行结果：输出三次3
原因：var声明的变量没有块级作用域

### 块级作用域
作用域就是一个变量的作用范围。也就是你声明一个变量以后,这个变量可以在什么场合下使用,以前只有全局作用域和函数作用域。

在用var定义变量的时候，变量是通过闭包进行隔离的，现在用了let，不仅仅可以通过闭包隔离，还增加了一些块级作用域隔离。 块级作用用一组大括号定义一个块,使用 let 定义的变量在大括号的外面是访问不到的
```js
if(true){
    let name = 'xy';
}
console.log(name);// ReferenceError: name is not defined
```

**for循环中可以用**
```js
// 嵌套循环不会相互影响，因为产生了块级作用域
for (let i = 0; i < 3; i++) {
    console.log("out", i);
    for (let i = 0; i < 2; i++) {
        console.log("in", i);
    }
}
```

**重复定义会报错**
在同一个作用域中重复定义会报错，在不同的作用域中允许重复定义
```js
if(true){
    let a = 1;
    let a = 2; //Identifier 'a' has already been declared
}
```

**不存在变量的预解释**
```js
console.log(i);//i is not defined
let i = 100;
```

**闭包新写法**
ES5写法：
```js
;(function () {
})();
```

ES6写法：
```js
{
}
```

## const
> const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。

```js
const A=123;
A=23;//此行代码在webstorm会划红线
console.log(A);//Uncaught TypeError: Assignment to constant variable
```

注意const限制的是不能给变量重新赋值，而变量的值本身是可以改变的,下面的操作是可以的
```js
const names = ['xy'];
names.push('abc');
console.log(names);
```

# 变量的解构赋值
## 解构数组
解构意思就是分解一个东西的结构,可以用一种类似数组的方式定义N个变量，可以将一个数组中的值按照规则赋值过去。
```js
var[name,age]=["xy",25];
console.log(name,age);
```

## 解构对象
```js
var {name,age}={name:'xy',age:25};//对象里的name属性的值会交给name这个变量，age的值会交给age这个变量
console.log(name,age);
```

```js
var person = {name:'zfpx',address:{province:'江苏',city:'南京'}};
var {name,address}={name:'zfpx',address:{province:'江苏',city:'南京'}};
console.log(address);//{province: "江苏", city: "南京"}
var {province,city}=address;
console.log(name,province,city);//zfpx 江苏 南京
```

# 数值的扩展
## Number.isFinite()
> 判断一个数是否非无穷

```js
console.log(Number.isFinite(12));//true
console.log(Number.isFinite(NaN));//false
console.log(Number.isFinite(Infinity));//false
```

## Number.isNaN(),Number.parseInt(),Number.parseFloat()
ES6将全局方法Number.isNaN(),parseInt()和parseFloat()移植到了Number对象上，行为完全保持不变

## Number.isInteger()
> 判断是否是整数

## Number.isSafeInteger()
> 判断是否是安全整数

# 字符串的扩展
## 模板字符串
模板字符串用反引号(数字1左边的那个键)包含，其中的变量用${}括起来
```js
var name = 'xy',age = 25;
let desc = `${name} is ${age} old!`;
console.log(desc);

//所有模板字符串的空格和换行，都是被保留的
var str = `<ul>
    <li>a</li>
    <li>b</li>
</ul>`;
console.log(str);
```

其中的变量会用变量的值替换掉

## 字符串新方法
### includes()
返回布尔值，表示是否找到了参数字符串。第二个参数，表示开始搜索的位置
```js
var str='abcdefg';
console.log(str.includes('a'));//true
```

### startsWith()
返回布尔值，表示参数字符串是否在源字符串的头部。第二个参数，表示从第n个位置开始搜索
```js
var str='abcdefg';
console.log(str.startsWith('b',1));//true
```

### endsWith()
返回布尔值，表示参数字符串是否在源字符串的尾部。第二个参数，表示前n个字符中搜索
```js
var str='abcdefg';
console.log(str.endsWith('c',3));//true
```

### repeat()
返回一个新字符串，表示将原字符串重复n次。
```js
'x'.repeat(3);
console.log('x'.repeat(3));//'xxx'
```

# 函数的扩展
## 箭头函数
ES6允许使用“箭头”（=>）定义函数。 箭头函数简化了函数的的定义方式，一般以 "=>" 操作符左边为输入的参数， 而右边则是进行的操作以及返回的值inputs=>output
```js
//ES5
function(i){ return i + 1; }
//ES6
(i) => i + 1
```

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
```js
//ES5
function(x, y) {
    x++;
    y--;
    return x + y;
}
//ES6
(x, y) => {x++; y--; return x+y}
```

## this问题
箭头函数根本没有自己的this，导致内部的this就是外层代码块的this 正是因为它没有this，从而避免了this指向的问题
```js
var person = {
    name:'xy',
    getName:function(){
        setTimeout(function(){console.log(this);},1000);//this指向window
        setTimeout(() => console.log(this),1000);//this指向person
    }
}
person.getName();
```

## 默认参数
可以给定义的函数接收的参数设置默认的值 在执行这个函数的时候，如果不指定函数的参数的值，就会使用参数的这些默认的值

ES5写法
```js
function animal(type){
    type = type || 'cat'
    console.log(type)
}
animal()
```

ES6写法
```js
function animal(type = 'cat'){
    console.log(type)
}
animal()
```

## spread操作符
把…放在数组前面可以把一个数组进行展开，直接传入一个函数而不需要使用apply
```js
let print = function(a,b,c){
    console.log(a,b,c);
};
print([1,2,3]);//[1, 2, 3] undefined undefined
print(...[1,2,3]);//1 2 3
```

### 替代apply
```js
var m1 = Math.max.apply(null, [8, 9, 4, 1]);
var m2 = Math.max(...[8, 9, 4, 1]);
console.log(m1);//9
console.log(m2);//9
```

### 替代concat
```js
var arr1 = [1, 3];
var arr2 = [3, 5];
var arr3 = arr1.concat(arr2);
var arr4 = [...arr1, ...arr2];
console.log(arr3);//[1, 3, 3, 5]
console.log(arr4);//[1, 3, 3, 5]
```

### 类数组转数组
```js
function max(a,b,c) {
    console.log(Math.max(...arguments));
}
max(1, 3, 4);//4
```

## 剩余操作符
剩余操作符可以把其余的参数的值都放到一个叫b的数组里面

```js
let rest = function(a,...b){
    console.log(a,b);//1 [2,3]
};
rest(1,2,3);
```

## 解构参数
```js
let destruct = function({name,age}){
    console.log(name,age);//xy 6
};
destruct({name:'xy',age:6});
```

# 数组的扩展
**数组新方法**
## Array.from()
> 将一个数组或者类数组变成数组,会复制一份

```js
var oldArr=[1,2,3];
let newArr = Array.from(oldArr);
console.log(newArr);//[1,2,3]

function fn(a,b,c) {
    let arr=Array.from(arguments);
    console.log(arr);//[1,2,3]
}
fn(1,2,3);
```

## Array.of()
> 将一组数值,转换为数组

```js
console.log(Array.of(1,2,3));//[1,2,3]
```

## Array.copyWithin()
> 替换数据

语法：Array.copyWithin([target],[start],[end]);

target （必需）：从该位置开始替换数据。
start （可选）：从该位置开始读取数据，默认为 0 。如果为负值，表示倒数。
end （可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。
这三个参数都应该是数值，如果不是，会自动转为数值。

```js

[1, 2, 3, 4, 5].copyWithin(0, 3, 4);//  将 3 号位复制到 0 号位
// ==>[4, 2, 3, 4, 5]

[1, 2, 3, 4, 5].copyWithin(0, -2, -1);// -2 相当于 3 号位， -1 相当于 4 号位
// ==>[4, 2, 3, 4, 5]

[].copyWithin.call({length: 5, 3: 1}, 0, 3);//  将 3 号位复制到 0 号位
// ==>{0: 1, 3: 1, length: 5}


var i32a = new Int32Array([1, 2, 3, 4, 5]);// 将 2 号位到数组结束，复制到 0 号位
i32a.copyWithin(0, 2);
// ==>Int32Array [3, 4, 5, 4, 5]

//  对于没有部署 TypedArray 的 copyWithin 方法的平台,需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// ==>Int32Array [4, 2, 3, 4, 5]
```

## Array.find()
> 查到指定的元素

```js
let arr = [1, 2 ,3, 3, 4, 5];
let find = arr.find((item, index, arr) => {
    return item === 3;
});
console.log(find);//3
```

## Array.findIndex()
> 查到指定的索引

```js
let arr = [1, 2 ,3, 3, 4, 5];
let findIndex = arr.findIndex((item, index, arr) => {
    return item === 3;
});
console.log(findIndex);//2
```

## Array.fill()
> 填充数组

会更改原数组 Array.prototype.fill(value, start, end = this.length);
```js
let arr = [1, 2, 3, 4, 5, 6];
arr.fill('a', 1, 3);
console.log(arr);//[ 1, 'a','a', 4, 5, 6 ]
```

# 对象的扩展
## 对象字面量
- 键值和键名一样，写一个就好
- 值为函数时，可以去掉  ':function'
```js
let name="xy";
let age = 27;
let getName = function(){
    console.log(this.name);
};
let person = {
    name,
    age,
    getName
};
person.getName();//xy
```

## Object.is()
> 对比两个值是否相等

```js
console.log(Object.is(NaN,NaN));//true
console.log(NaN==NaN);//false
```

## Object.assign()
> 把多个对象的属性复制到一个对象中

第一个参数是复制的对象
从第二个参数开始往后,都是复制的源对象

```js
var nameObj = {name:'xy'};
var ageObj = {age:28};
var obj = {};
Object.assign(obj,nameObj,ageObj);
console.log(obj);//{ name: 'xy', age: 28 }

//克隆对象
function clone (obj) {
    return Object.assign({}, obj);
}
```

## Object.setPrototypeOf()
> 将一个指定的对象的原型设置为另一个对象或者null

```js
var obj={};//__proto__:Object
Object.setPrototypeOf(obj, null);
console.log(obj);//No Properties
```

## proto
> 直接在对象表达式中设置prototype

```js
var obj1  = {name:'xy'};
var obj3 = {
    __proto__:obj1
};
console.log(obj3.name);//xy
console.log(Object.getPrototypeOf(obj3));//{name: "xy"}
```

## super()
> 通过super可以调用prototype上的属性或方法

```js
let person ={
    eat(){
        return 'milk';
    }
}
let student = {
    __proto__:person,
    eat(){
        return super.eat()+' bread'
    }
}
console.log(student.eat());//milk bread
```

# Symbol
> ES6引入了一种新的原始数据类型Symbol，表示`独一无二`的值 它是JavaScript语言的第七种数据类型

## 声明Symbol
```js
let s = Symbol('xy');
console.log(s);//Symbol(xy)
```

## 作为属性名
由于每一个Symbol值都是不相等的，这意味着Symbol值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性
```js
var person = {
    [Symbol()]:1,
    [Symbol()]:2,
    [Symbol()]:3
};
console.log(person); //{Symbol(): 1, Symbol(): 2, Symbol(): 3}
```

在对象的内部，使用Symbol值定义属性时，Symbol值必须放在方括号之中

消除魔术变量
```js
var Operator = {
    add: Symbol(),
    minus:Symbol()
};
function calculate(op, a, b) {
    switch (op) {
        case Operator.add:
            return a + b;
            break;
        case Operator.minus:
            return a - b;
            break;
    }
}
console.log(calculate(Operator.add, 10,10));
console.log(calculate(Operator.minus, 10,10));
```

# 生成器与迭代器
我们知道生成器也是迭代器，所以操作迭代器的方法都可以用来操作生成器，下面逐一过下：
```js
function* genFunc() {
    yield 'a';
    yield 'b';
}
```

## next方法调用
```js
let genObj = genFunc();
console.log(genObj.next());//{ value: 'a' , done: false }
console.log(genObj.next());//{ value: 'b' , done: }
console.log(genObj.next());//{ value:  undefined, done: true  }
```

## for-of循环
```js
for (let x of genFunc()) {
    console.log(x);//a b
}
```

## spread操作符
```js
let arr = [...genFunc()];
console.log(arr);// ['a', 'b']
```

## 数组解构赋值
```js
let [x, y] = genFunc();
console.log(x,y);//'a' 'b'
```

# 集合
## Set
一个Set是一堆东西的集合,Set有点像数组,不过跟数组不一样的是，Set里面不能有重复的内容
```js
var books = new Set();
books.add('js');
books.add('js');//添加重复元素集合的元素个数不会改变
books.add('html');
books.forEach(function(book){//循环集合
    console.log(book);//js  html
});
console.log(books.size);//2  集合中元数的个数
console.log(books.has('js'));//true  判断集合中是否有此元素
books.delete('js');//从集合中删除此元素
console.log(books.size);//1
console.log(books.has('js'));//false
books.clear();//清空 set
console.log(books.size);//0
```

## Map
可以使用 Map 来组织这种名值对的数据

```js
var books = new Map();
books.set('js',{name:'js'});//向map中添加元素
books.set('html',{name:'html'});
console.log(books);//Map {"js" => Object {name: "js"}, "html" => Object {name: "html"}}
console.log(books.size);//2  查看集合中的元素
console.log(books.get('js'));//{name: "js"}   通过key获取值
books.delete('js');//执照key删除元素
console.log(books.has('js'));//false   判断map中有没有key
books.forEach((value, key) => { //forEach可以迭代map
    console.log( key + ' = ' + value);
});
books.clear();//清空map
console.log(books);//Map {}
```

# 模块的导入导出
我们之前写的Javascript一直都没有模块化的体系，无法将一个庞大的js工程拆分成一个个功能相对独立但相互依赖的小工程，再用一种简单的方法把这些小工程连接在一起。

这有可能导致两个问题：
- 一方面js代码变得很臃肿，难以维护;
- 另一方面我们常常得很注意每个script标签在html中的位置，因为它们通常有依赖关系，顺序错了可能就会出bug;在ES6之前为解决上面提到的问题，我们得利用第三方提供的一些方案，主要有两种CMD(服务器端,如CommonJS)和AMD（浏览器端，如require.js）。
而现在我们有了ES6的module功能，它实现非常简单，可以成为服务器和浏览器通用的模块解决方案。

ES6模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CMD和AMD模块，都只能在运行时确定这些东西。


假设我们有两个js文件: index.js和content.js,现在我们想要在index.js中使用content.js返回的结果，我们要怎么做呢？
**require.js写法**
首先定义content.js
```js
define('content.js', function(){
    return 'A cat';
})
```

然后定义index.js
```js
require(['./content.js'], function(animal){
    console.log(animal);   //A cat
})
```

**CommonJS写法**
定义content.js
```js
module.exports = 'A cat'
```

定义index.js
```js
var animal = require('./content.js')
```

**ES6Module写法**
定义content.js
```js
export default 'A cat'
```

定义index.js
```js
import animal from './content'
```

## 导出语法
```js
export {name1,name2,...,nameN}
export {variable1 as name1,variable2 as name2,...}
export let name1,name2,...
export let name1=..,name2=...
export default expression
export default function(...){}...  //also class,function
export default function name1(...){...}  //also class,function
export{name1 as default,...}
export * from ...
export {name1,name2,...} from ...
expoert {import1 as name1,import2 as name2,...} from ...
```

实例
```js
export {function};
//导出一个函数
export const foo = 2;
//导出一个常量
export default myFunctionClass;
//默认导出，每个模块只有一个默认导出，导出的可以是一个函数，一个对象，一个类
```

## 导入语法
```js
import name from 'module-name'
import {member} from 'module-name'
import {member as alias} from 'module-name'
import {member1,member2as alias2,[...]} from 'module-name'
import name,{member,[,[...]]} from 'module-name'
import 'module-name'
```

实例：
```js
import name from 'my-module.js' ;
//导出整个模块到当前作用域，name作为接收该模块的对象名称
　　
import {moduleName} from 'my-module.js';
//导出模块中的单个成员moduleName
import {moduleName1,moduleName2} from 'my-module';
//导出模块中的多个成员moduleName1、moduleName2
　　
import {moduleName as moduleAlias} from 'my-module';
import myDefault,{moduleName1,moduleName2} from 'my-module';
//myDefault为my-module.js文件default导出项
```
