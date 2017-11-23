---
title: 深入了解CSS3新特性
date: 2016-12-18 18:44:20
categories: CSS3
---

# CSS3选择器
## 结构选择器
- :nth-child(n)
获取第几个元素（从1开始设置）
- :nth-child(2n)
获取偶数元素（从0开始设置）
- :nth-child(2n+1)
获取奇数元素
- :nth-of-type(n)
某个元素下的同种类型的子元素的第几个
- :first-child  ->nth-child(1)
获取第一个子元素
- :first-of-type ->nth-of-type(1)
获取同种类型的第一个子元素
- :last-child
获取最后一个子元素
- :last-of-type
获取同种类型的最后一个子元素
- :only-child
仅有一个子元素
- :only-of-type
同种类型的子元素只有一个
- :empty
不包含任何的元素节点，文本节点

## 否定选择器
- :not()
括号里写的是任意的选择器

## 属性选择器
- E[attr=val]
- E[attr|=val]
只能等于val  或只能以val-开头
- E[attr*=val]
包含val字符串
- E[attr~=val]
属性值有多个,其中有一个是val
- E[attr^=val]
以val开头
- E[attr$=val]
以val结尾

## 伪类目标选择器
- :target
用来匹配url指向的目标元素  当前活动的目标元素起作用，存在url指向该匹配元素时,样式效果才会生效

## 伪元素
利用content特性，能够将css中的内容在页面中显示出来，但是不会生成DOM节点
有两个冒号，但是一个也可以
- : first-line
匹配第一行文本
- : first-letter
匹配首字符
- : before & : after
DOM元素前后插入额外的内容

# border-radius
> 边框是圆角

border-radius: 1-4个数字 / 1-4个数字
前面是水平半径，后面是垂直半径
四个数字方向分别是左上  右上  右下  左下
不给“/”则水平半径和垂直半径一样
```css
border-radius: 2em 1em 4em / 0.5em 3em;

//等价于
border-top-left-radius: 2em 0.5em;
border-top-right-radius: 1em 3em;
border-bottom-right-radius: 4em 0.5em;
border-bottom-left-radius: 1em 3em;
```

# 渐变
## 线性渐变
> 只能用在背景上，颜色是沿着一条直线轴变化

**语法**：linear-gradient([<起点> || <角度>,]? <点>, <点>…)
**参数**：
- 起点/角度
起点：从什么方向开始渐变  left、top、center、right、bottom
角度：从什么角度开始渐变  xxx deg的形式
- 点：渐变点的颜色和位置
red 50%，位置可选
```css
background: linear-gradient(45deg,#00b38a 10%,#f8f800 20%,#ed0000 30%);
//重复的径向渐变：
background: repeating-linear-gradient(45deg,#00b38a 10%,#f8f800 20%,#ed0000 30%);
```

## 径向渐变
> 从“一个点”向多方向颜色渐变

**语法**：radial-gradient([[<shape> || <size>] [at <position>]?,| at <position>,]?<color-stop>[,<color-stop>]+);
**参数**：
- 形状/大小
shape形状 ：ellipse、circle  或设置水平半径,垂直半径
size渐变的大小，即渐变到哪里停止，有如下关键词:
closest-side：最近边
farthest-side：最远边
closest-corner：最近角
farthest-corner：最远角 (默认值)
- position ：关键词|数值|百分比
```css
background: radial-gradient(100px 200px at center,#f8f800 10%,#00ffff 50%,#f600f6 40%);
//重复的径向渐变：
background: repeating-radial-gradient(100px 200px at center,#f8f800 10%,#00ffff 50%,#f600f6 40%);
```

# 背景
## background-origin
> 背景图的起始位置

- border-box
从border区域显示
- padding-box
从padding区域显示（默认值）
- content-box
从content区域显示

## background-clip
> 背景图的裁剪位置

- border-box
从border区域往外裁剪（默认值）
- padding-box
从padding区域向外裁剪
- content-box
从content区域往外裁剪
- text
文本裁剪

## background-size
> 背景图的大小

- 100% 100%
百分比
- 10px 10px
数值
- contain
按原始比例收缩,背景图显示完整,但不一定铺满整个容器
- cover
按原始比例收缩,背景图可能显示不完整,但铺满整个容器

## background-attachment
> 背景图片是滚动的还是固定的 fixed(固定的) 默认是滚动的

# 滤镜
-webkit-filter: [val]
值有以下几种：
- normal;
正常
- grayscale
灰度
- sepia
褐色
- saturate
饱和度
- hue-rotate
色相旋转
- invert
反色
- opacity
透明度
- brightness
亮度
- contrast
对比度
- blur
模糊
- drop-shadow
阴影

# 盒子阴影
**语法**：box-shadow: h  v  blur spread color inset;
X偏移(正->右，负->左)  Y偏移(正->下，负->上)  模糊半径  扩展半径  阴影颜色  外/内阴影（默认外阴影）
ps:设置单阴影：通过扩展半径
```css
.box1{
    box-shadow: 10px 0 10px -5px green ;
    /*X偏移 Y偏移 模糊半径 扩展半径 阴影颜色 外/内阴影（默认外阴影）*/
    box-shadow:0 0 0 10px green;//只设置扩展半径，相当于设置边框，不会算在盒子模型里面
}
```

# 文本阴影
**语法**：text-shadow : x y blur  color
X偏移(正->右，负->左)  Y偏移(正->下，负->上)  模糊半径  阴影颜色
多层阴影制作文字立体效果 ,设置多种颜色,中间以逗号隔开(后面偏移值>前面偏移值)
```css
.box2{
    text-shadow: 1px 1px 2px green, 2px 2px 2px deepskyblue,3px 3px 2px purple;
}
```

# 镂空字体
```css
.box3{
    width:300px;
    font-size: 60px;
    -webkit-text-stroke: 2px blue;
    color: transparent;
}
```

# 遮罩
```
.box4{
    width:300px;
    height: 300px;
    background: url("img/pic.jpg");
    background-size:cover;
    -webkit-mask-image:url(img/mask.png) ;//遮罩图片
    -webkit-mask-repeat: no-repeat ;//遮罩是否重复
    -webkit-mask-position:14px 70px ;//遮罩的位置
}
```