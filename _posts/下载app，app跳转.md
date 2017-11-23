---
title: 下载app，app跳转
date: 2017-11-12 18:44:20
categories: 实战笔记
---

下载app两种方式：
- 在微信浏览器给予用户提示，“用浏览器打开”（微信会过滤apk文件，不能直接下载或者打开app）
- 跳转到腾讯应用宝，一切交给应用宝 [http://a.app.qq.com/o/simple.jsp?pkgname=com.kkd.kkdapp](http://a.app.qq.com/o/simple.jsp?pkgname=com.kkd.kkdapp)

```javascript
//下载app
var UA = window.navigator.userAgent;
//终端
var os = {
    'android': UA.match(/(Android);?[\s\/]+([\d.]+)?/),
    'iphone': UA.match(/(iPhone\sOS)\s([\d_]+)/)
};
if(os.iphone){//ios
    if(channel=="iOS"){//app

    }else{//h5
        if(client.weixin){
            window.location.href  = "${rc.getContextPath()}/wxDownload.html";
        }else{
            var ifr = document.createElement('iframe');
            ifr.src = 'kkdApp://';
            ifr.style.display = 'none';
            document.body.appendChild(ifr);
            window.setTimeout(function(){
                window.location.href="itms-apps://itunes.apple.com/app/id938522778";//ios app下载链接
            },2000);
        }
    }
}else if(os.android){//android
    if(channel=="Android"){//app

    }else{//h5
        if(client.weixin){
            window.location.href  = "${rc.getContextPath()}/wxDownload.html?fromUrl=activity";
        }else{
            var ifr = document.createElement('iframe');
            ifr.src = 'kkdapp://splash';
            ifr.style.display = 'none';
            document.body.appendChild(ifr);
            window.setTimeout(function(){
                document.body.removeChild(ifr);
                window.location.href="http://image.kuaikuaidai.com/apk/android/kuaikuaidai.apk";//安卓app 下载链接
            },2000);
        }
    }
}
```