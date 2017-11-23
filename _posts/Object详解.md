---
title: Object详解
date: 2016-11-04 18:44:20
categories: JavaScript
---

什么是对象？
> 万物皆对象，对象由由多组键值对（属性名和属性值）组成。

# 创建对象
## 实例创建
```js
var obj=new Object();
```

如果不给构造函数传递参数，可以省略后面的括号，但是不推荐这种方式

## 字面量创建
```js
var obj={
    name:'xy',
    age:23,
    111:123
}
```

# 属性操作
## 访问属性
### 点访问
```js
obj.name;
```

属性名为数字的时候不能使用此种方式

### 方括号访问
```javascript
obj['name'];
```
属性名为数字的时候可以使用此种方式，并且属性名是数字的时候可以省略引号

```javascript
obj[111];
```

## 增加属性
对象名.新属性名=新属性值，或者
对象名['新属性名']=新属性值
```js
obj.sex='女';
obj['sex']='女';
```

## 修改属性
对象名.属性名=新属性值，
或者 对象名.属性名=新属性值
```js
obj.age=25;
obj['age']=25;
```

## 删除属性
### 删除属性
delete 对象名.属性名，
或者 delete 对象名['属性名']
```js
delete obj.sex;
delete obj['sex'];
```

### 删除属性值
对象名.属性名=null，
或者 对象名['属性名']=null
```js
obj.sex=null;
obj['sex']=null;
```

# 内置属性和自定义属性
## 内置属性
> 对象天生自带的属性

```js
obj.className='bg1';//className就是对象天生自带的属性
```

## 自定义属性
> 我们自己定义的属性

```js
obj.hobby='eat';//hobby就是我们自己定义的属性
```

# 可枚举属性和不可枚举属性
> 在JavaScript中，对象的属性分为可枚举和不可枚举之分，它们是由属性的enumerable值决定的。可枚举性决定了这个属性能否被for-in查找遍历到。

**如何判断属性是否可枚举**？
js中基本包装类型的原型属性是不可枚举的，如Object, Array, Number等，如果你写出这样的代码遍历其中的属性：
```js
var num = new Number();
for(var pro in num) {
    console.log("num." + pro + " = " + num[pro]);//什么也不输出
}
```

它的输出结果会是空。这是因为Number中内置的属性是不可枚举的，所以不能被for…in访问到。

Object对象的propertyIsEnumerable()方法可以判断此对象是否包含某个属性，并且这个属性是否可枚举。

# 检测属性
我们经常要判断某一个属性是否存在于某一个对象中。这个时候我们可以通过in运算符，hasOwnProperty()方法或是propertyIsEnumerable()方法来进行判断。

## in操作符
> in 是一个二元操作符，用来判断当前属性是否属于某个对象。并且即使是`继承属性`，也是可以测试的。

**语法**：[attr] in [Object]
```js
var obj={name:'xy'};
console.log('name' in obj);//true
console.log('JSON' in window);//true
```

## hasOwnProperty()
> hasOwnProperty方法只能测试当前属性是不是对象的`私有属性`。

```js
function Person(name,age) {
    this.name=name;
    this.age=age;
}
Person.prototype.x=200;
var person=new Person('xy',8);
for(attr in person){
    if(person.hasOwnProperty(attr)){//只遍历私有的属性和方法
        console.log(person[attr]);//xy  8
    }
    //console.log(person[attr]);//xy  8  200
}
```

## propertyIsEnumerable()
> 只有当当前的属性是`自身属性`，并且是`可枚举`的的时候，这一方法才会返回true

```js
 var obj={
    name:'xy',
    age:20
};
Object.defineProperty(obj,"name",{
    enumerable:false
});
console.log(obj.propertyIsEnumerable("name"));//false
console.log(obj.propertyIsEnumerable("age"));//true
```

# 遍历属性
## for-in
> 可以遍历对象中的所有的可枚举属性，包括当前对象的自有属性和继承属性。

```js
for(var attr in obj){
    //key-->属性名   obj-->要遍历的对象
    //obj[attr]-->对象的值  注意：key是个变量，一定不能加引号
    console.log(obj[attr]);
}
```

# Object上的方法
## Object.create()
> 创建一个拥有指定原型和若干个指定属性的对象。

**语法**：Object.create(proto, [ propertiesObject ])
proto：一个对象，作为新创建对象的原型。或者为 null。
propertiesObject：可选。该参数对象是一组属性与值，该对象的属性名称将是新创建的对象的属性名称，值是属性描述符

如果 proto 参数不是 null 或一个对象值，则抛出一个 TypeError 异常。
```js
// 创建一个原型为null的空对象
o = Object.create(null);

var o = {};
// 以字面量方式创建的空对象就相当于:
o = Object.create(Object.prototype);

var o = Object.create(Object.prototype, {
  // foo会成为所创建对象的数据属性
  foo: { writable:true, configurable:true, value: "hello" }
}})

```

## Object.keys()
> 枚举对象的所有`自身属性`，返回一个数组。包括：`可枚举属性`。

```js
var obj={
    name:'xy',
    age:8
};
var attrAry=Object.keys(obj);
console.log(attrAry);//["name", "age"]
```

## Object.getOwnPropertyNames()
> 枚举对象的所有`自身属性`，返回一个数组。包括：`不可枚举属性`。

```js
var obj={
    name:'xy',
    age:20
};
Object.defineProperty(obj,"name",{
    enumerable:false//name不可枚举
});
console.log(Object.getOwnPropertyNames(obj));//["name", "age"]
console.log(Object.keys(obj));//["age"]
```

## Object.defineProperty
> 自定义属性的特性

**语法**：Object.defineProperyty(obj,prop,descriptor)
obj: 需要定义的对象
prop：需要定义 或修改的属性名
descriptor:属性定义或修改的属性描述

属性描述包括：
- writable  是否可写（修改属性）
- configurable  是否可配置（删除属性）
- enumerable  是否可枚举（遍历属性）
- value  属性的值
以上的属性值默认都是true

```js
var obj={
    name:'xy',
    age:20
};
Object.defineProperty(obj,"name",{
    writable:false,//不可写
    configurable:false,//不可配置
    enumerable:true//可以枚举
});
obj.name='abc';
console.log(obj.name);//xy
delete obj.name;
console.log(obj.name);//xy
```

## Object.defineProperties()
> 一个对象上添加或修改一个或者多个自有属性，并返回该对象。

**语法**：Object.defineProperties(obj, props)

```
var obj={
    name:'xy',
    age:20
};
Object.defineProperties(obj,{
    "name":{
        writable:false,//不可写
        configurable:true,//可配置
        enumerable:true//可枚举
    },
    "age":{
        writable:true,//可写
        configurable:false,//不可配置
        enumerable:true,//可枚举
        value:25//修改age的属性值
    }
 });
obj.name='abc';
console.log(obj.name);//xy
delete obj.age;
console.log(obj.age);//20
```

# Object实例上的方法
Object的每个实例都具有以下属性和方法：
- constructor
保存着当前实例的构造函数

- hasOwnProperty(propertyName)
检测给定的属性是否在当前对象的私有属性

- propertylsEnumerable(propertyName)
只有当当前的属性是自有属性，并且是可枚举的的时候，这一方法才会返回true

- isPrototypeOf(object)
检测给定的对象是否是当前对象的原型

- toLocaleString()
返回对象的字符串表示，该字符串与执行环境的地区对应

- toString()
返回对象的字符串表示

