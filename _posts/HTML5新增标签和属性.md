---
title: HTML5新增标签和属性
date: 2017-02-02 18:44:20
categories: HTML5
---


# 基础特性
## 新增语义化标签
- header
网页的头部或这个区块的头部
- nav
网页的导航
- main
网页的主体内容
- footer
网页的尾部或这个区块的尾部
- section
一个区域或区块
- article
独立的区块  强调的是独立性，比如文章
- aside
通常表示侧边栏，表示与主体内容无关的内容
- hgroup
对整个页面/页面中的一个内容区块的标题进行组合
- figure
表示一段独立的流内容 元素经常用于图片
- figcaption
代表一个图例的说明

## h5标签兼容性处理
HTML5语义化标签在IE6-8下，对于不支持的标签不会有任何的样式,也默认的当成行内元素来出来,所以在样式表里要对这些标签定义一下 它默认的display
    <!--[if lt IE 9]>
        <script type="text/javascript" src="html5.min.js"></script>
    <![endif]-->
【条件注释法】
    gt--->当前版本以上的版本，不包含当前版本
    gte-->当前版本上的版本，包含当前版本
    lt-->当前版本以下的版本，不包含当前版本
    lte-->当前版本以下的版本，包含当前版本以

## 新增功能标签
- mark
高亮显示
- progress
进度条
- time
用来表现时间或日期
- datalist
选项列表与 input 元素配合使用，可以设置提示信息
```html
<input  type="text" id="" list="shopCars"/>
<datalist id="shopCars"> <!--为表单设置可选值-->
    <option value="aa"></option>
    <option value="ab"></option>
    <option value="c"></option>
</datalist>
```

- details
用于描述文档或文档某个部分的细节
summary标签和details一起使用,表示标题,用户点击标题时会得到细节信息
```html
<details>
    <summary>111</summary>
     <ul>
        <li>111</li><li>222</li><li>333</li>
     </ul>
</details>
```

- output
不同类型的输出,比如脚本的输出
```html
<form oninput="x.value= parseInt(a.value)+parseInt(b.value)">
    <input type="range" id="a" max="100" min="0" value="50"/>+
    <input type="number" id="b" value="50"/>
    <output name="x" for="a b">150</output>
</form>
```

## 新增表单种类
### 新增input元素的种类
- search
搜索框  出现的软键盘中右下角按钮由前往改成搜索
- tel
电话号码输入框  数字键盘（手机上才能显示）
- url
URL地址
- email
邮件输入框
- number
数字输入框
- rang
特定范围内的数值选择器(通过拖动滚动条改变一定范围内的数字)
- color
颜色选取器 只在 Opera 和 Blackberry 浏览器
- datetime
显示完整日期和时间 UTC标准时间（目前不支持）
- datetime-local
显示完整日期和时间
- time
显示时间
- month
显示月
- week
显示周

### 表单新特性
- placeholder
输入框占位符，常用作输入提示，在光标聚焦时，占位符自动消失
- autocomplete
是否保存用户输入值(规定输入字段是否应该启用自动完成功能。)   默认为on,关闭提示选择off
- autofocus
自动聚焦
- required
此项必填，不能为空
- pattern
正则验证  pattern="\d{1,5}“
- form
只要加上 form 属性，表单元素可以放到页面的任意位置。
- formnovalidate & novalidate
它俩都表示不需要验证表单,直接提交
novalidate 用于 form 标签
formnovalidate 用于 submit类型的提交按钮。
**表单验证**
验证不通过会触发invalid事件
表单元素上自带validity对象，该对象有如下属性：
当验证不通过时，这些属性的返回值如下：
- vaild
验证不通过时返回false
- patternMismatch
正则不匹配时返回true
- typeMismatch
输入类型不匹配时候返回true
- valueMissing
输入为空时返回true
通过设置setCustomValidity属性，可以改变默认的提示信息
```js
var oText=document.getElementById("oText");
oText.addEventListener("invalid",function () {
/*           console.log(this.validity);*/
    /*if(this.validity.patternMismatch){//验证不通过 true
        this.setCustomValidity("请输入2位数字");
    }else{
        this.setCustomValidity("");
    }*/
    /*if(this.validity.valueMissing){//验证不通过 true
        this.setCustomValidity("信息不能为空！");
    }else{
        this.setCustomValidity("");
    }*/
    if(this.validity.typeMismatch){//验证不通过 true
        this.setCustomValidity("类型不正确！");
    }else{
        this.setCustomValidity("");
    }
},false);
```

