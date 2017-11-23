---
title: bind,live,delegate,on绑定事件的区别
date: 2017-02-22 18:44:20
categories: jQuery
---

# bind()
> 向匹配元素添加一个或多个事件处理器。

**语法**：$(selector).bind(event,data,fn)

| 参数      |    描述 |
| :-------- |  :--: |
| event  | 必需。规定添加到元素的一个或多个事件。<br/>由空格分隔多个事件。必须是有效的事件。 |
| data  | 可选。规定传递到函数的额外数据。 |
| fn  | 必需。规定当事件发生时运行的函数。 |

**适用jquery版本**：
适用所有版本，但是根据官网解释，自从jquery1.7版本以后bind()函数推荐用on()来代替。

# live()
> 向当前或未来的匹配元素添加一个或多个事件处理器

**语法**：$(selector).live(event,data,fn)

| 参数      |    描述 |
| :-------- |  :--: |
| event  | 必需。规定添加到元素的一个或多个事件。<br/>由空格分隔多个事件。必须是有效的事件。 |
| data  | 可选。规定传递到函数的额外数据。 |
| fn  | 必需。规定当事件发生时运行的函数。 |

**适用jquery版本**：
jquery1.9版本以下支持，jquery1.9及其以上版本删除了此方法，jquery1.9以上版本用on()方法来代替。

# delegate()
> 向当前或未来的匹配元素添加一个或多个事件处理器

**语法**：$(selector).delegate(childSelector,event,data,fn)

| 参数      |    描述 |
| :-------- |  :--: |
| childSelector  | 必需。需要添加事件处理程序的元素，一般为selector的子元素。 |
| event  | 必需。规定添加到元素的一个或多个事件。<br/>由空格分隔多个事件。必须是有效的事件。 |
| data  | 可选。规定传递到函数的额外数据。 |
| fn  | 必需。规定当事件发生时运行的函数。 |

**举例说明**：
```js
var data=123;
$('.wrapper div').delegate('.inner','click',data,function () {
    console.log(data);
});
```

**适用jquery版本**：
jquery1.4.2及其以上版本；

# on()
> 向当前或未来的匹配元素添加一个或多个事件处理器

**语法**：$(selector).on(event,childselector,data,function)

| 参数      |    描述 |
| :-------- |  :--: |
| event  | 必需。规定添加到元素的一个或多个事件。<br/>由空格分隔多个事件。必须是有效的事件。 |
| childSelector  | 可选。需要添加事件处理程序的元素，一般为selector的子元素。 |
| data  | 可选。规定传递到函数的额外数据。 |
| fn  | 必需。规定当事件发生时运行的函数。 |

**单事件处理**：
```js
$(selector).on("click",data,fn);
```

**多事件处理**：
- 利用空格分割多事件
```js
$(selector).on("click dbclick mouseout",childselector,data,fn);
```

- 利用大括号灵活定义多事件
```js
$(selector).on({
    event1:fn,
    event2:fn,
    ...
})　;
```

- 链式绑定事件
```js
$(selector)
.on("click",childselector,data,fn)
.on("dbclick",childselector,data,fn)
.on("mouseout",childselector,data,fn);
```

**适用jquery版本**：
jquery1.7及其以上版本；jquery1.7版本出现之后用于替代bind()，live()绑定事件方式；

# 四种方式的优点和缺点
## 相同点
- 都支持单元素多事件的绑定；空格相隔方式或者大括号替代方式;
- 均是通过事件冒泡方式，将事件传递到document进行事件的响应；

## 比较和联系
- bind()函数只能针对已经存在的元素进行事件的设置；但是live(),on(),delegate()均支持未来新添加元素的事件设置

- bind()函数在jquery1.7版本以前比较受推崇，1.7版本出来之后，官方已经不推荐用bind()，替代函数为on(),这也是1.7版本新添加的函数，同样，可以用来代替live()函数，live()函数在1.9版本已经删除；

- live()函数和delegate()函数两者类似，但是live()函数在执行速度，灵活性和CSS选择器支持方面较delegate()差些

- bind()支持Jquery所有版本；live()支持jquery1.8-；delegate()支持jquery1.4.2+；on()支持jquery1.7+

# 总结
如果项目中引用jquery版本为低版本，推荐用delegate(),高版本jquery可以使用on()来代替。
