---
title: number数据类型
date: 2016-11-02 18:44:20
categories: JavaScript
---

数字数据类型包括：
- 整数（正整数，0，负整数）
- 小数
- NaN

# 数值转化函数
js中数值转化的函数有：
- Number
- parseInt
- parseFloat

## Number()
**作用**：把对象的值转为数字
**语法**：Number([obj])
**返回值**：数字 / NaN
**转化规则：**
- **字符串直接转化**
```js
console.log(Number('123'));//123
console.log(Number('123abc'));//NaN
console.log(Number(''));//0
console.log(Number('  '));//0
```

- **对象先转化为字符串（通过toString()），再转化为数字（通过Number()）**
```js
console.log(Number([]));//''==>0
console.log(Number([1]));//'1'==>1
console.log(Number([1,2,3]));//'1,2,3'==>NaN
console.log(Number({}));//[object Object]==>NaN
console.log(Number({name='xy'}));//[object Object]==>NaN
```

- **布尔直接转化为数字**
```js
console.log(Number(true));//1
console.log(Number(false));//0
```

- **null转化为0**
```js
console.log(Number(null));//0
```

- **undefined转化为NaN**
```js
console.log(Number(undefined));//NaN
```

## parseInt()
**作用**：将对象转化为整型
**用法**：parseInt([obj])
**转化规则：** 从第一个值开始查找，直到不是数字的结束
```js
console.log(parseInt('123abc'));//123
console.log(parseInt('abc123'));//NaN
console.log(parseInt('.5'));//NaN
console.log(parseInt('0.5'));//0
```

## parseFloat()
**作用**：将对象转化为浮点型
**用法**：parseFloat([obj])
**转化规则：**从第一个值开始查找，直到不是数字的结束，识别小数点
```js
console.log(parseInt('123.456'));//123.456
console.log(parseInt('123abc'));//123
console.log(parseInt('abc123'));//NaN
console.log(parseInt('.5'));//0.5
console.log(parseInt('0.5'));//0.5
```

# NaN与isNaN
## NaN
NaN代表不是数字。任何值与NaN都不相等，包括它自己
```js
console.log(NaN==NaN);//false
```

但是NaN属于数字数据类型number
```js
console.log(typeof NaN);//number
```

## isNaN()
**作用**：判断一个值是否不是数字
**用法**：isNaN([obj])
**返回值**：如果不是数字，返回true，否则，返回false
**判断规则：**调用Number方法，看能否转为数字
```js
console.log(isNaN('123abc'));//NaN==>true
console.log(isNaN([]));//0==>false
console.log(isNaN({}));//NaN==>true
console.log(isNaN(NaN));//true
```























