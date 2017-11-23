---
title: 问号传参和JSON对象之间的解析
date: 2017-03-10 18:44:20
categories: JavaScript
---

# 问号传参转化为JSON对象
name=1&age=34   ===>   {name: "1", age: "34"}
```js
function QueryURLParameter(url) {
    var reg=/([^?&#=]+)=([^?&#=]+)/g,
            obj={};
    url.replace(reg,function () {
        obj[arguments[1]]=arguments[2];
    });
    return obj;
}

var url="http://www.baidu.com?name=1&age=34";
console.log(QueryURLParameter(url))
```

QueryURLParameter扩展到字符串原型上
```js
(function (pro) {
    //获取URL问号后面的参数值，最后以对象键值对的方式存储
    function QueryURLParameter() {
        var reg=/([^?&#=]+)=([^?&#=]+)/g,
                obj={};
        this.replace(reg,function () {
            obj[arguments[1]]=arguments[2];
        });
        return obj;
    }
    pro.QueryURLParameter=QueryURLParameter;//将QueryURLParameter扩展到字符串的原型上
})(String.prototype);

var url="http://www.baidu.com?name=1&age=34";
var obj=url.QueryURLParameter();
console.log(obj);//{name: "1", age: "34"}
```

# JSON对象转化为问号传参
{name: "1", age: "34"}  ====>  name=1&age=34
```js
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
var obj={name: "1", age: "34"};
var str=formatData(obj);
console.log(str);//name=1&age=34
```