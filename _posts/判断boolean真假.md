---
title: 判断boolean真假
date: 2016-11-09 18:44:20
categories: JavaScript
---

布尔类型，只有两个值，真为true，假为false
# Boolean()
**作用**：判断布尔值的真假
**用法**：Boolean([obj])
**返回值**：true/false
**判断规则**：0 NaN “” null undefined为假，其余都为真
```js
console.log(Boolean(' '));//trie
console.log(Boolean('NaN'));//true
console.log(Boolean(null));//false
```

# **!与!!**
**!** ：先将其他数据类型转为布尔类型再取反
```js
console.log(!true);//false
console.log(!0);//true
console.log(!'0');//false
console.log(!'[]');//false
```

**!!** ：先将其他数据类型转为布尔类型再取反，再取反，等价于Boolean()，就是将其他数据类型转为布尔类型
```js
console.log(!!'0');//true
console.log(!!{});//true
```