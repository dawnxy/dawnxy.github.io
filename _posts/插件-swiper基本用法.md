---
title: 插件-swiper基本用法
date: 2017-02-13 18:44:20
categories: JavaScript
---

# Swiper插件简介
Swiper是纯javascript打造的滑动特效插件，面向手机、平板电脑等移动终端。

Swiper能实现触屏焦点图、触屏Tab切换、触屏多图切换等常用效果。

# Swiper使用案例-H5单屏页面滑动
## 加载插件
需要用到的文件有swiper.min.js和swiper.min.css文件

## 页面布局
```html
<div class="swiper-container">
    <div class="swiper-wrapper">
        <div class="swiper-slide">Slide 1</div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
    </div>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>

    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>

    <!-- 如果需要滚动条 -->
    <div class="swiper-scrollbar"></div>
</div>
```

## 移动端适配方案
```js
var desW=640;
var winW=document.documentElement.clientWidth;
document.documentElement.style.fontSize=winW/desW*100+"px";
```

## swiper的使用
以下都是一些常用的参数，如有需要，请参考官方文档：http://www.swiper.com.cn/usage/index.html
```js
var mySwiper=new Swiper(".swiper-container",{
    direction:"horizontal" ,//horizontal横向 vertical纵向
    pagination:".swiper-pagination",//分页效果
    speed:2000,//轮播速度
    initialSlide:0,//加载页面时显示哪一屏(默认第一屏)
    autoplay:1000,//自动轮播时间间隔
    autoplayDisableOnInteration:false,//用户操作swiper之后，是否禁止autoplay。默认为true：停止。
    paginationType:"progress",//分页类型--》进度条
    effect:"coverflow",//轮播效果  cube/fade/coverflow/flip
    prevButton:".swiper-button-prev",//前切换按钮
    nextButton:".swiper-button-next",//后切换按钮
    loop:true,//在循环模式下swiper会自动帮我们初始化
    lazyLoading:true,//图片延迟加载
    lazyLoadingInPrevNext : true,//提前对前一张或后一张做延迟加载
    slideActiveClass:"active"
    onInit:function (swiper) {
        //swiper初始化后出发这个函数
    },
    onTransitionEnd:function (swiper) {
        //滑块滑动结束时出发这个函数

        //以下代码用来处理滑到最后一页还可以继续滑动到第一页，循环滑动的效果
        var curIndex=swiper.activeIndex;//当前显示滑块的索引
        var slides=swiper.slides;//获得所有的滑块
        [].forEach.call(slides,function (item,index) {
            item.id="";
            if(index==curIndex){
                if(index==0){
                    slides[index].id="page"+slides.length-2;
                }else if(index==(slides.length-1)){
                    slides[index].id="page1";
                }else{
                    slides[index].id="page"+index;
                }
            }
        });
    }
});
```

## music区域
```html
//html
<div class="music">
    <div class="control"><audio src="audio/serenade.mp3" loop id="serenade" preload="none"></audio></div>
    <div class="control_after"></div>
</div>
```

```css
//css
.music{ position: absolute; right:0; top:0.2rem; z-index: 100; width:1rem; height: 1rem; opacity: 0;}
.control{ position: absolute; top:0; left: 0; width:100%; height: 100%; background: url(../imgs/music.gif) no-repeat; -webkit-background-size: 100% 100%; background-size: 100% 100%;}
.control_after{ position: absolute; top:0.1rem; left:0.1rem; z-index: -1; width:0.6rem; height: 0.6rem; background: url(../imgs/music_off.png) no-repeat; -webkit-background-size: 100% 100%; background-size: 100% 100%;}
.musicCur .control_after{ -webkit-animation: rotate 5s linear infinite; animation: rotate 5s linear infinite;}
.pause{ background: none;}
```

```js
//js
var music=document.querySelector(".music");
var control=document.querySelector(".control");
var serenade=document.querySelector("#serenade");
//隔1秒后让音乐播放，并增加转动效果
window.setTimeout(function () {
    serenade.play();
    music.style.opacity=1;
    music.className="music musicCur"
},1000);
//绑定点击事件，如果播放则停止，如果停止则播放
//通过serenade.paused判断音频文件是否播放，如果是播放，则值是false，如果是停止的，则值是true
music.addEventListener("click",function () {
    if(serenade.paused){//如果停止
        serenade.play();//就播放
        music.className="music musicCur";
        control.className="control";
    }else{//如果播放
        serenade.pause();//就停止
        music.className="music";
        control.className="control pause";
    }
},false);
```
