---
title: 跨域及JSONP的原理
date: 2017-02-03 18:44:20
categories: JavaScript
---

# JSON与JSONP
> JSON是一种数据交换格式

> 而JSONP是一种依靠开发人员的聪明才智创造出的一种非官方跨域数据交互协议

# 同源与非同源
如果你请求数据的地址跟当前页面的地址，协议，域名，端口号，全部一致，就是同源，如果有一个不一致，就是非同源

# 跨域的方式
- jsonp(最常用的方式)
借用`<script>`标签的无跨域限制漏洞，来达到与第三方通讯的目的
- window.name
window 对象的name属性是一个很特别的属性，当该window的location变化，然后重新加载，它的name属性可以依然保持不变。那么我们可以在页面 A中用iframe加载其他域的页面B，而页面B中用JavaScript把需要传递的数据赋值给window.name，iframe加载完成之后，页面A修改iframe的地址，将其变成同域的一个地址，然后就可以读出window.name的值了。这个方式非常适合单向的数据请求，而且协议简单、安全。不会像JSONP那样不做限制地执行外部脚本。
- document.domain
通过修改document的domain属性，我们可以在域和子域或者不同的子域之间通信。同域策略认为域和子域隶属于不同的域，比如www.a.com和 sub.a.com是不同的域，这时，我们无法在www.a.com下的页面中调用sub.a.com中定义的JavaScript方法。但是当我们把它们document的domain属性都修改为a.com，浏览器就会认为它们处于同一个域下，那么我们就可以互相调用对方的method来通信了。
- window.postMessage
window.postMessage是HTML5定义的一个很新的方法，这个方法可以很方便地跨window通信。由于它是一个很新的方法，所以在很旧和比较旧的浏览器中都无法使用。

# JSONP的原理
很简单，就是利用`<script>`标签没有跨域限制的“漏洞”（历史遗迹啊）来达到与第三方通讯的目的。当需要通讯时，本站脚本创建一个`<script>`元素，地址指向第三方的API网址，形如：
```html
<script src="http://www.example.net/api?wd=z&callback=fn"></script>
```

并提供一个回调函数来接收数据（函数名可约定，或通过地址参数传递）。
第三方产生的响应为json数据的包装（故称之为jsonp，即json padding），形如：
fn({"name":"hax","gender":"Male"})
这样浏览器会调用callback函数，并传递解析后json对象作为参数

## 使用原生js进行jsonp跨域请求:
```js
function fn(result) {
    console.log(result);
}

<script src="https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=z&cb=fn"></script>
```

### 客户端
- 创建一个函数
- 增加一个script标签，在src中引入请求数据的接口地址，通过问号传参形式，传递一个回调函数来接收数据，让浏览器帮我们发送请求 &wb=z&cb=fn  cb是一个标识，一般服务器中定义的标识是callback，但是也可以按照约定来命名，百度中是"cb"
- 客户端的浏览器接收到返回的结果  ==>函数名(数据内容)

### 服务器端
- 接收客户端的请求，解析传递的URL参数
- 根据传递的参数到数据库中获取与用户输入的关键词相匹配的数据
- 把获取的结果返回给浏览器  fn({q:"z",p:false,s:["知乎","直播吧","支付宝","在线翻译","中国银行","智联招聘","战旗tv","中国知网","招商银行","真正男子汉第二季"]})

## 使用jQuery进行jsonp跨域请求
项目中通常这样用
```js
function fn(result) {
    console.log(result);
}

$.ajax({
    url:"https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=z",
    type:"get",//jsonp都是get请求，jquery默认就把缓存清掉了
    dataType:"jsonp",
    jsonp:"cb",//把callback变为cb,jQ中默认是callback
    jsonpCallback:"fn"//把随机函数名变为fn
});
```
