---
title: Date方法及应用
date: 2016-10-25 18:44:20
categories: JavaScript
---

> Date 对象用于处理日期和时间。

# Date.now()
> Date.now()，用于返回现在的时间戳

```
console.log(Date.now());//1488717051338
```

# Date()
> 返回当日的日期和时间。

```js
console.log(Date());//Sun Mar 05 2017 19:41:45 GMT+0800 (中国标准时间)
```

# getFullYear()
> 从 Date 对象以四位数字返回年份。

```js
var d=new Date();
console.log(d.getFullYear());//2017
```

# getMonth()
> 从 Date 对象返回月份 (0 ~ 11)。

```js
var d=new Date();
console.log(d.getMonth());//2
```

# getDate()
> 从 Date 对象返回一个月中的某一天 (1 ~ 31)。

```js
var d=new Date();
console.log(d.getDate());//5
```

# getDay()
> 从 Date 对象返回一周中的某一天 (0 ~ 6)。

```js
var d=new Date();
console.log(d.getDay());//0
```

# getHours()
> 返回 Date 对象的小时 (0 ~ 23)。

```js
var d=new Date();
console.log(d.getHours());//20
```

# getMinutes()
> 返回 Date 对象的分钟 (0 ~ 59)。

```js
var d=new Date();
console.log(d.getMinutes());//31
```

# getSeconds()
> 返回 Date 对象的秒数 (0 ~ 59)。

```js
var d=new Date();
console.log(d.getSeconds());//52
```

# getMilliseconds()
> 返回 Date 对象的毫秒(0 ~ 999)。

```js
var d=new Date();
console.log(d.getMilliseconds());//837
```