- hidden
元素隐藏
- spellcheck
对可编辑的内容纠错
- tabindex
按照顺序跳转  -针对表单元素
- contenteditable
true 可编辑
- window.document.designMode="on"
全局设置，页面上所有的内容都可以编辑

# 不常见的特性
## 防止网站钓鱼攻击
大多数使用 target ='_ blank' 的人都不知道一个有趣的事实——新打开的标签可以更改 window.opener.location 到一些网络钓鱼页面。它会在开放页面上代表你执行一些恶意 JavaScript 代码。因为用户相信打开的页面已安全，所以他们不会有所怀疑。
为了完全消除这个问题，HTML 5.1 已经通过隔离浏览器上下文的方式标准化了的 rel=”noopener”属性的用法。 rel =“noopener”可以在 `<a>` 和 `<area>` 标签中使用。
```html
<a href="#" target="_blank" rel="noopener">
  The link won't make trouble anymore
</a>
```

## 支持 Frame 的全屏
为 Frame 开发的布尔变量 allowfullscreen 属性允许您通过使用 requestFullscreen() 方法控制内容是否可以全屏显示。 例如，我们使用嵌入 YouTube 的播放器的 iframe 做示例。 需要设置 allowfullscreen 属性才能让播放器全屏显示视频。

## 图片零宽度
HTML 新版本允许你添加零宽度的图片。当图片不需要向用户展示时，可以使用此特性。假如一个 img 元素还有其他用途而不仅仅是展示一个图片，例如，作为一个服务的一部分用来计算页面视图个数，在 width 和 height 属性中使用 0 数值。对于 0 宽度的图片，推荐使用空属性。

```html
<img src="theimagefile.jpg" width="0" height="0" alt="">
```

## 浏览器的上下文菜单
在 HTML 5.1 中, 你可以使用 `<menu>` 标记来定义菜单，里面包含了一个或者多个 `<menuitem>` 元素, 然后利用 contextmenu 属性将其绑定到任何元素上。 `<menu>` 元素的 id 取值应该与我们想要为其添加上下文菜单的元素的 contextmenu 属性取值保持一致。

每一个 `<menuitem>` 都可以有如下三个表单项中的一个:
- radio – 从一个分组中获取选项；
- checkbox – 选择或者取消选择一个选项；
- command – 在点击时执行一个动作。

```html
<h2 contextmenu="popup-menu">
  Right click to get the context menu demo.
</h2>
<menu type="context" id="popup-menu">
  <menuitem type="checkbox" checked="true">Checkbox 1 </menuitem>
  <menuitem type="command" label="Command" onclick="alert('WARNING')">Checkbox 2</menuitem>
  <menuitem type="radio" name="group1">Radio button a</menuitem>
  <menuitem type="radio" name="group1" checked="true">Radio button b</menuitem>
  <menuitem type="checkbox" disabled>Disabled menu item</menuitem>
</menu>
```

## 在脚本和样式上使用加密随机数
加密随机数（cryptographic nonce ）是一个随机生成的数字，只能被使用一次, 而且针对每一次页面请求，它都得被生成出来。这个 nonce 属性可以被使用在 `<script>` 和 `<style>` 元素中。

它一般被用在一个网站的内容安全策略之中，以决定一个特定的样式和脚本是否应该在页面上被实现。在下面所提供的代码中，这个 value 是硬编码的，不过在实际的使用场景中，这个值是随机生成的。

```js
<script nonce='d3ne7uWP43Bhr0e'>
  The JavaScript Code
</script>
```
