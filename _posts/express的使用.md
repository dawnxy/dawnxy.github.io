---
title: express的使用
date: 2017-01-28 18:44:20
categories: Node
---

什么是express?

> express是一个基于Node.js的web应用开发框架，是目前最流行的基于Node.js的web开发框架，它提供一系列强大的功能，例如：

- 1>.路由控制
- 2>.参数获取
- 3>.中间件
- 4>.send和sendFile
- 5>.静态文件服务
- 6>.模板解析
- 7>.重定向

# 初始化包
```bash
npm init -y
```

# 安装模块
```bash
npm install express body-parser ejs --save
```

如果不初始化包，默认会装到根目录下，保证所有项目都能找到node_modules

# 路由控制
> 根据客户端不同的请求，返回不同的响应

```javascript
var express=require('express');
var app=express();
app.get('/',function(){
    res.end('首页');
});
app.get('/about',function(){
    res.end('关于我们');
});
app.listen(80);
```

# 参数获取
- 请求方法：req.method
- 域名：req.headers.host.split(‘:’)[0])
- 请求路径：req.path
- order值：req.query.order
- 端口号：req.headers.host.split(‘:’)[1])
- 用户id：req.params.id

```javascript
var express=require('express');
var app=express();
//localhost:9090/usrs/1?order=asc
app.get('/users/:id',function (req,res) {
   console.log('用户id：'+req.params.id);
    res.end();
});
app.listen(9090);
```

# 中间件
什么是中间件？

> 中间件就是处理HTTP请求的函数。用来完成各种特定的任务 比如检查用户是否登录、检测用户是否有权限访问等。

## 中间件的功能
- 执行任何代码。
- 修改请求和响应对象。
- 终结请求-响应循环。
- 调用堆栈中的下一个中间件。

## 中间件的分类
### 应用级中间件

```javascript
app.use(function (req,res,next) {
    if(req.query.username=='admin'){
        next();
    }else{
        res.end('无权限访问！');
    }
});
```

### 路由级中间件

路由级中间件和应用级中间件一样，只是它绑定的对象为 express.Router()。

```javascript
var express=require('express');
var router=express.Router();
router.get('/signup',function(){
});
router.get('/signin',function(){
});
module.exports=router;
    
//根据路径来区分执行不同的中间件
app.use('/user',user);//用户类的中间件
app.use('/article',article);//文章类的中间件
```

### 错误处理中间件

错误处理中间件有 4 个参数，定义错误处理中间件时必须使用这 4 个参数。即使不需要 next 对象，也必须在签名中声明它，否则中间件会被识别为一个常规中间件，不能处理错误。

```javascript
app.use(function (req,res,next) {
    console.log(1);
    next('出错');//如果调用next的时候传递了参数就表示出错了，那么就会跳过正常的中间件或路由，执行错误处理中间件
});
    
//错误处理中间件
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

### 内置中间件

express.static()是expres中唯一的内置中间件，负责在 Express 应用中提托管静态资源。每个应用可以有多个静态目录

```javascript
app.use(express.static(path.resolve('public')));
app.use(express.static(path.resolve('uploads')));
app.use(express.static(path.resolve('files')));
```

### 第三方中间件

body-parser cookie-parser express-session

以下是对body-parser中间件的使用：

```bash
npm install body-parser
```

```javascript
var express = require('express');
var app = express();
    
//1、引入body-parser
var bodyParser = require('body-parser');
    
//2、使用bodyParser
app.use(bodyParser.urlencoded({extended:true}));//解析查询字符串格式
app.use(bodyParser.json());//解析json格式
app.post('/signup',function(req,res){
    //req.body==>得到的请求主体
})
```

# send和sendFile
```javascript
var obj={name:'zfpx',age:8};
    
//send
app.get('/obj',function (req,res) {
    // res.end(obj);//end只能返回字符串类型和Buffer格式的的数据
    res.send(obj);//send是express给res对象上增加的方法，支持任意类型的参数  对象 数字 字符串 Buffer，可以自动处理编码
});
    
//sendFile
app.get('/index',function (req,res) {
    //sendFile只支持绝对路径，三种方式请求到文件
    // res.sendFile(path.join(__dirname,'./index.html'));
    // res.sendFile(path.resolve('./index.html'));
    res.sendFile('./index.html',{root:__dirname});
});
    
//sendStatus
app.all('/*',function (req,res) {
    //如果返回一个状态码的话，需要用sendStatus
    res.sendStatus(404);
});
```

# 静态文件服务
> 静态文件服务：用来处理静态文件(css js imgs)

当接到客户端的请求后，会去静态文件根目录下找有没有这个文件，如果有则把它从硬盘上读出来返回给客户端，如果没有，则调用next向下继续匹配

```javascript
app.use(express.static(path.resolve('public')));
```

# 模板解析
> 模板引擎：模板+数据+渲染

## 安装模板引擎ejs
```bash
npm install ejs --save
```

## 模板引擎使用步骤
```javascript
//1.定义数据
var title="模板引擎";
var users=[
    {name:'zfpx1',id:1},
    {name:'zfpx2',id:2},
    {name:'zfpx3',id:3}
];
    
//2.设置模板
//1>.指定渲染模板文件的后缀名为html
app.set('view engine','html');  //view engine==>定死的，不能改
    
//2>.设置模板引擎的绝对路径
app.set('views',path.resolve('views'));
    
//3>.修改模板引擎的后缀
app.engine('html',require('ejs').__express);
    
//3.渲染视图
app.get('/user',function (req,res) {
    res.render('user',{title,users});
});
```

# 重定向
> 重定向：告诉客户端向另外一个路径发送请求

```javascript
res.redirect('/error');//重定向到错误页
res.redirect('back');//回到上一个页面
```


