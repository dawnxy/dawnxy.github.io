---
title: Math方法及应用
date: 2016-10-05 18:44:20
categories: JavaScript
---

> Math 对象用于执行数学任务。
# Math.abs()
> 求一个数的绝对值

**语法**：Math.abs([num])
```js
console.log(Math.abs(-123));//123
```

# Math.pow()
> 求一个数的几次幂

**语法**：Math.pow([num1],[num2])
```js
console.log(Math.pow(2,3));//8
```

# Math.sqrt()
> 求一个数的平方根

**语法**：Math.sqrt([num])
```js
console.log(Math.sqrt(4));//2
```

# Math.ceil()
> 对一个数向上取整

**语法**：Math.ceil([num])
```js
console.log(Math.ceil(4.2));//5
```

# Math.floor()
> 对一个数向下取整

**语法**：Math.floor([num])
```js
console.log(Math.floor(4.8));//4
```

# Math.max()
> 求参数列表中的最大值

**语法**：Math.max([num1],[num2]...)
```js
console.log(Math.max(1,2,4,8));//8
```

# Math.min()
> 求参数列表中的最小值

**语法**：Math.min([num1],[num2]...)
```js
console.log(Math.min(2,3,5,1));//1
```

# Math.round()
> 对一个数进行四舍五入

**语法**：Math.round([num])
```js
console.log(Math.round(4.5));//5
console.log(Math.round(4.2));//4
```

# Math.random()
> 返回0到1之间的随机小数

**语法**：Math.random()
```js
console.log(Math.random());//返回0-1之间的随机小数
```

# Math.random(Math.round()*(m-n)+n)
> 求任意两个数之间的随机整数

**语法**：Math.random(Math.round()*(m-n)+n)
```js
console.log(Math.round(Math.random()*(100-10)+10));//返回10-100之间的随机整数
```