---
title: Ajax详解
date: 2016-12-08 18:44:20
categories: JavaScript
---

# 什么是AJAX?
> AJAX是Asynchronous JavaScript And XML(异步的JavaScript和XML)的缩写
用来实现客户端与服务器端的少量数据交互，在不重新加载整个页面的情况下，就可以局部更新网页内容

按照AJAX的定义来看，客户端从服务器端获取的数据应该是XML格式的数据，但是真实项目中最常用的却是JSON格式的数据，所以客户端从服务器端获取的数据格式常见的有这几种：JSON格式/XML格式/文件流格式

# AJAX原理（4步）
```js
//1、创建一个AJAX对象
var xhr=new XMLHttpRequest();

//2、打开一个url地址，给AJAX对象做发送前的一些基础配置
xhr.open([请求方式],[请求地址],[同步/异步（默认）])

//3、监听AJAX状态改变，在不同的状态下做不同的事情
xhr.onreadystatechange=function(){
    if(xhr.readyState==4&&xhr.status==200){
        var data=xhr.responseText;
    }
}

//4、发送请求
xhr.send();
```

# AJAX各部分详解
## 请求方式
### GET系列
给服务器的少，从服务器获取的多
- GET 一般应用于向服务器获取数据
- DELETE  一般应用于向服务器上删除文件
- HEAD  一般应用于客户端只想获取服务器返回的“响应头信息

### POST系列
给服务器的多，从服务器获取的少
- POST  向服务器增加资源文件
- PUT  在服务器上放一个文件
ps:不管是哪一种请求方式，客户端都可以给服务器传递内容，也可以从服务器获取内容

### GET系列和POST系列有哪些区别?
GET系列传递给服务器内容都是通过问号传参的方式传过去的
POST系列是通过请求主体传给服务器的
- 内容大小
GET是在URL末尾追加问号传参传递的，如果传递的内容过多，URL就会变得很长，但是浏览器都有一个最大URL长度限制（谷歌8KB，火狐7KB, IE2KB）
POST理论上没有传递大小的限制，但是真实项目中，为了保证传输的速度，我们自己会做限制，例如限制上传的内容在xxxKB以内
- 安全性
GET不安全，POST相对安全
GET请求容易被URL劫持，导致传递给服务器的数据泄露
- 缓存问题
GET请求可能会产生缓存(我们不可控)，POST不会
如何清除缓存？===>在URL后面追加一个随机数/时间戳
`xhr.open("get","/temp.xml?name=zf&_="+Math.random());`

## 请求地址
客户端是通过这个地址先找到服务器的对应的服务和对应接口，然后发个请求获取自己需要的数据；真实项目中获取数据的这个地址都是后台开发给前端的（API文档）

## AJAX的同步与异步
async/sync:设置同步还是异步，默认是true，代表异步，写false代表同步
【AJAX中的同步】：当AJAX任务开始的时候（xhr.send）一直需要到readyState=4的时候，任务才结束，此时才可以处理其他的事情
【AJAX中的异步】：当AJAX任务开始的时候（xhr.send）不需要到readyState=4，依然可以处理其他的事情，只有当其他任务完成后，我们再看是否到4，到达4后，再做一些相关的操作

## xhr.readyState
AJAX的状态码：代表AJAX的处理状态
    0  UNSENT  未发送，创建完成一个XHR的时候默认值就是0
    1  OPENED  已打开，已经执行xhr.open了
    2  HEADERS_RECEIVED  服务器响应头信息已经被客户端接收 已经执行xhr.send
    3  LOADING   服务器响应主体的内容正在返回
    4  DONE  服务器响应主体内容接收完成
AJAX请求数据这件事从xhr.send开始，到xhr.readyState=4的时候才算结束

## xhr.status
服务器响应状态码，服务器可以通过不同的码，反映出最后的结果
【2开头 请求成功】
    200  成功，以2开头的状态码都是成功  响应主体的内容已经成功返回了
【3开头 重定向】
    301  永久重定向（永久转移）
    302  临时重定向，处理服务器的负载均衡
            服务器的负载均衡  服务器集群
            当访问人数过多，当前服务器处理不过来，会临时转移到另外一台服务器上处理
    304  读取缓存数据（这个是自己在服务器上做的处理）
    ctrl+f5  清除当前页面缓存
【4开头  请求错误】
    400  请求参数错误
    401  无权限访问
    404  请求地址不存在
    ==》4开头都是客户端的问题
【5开头  服务器错误】
    500  未知的服务器错误
    503  服务器超负荷
    ==》5开头的都是服务器错误

# AJAX常用属性和方法
## 常用方法(7个)
- 设置请求头：setRequestHeader([key],[value])
- 获取请求头：getRequestHeader([key])
- 设置响应头：setResponseHeader([key],[value])
- 获取响应头：getResponseHeader([key])
- 打开URL地址：open([request method],[request url],[async:true/false]);
- 设置请求主体：send(data)
- 终止AJAX请求  abort()

