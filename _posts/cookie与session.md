---
title: cookie与session
date: 2017-01-23 18:44:20
categories: Node
---

# cookie
> HTTP1.0中协议是无状态的，但在WEB应用中，在多个请求之间共享会话是非常必要的，所以出现了Cookie
cookie是为了辩别用户身份，进行会话跟踪而存储在客户端上的数据

## cookie的处理流程
- 第一次客户端向服务器发送请求
- 服务器通过响应头的Set-Cookie向客户端种植cookie
- 客户端再向服务器发送请求时，带上Cookie请求头
- 服务器通过读取请求头中的Cookie并进行相应

## cookie原理
- 种植cookie

```javascript
res.setHeader('Set-Cookie',"name=zfpx");
```

- 获取cookie

```javascript
//req.headers.cookie  获取到的形式==》visit=5; name=zfpx，所以要转化为对象
querystring.parse(req.headers.cookie,'; ','=')
```

## 案例-根据cookie判断用户第几次访问当前页面
```javascript
var http = require('http');
var querystring = require('querystring');
http.createServer(function(req,res){
    var urlObj = require('url').parse(req.url);
    var pathname = urlObj.pathname;
    //当客户端请求路径为/write时，服务器向客户端种植cookie
    if(pathname =='/visit'){
        var cookie=req.headers.cookie;
        var visitVal=1;
        if(cookie){//不是第一次访问
            visitVal = querystring.parse(cookie,'; ','=')['visit'];
            visitVal++;
        }
        res.setHeader('Set-Cookie',"visit="+visitVal);//种植cookie
        res.end('欢迎第'+visitVal+'次访问');
    }else{
        res.end('404');
    }
}).listen(8080);
```

## express使用cookie
- 引用第三方中间件cookie-parser

```javascript
var cookieParser=require('cookie-parser');
app.use(cookieParser());
```

- 种植cookie
```javascript
res.cookie('name','zfpx');
```

- 获取cookie

```javascript
req.cookies
```

## 案例-express统计用户第几次访问当前页面
```javascript
var express = require('express');
var app=express();
var cookieParser=require('cookie-parser');
app.use(cookieParser());
app.get('/visit',function(req,res){
    res.cookie('visit',isNaN(req.cookies.visit)?1:parseInt(req.cookies.visit)+1);
    res.send(req.cookies.visit);
});
app.listen(80);
```

## cookie的重要属性
res.cookie([key],[value],{domain|path|expires|maxAge|httpOnly});
//第三个参数是个对象，用来规定cookie种植的条件

- domain==>域名

    ```javascript
    res.cookie('name','zfpx',{domain:'a.zfpx.cn'});
    ```

- path==>路径

    ```javascript
    res.cookie('name','zfpx',{path:'/read'});
    ```

- expires==>过期时间

    ```javascript
    res.cookie('name','zfpx',{expires:new Date(Date.now()+10*1000)});
    ```

- maxAge==>有效时间

    ```javascript
    res.cookie('name','zfpx',{maxAge:10*1000});
    ```

- httpOnly==>不允许js访问cookie

    ```javascript
    res.cookie('name','zfpx',{httpOnly:true});
    ```

## 使用cookie的注意事项
- 可能被客户端篡改，使用前验证合法性
- 不要存储敏感数据，比如用户密码，账户余额
- 使用httpOnly保证安全
- 尽量减少cookie的体积
- 设置正确的domain和path，减少数据传输

# session
> session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而session保存在服务器上

客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，这就是session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了

## session的原理(基于cookie)
```javascript
/*需求：
* 第一次光临，送一张100元理发卡
* 第二次光临，余额90..
* 第三次光临，余额80..
* */
var express=require('express');
var cookieParser=require('cookie-parser');
var app=express();
app.use(cookieParser());
    
const SESSION_KEY='connect.sid';//定义常量
var sessions={};//在内存开辟一段区域
app.get('/visit',function (req,res) {
    var sessionId=req.cookies[SESSION_KEY];//sessionId其实就是卡号
    if(sessionId){//如果有卡号，表示是老顾客
        //通过卡号把卡号对应的对象从sessions对象取出来
        var sessionObj=sessions[sessionId];
        //在原来基础上减去10元
        sessionObj.balance-=10;
        res.end('欢迎您再次光临，您卡里的余额还剩'+sessionObj.balance+"元");
    }else{//如果没有卡号，新顾客，办卡
        //卡号要求是唯一的，并且不容易被猜到
        sessionId=''+Date.now()+Math.random();//变成字符串
        sessions[sessionId]={balance:100};//在服务器端记录此卡号对应的信息
        res.cookie(SESSION_KEY,sessionId);//把此卡号通过cookie发送给客户端
        res.end('欢迎你初次光临，送你一张100元理发卡');
    }
});
app.listen(80);
```

## express中session的使用
```javascript
var express=require('express');
//1、引入第三方中间件express-session
var session=require('express-session');
var app=express();
    
//2、使用express-session
app.use(session({
    resave:true,//每次访问服务器，重新保存session
    saveUninitialized:true,//保存未初始化的session
    secret:'zfpx'//用来加密cookie
}));//一般来说，中间件模块导出对象都是一个函数，调用它可以返回一个中间件函数
    
app.get('/write',function (req,res) {
    req.session.username='zfpx';//赋值
    res.end('write');
});
app.get('/read',function (req,res) {
    res.send(req.session.username);//取值
});
app.listen(80);
```

# cookie与session区别
- cookie数据存放在客户的浏览器上，session数据放在服务器上。
- cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗 考虑到安全应当使用session
- session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能 考虑到减轻服务器性能方面,应当使用COOKIE
- 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie
`