## 属性：
- 获取响应主体：xhr.responseText/xhr.responseXML/xhr.response
- responseType  响应类型
- responseURL  响应URL
- timeout  超时时间
- onreadystatechange: 状态码改变
- readyState: 请求的状态。
- status: 服务器的HTTP状态码（200对应OK，404表示Not Found(未找到)，等等);
- statusText: HTTP状态码的相应文本(OK或Not Found等等).

# AJAX库
```js
/*-封装属于我们自己的AJAX方法库-*/
//创建AJAX对象（处理兼容性）
function createXHR() {
    var xhr=null,
        flag=false,
        ary=[
            function () {
                return new XMLHttpRequest();
            },
            function () {
                return new ActiveXObject("Microsoft.XMLHTTP");
            },
            function () {
                return new ActiveXObject("Msxml2.XMLHTTP");
            },
            function () {
                return new ActiveXObject("Msxml3.XMLHTTP");
            }
        ];
    for(var i=0;i<ary.length;i++){
        var curFn=ary[i];
        try{
            xhr=curFn();
            createXHR=curFn;
            flag=true;
            break;
        }catch (e){
        }
    }
    if(!flag){
        throw new Error("your browser is not support ajax,please change your browser,try again!");
    }
    return xhr;
}
//AJAX方法
function ajax(opts) {
    var _default={
        url:"",
        type:"get",
        dataType:"text",
        data:null,
        async:true,
        cache:true,
        success:null,
        error:null,
        header:null
    };
    for(var key in opts){
        if(opts.hasOwnProperty(key)){
            _default[key]=opts[key];
        }
    }
    var mark=_default.url.indexOf("?")>=0?"&":"?",
        regGet=/^(get|delete|head)$/i;
    if(_default.data){
        if(regGet.test(_default.type)){
            _default.data=formatData(_default.data);
            _default.url+=mark+_default.data;
            _default.data=null;
        }else{
            _default.data=JSON.stringify(_default.data);
        }
    }
    if(regGet.test(_default.type)&&_default.cache==false){
        mark=_default.url.indexOf("?")>=0?"&":"?";
        _default.url+=mark+"_="+Math.random();
    }
    var xhr=createXHR();
    xhr.open(_default.type,_default.url,_default.async);
    xhr.onreadystatechange=function () {
        if(/^(?:2|3)\d{2}$/.test(xhr.status)){
            if(xhr.readyState==2){
                var serverTime=new Date(xhr.getResponseHeader("Date"));
                _default.header&&_default.header.call(xhr,serverTime);
            }
            if(xhr.readyState==4){
                var val=xhr.responseText;
                _default.dataType=_default.dataType.toUpperCase();
                switch (_default.dataType){
                    case "JSON":
                        val="JSON" in window?JSON.parse(val):eval("("+val+")");
                        break;
                    case "XML":
                        val=xhr.responseXML;
                        break;
                }
                _default.success&&_default.success.call(xhr,val);
            }
        }
        _default.error&&_default.error.call(xhr,{
            status:xhr.status,
            statusText:xhr.statusText
        });
    };
    xhr.send(_default.data);
}
//将JSON对象格式化为URL中的查询字符串
function formatData(obj) {
    if(({}).toString.call(obj)!=="[object Object]"){
        return;
    }
    var result="";
    for(var key in obj){
        if(obj.hasOwnProperty(key)){
            result+=key+"="+obj[key]+"&";
        }
    }
    result=result.substr(0,result.length-1);
    return result;
}
```

# jQuery中的AJAX
## $.ajax()
```js
$.ajax({
    url:"data.txt",//请求地址
    type:"get",//请求方式
    dataType:"text",//预设获取数据的内容格式，注意，这不是服务器端处理的数据格式，而是ajax方法处理的
    data:null,//传递给服务器的内容放在DATA中
    async:true,//设置同步还是异步
    cache:false,//清除GET系列请求的缓存
    timeout:10000,//设置超时时间
    context:this,//为所有回调函数规定this值
    jsonp:'cb',//在一个 jsonp 中重写回调函数的字符串。
    username:'xy',//规定在 HTTP 访问认证请求中使用的用户名。
    password:1234,//规定在 HTTP 访问认证请求中使用的密码。
    success:function (result) {
        //请求成功执行这个回调函数，REQUEST是获取的响应主体内容
    },
    error:function (msg) {
        //请求失败，MSG中存储的就是失败的原因
    },
    complete:function () {
        //不管成功还是失败都会执行这里，它代表完成
    }
});
```

## $.get()
使用 HTTP GET 请求从服务器加载数据
```js
$.get('data.txt',function(data,status){
    console.log(data);
    console.log(status);
})
```

## $.post()
使用 HTTP POST 请求从服务器加载数据
```js
//使用 AJAX 的 POST 请求来改变 <div> 元素的文本：
$("input").keyup(function(){
    txt=$("input").val();
    $.post("demo_ajax_gethint.html",{suggest:txt},function(result){
        $("span").html(result);
    });
});
```