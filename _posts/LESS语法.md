---
title: LESS语法
date: 2017-02-11 18:44:20
categories: Less
---

什么是less？
> Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。Less 可以运行在 Node 或浏览器端。

# less的编译
## 客户端编译
> 适合于本地开发环境，浏览器没法直接解析less代码，需要依赖于less解析器

在客户端使用 Less.js 是最容易的方式，并且在开发阶段很方便，但是，在生产环境中，性能和可靠性非常重要，我们建议最好使用 node.js 或其它第三方工具进行预编译。

在页面中加入 .less 样式表的链接，并将 rel 属性设置为 "stylesheet/less"：
```css
<link rel="stylesheet/less" type="text/less" href="index.less">
```

接下来，下载 less-2.5.3.min.js 并通过 `<script>` 标签将其引入：
```js
<script>
    var less={
        "env":"development",//开发环境
        "poll":1000//在监视模式下，监视代码的间隔时间
    }
</script>
<script src="less-2.5.3.min.js"></script>
<script>
    less.watch();//在监视环境下
</script>
```

监视模式是客户端的一个功能，这个功能允许你当你改变样式的时候，客户端将自动刷新。
要使用它，只要在URL后面加上’#!watch’，然后刷新页面就可以了。另外，你也可以通过在终端运行less.watch()来启动监视模式。

- 务必确保在 less.js 之前加载你的样式表。
- 如果加载多个 .less 样式表文件，每个文件都会被单独编译。因此，一个文件中所定义的任何变量、mixin 或命名空间都无法在其它文件中访问。
- 由于浏览器端使用时是使用ajax来拉取.less文件，因此直接在本机文件系统打开(即地址是file开头)或者是有跨域的情况下会拉取不到.less文件，导致样式无法生效。
**Less CDN 加速**：
`<script src="http://cdn.bootcss.com/less.js/1.7.0/less.min.js"></script>`

## 服务器端编译
### 安装less
```bash
npm install less -g//全局安装
npm install less --save-dev//本地安装，开发依赖
```

### 检测less版本
```bash
lessc -v
```

### 编译
```bash
lessc index.less
lessc index.less>index.css//将index.less编译到指定文件index.css
lessc index.less>index.min.css-x//将index.less编译指定文件index.min.css并压缩
```

当然除了上述的几种方式还有其他的方式如:通过编译工具 kala
格局更高的，使用gulp、webppack 搭建工程化自动打包、编译、压缩、检测、上线等
# less的语法
## 变量
变量作为属性 选择器 字符串的调用形式：@{val}
如果是作为值的调用形式： @val
### @符号定义
```css
@base: #f938ab;
.box {
    color: @base; /*变量引用*/
}
```

变量的作用就是把值定义在一个地方（或一个文件里，通过@import导入），然后在各处使用，这样能让代码更易维护。

### 定义url变量
```css
@images: "../img";
// 用法
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```

### 定义变量属性
```css
@property: color;
.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```

## 混合
在 LESS 中我们可以定义一些通用的属性集为一个class，然后在另一个class中去调用这些属性. 下面有这样一个class:
### 不带参数的混合
```css
@base: #f938ab;
.ellipsis_txt {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    overflow: hidden;
    word-break: break-all;
    text-overflow: ellipsis;
    -webkit-box-orient: vertical;
}
//如果.ellipsis_txt()=>.ellipsis_txt本身不会被编译，只供其他样式调用
.box {
    color: @base;
    .ellipsis_txt;
    /*或者
    .ellipsis_txt(); //括号是可选的
    */
}
```

任何 CSS class, id 或者 元素 属性集都可以以同样的方式引入.

### 带参数的混合
在 LESS 中，你还可以像函数一样定义一个带参数的属性集合:
```css
.border-radius (@radius) {
  border-radius: @radius;
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
}
```

然后在其他的class这样来调用
```css
#header {
  .border-radius(4px);
}
.button {
  .border-radius(6px);
}
```

#### 设置默认的参数
```css
.border-radius (@radius: 5px) {
  border-radius: @radius;
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
}
```

所以现在如果我们像这样调用它的话:
```css
#header {
  .border-radius;  //radius的值就会是5px.
}
```

#### 使用但是不暴露属性集合
你也可以定义不带参数属性集合,如果你想隐藏这个属性集合，不让它暴露到CSS中去，但是你还想在其他的属性集合中引用，你会发现这个方法非常的好用:
```css
.wrap () {
  text-wrap: wrap;
  white-space: pre-wrap;
  white-space: -moz-pre-wrap;
  word-wrap: break-word;
}
pre { .wrap }
```

输出：
```css
pre {
  text-wrap: wrap;
  white-space: pre-wrap;
  white-space: -moz-pre-wrap;
  word-wrap: break-word;
}
```

#### @arguments变量
@arguments包含了所有传递进来的参数. 如果你不想单独处理每一个参数的话就可以像这样写:
```css
.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);
```

将会输出：
```css
box-shadow: 2px 5px 1px #000;
-moz-box-shadow: 2px 5px 1px #000;
-webkit-box-shadow: 2px 5px 1px #000;
```

#### 多参数混合
多个参数可以使用分号或者逗号分隔，推荐使用分号分隔，因为逗号有两重含义：它既可以表示混合的参数，也可以表示一个参数中一组值的分隔符。
使用同样的名字和同样数量的参数定义多个混合是合法的。在被调用时，LESS会应用到所有可以应用的混合上。比如你调用混合时只传了一个参数.mixin(green)，那么所有只强制要求一个参数的混合都会被调用：
```css
.mixin(@color) {
    color-1: @color;
}
.mixin(@color; @padding:2) {
    color-2: @color;
    padding-2: @padding;
}
.mixin(@color; @padding; @margin: 2) {
    color-3: @color;
    padding-3: @padding;
    margin: @margin @margin @margin @margin;
}
.some .selector div {
    .mixin(#008000);
}
```

编译结果
```css
.some .selector div {
    color-1: #008000;
    color-2: #008000;
    padding-2: 2;
}
```

#### 使用表达式
```css
.average(@x, @y) {
  @average: ((@x + @y) / 2);
}
div {
  .average(16px, 50px); // "call" the mixin
  padding: @average;    // use its "return" value
}
```

编译结果
```css
div {
  padding: 33px;
}
```

## 嵌套
可以在一个css里有多个css块，以方便我们更好的组织代码，编写css模板。
```css
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

生成
```css
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```

支持&符
```css
#header {
  color: black;
  &-navigation {
    font-size: 12px;
  }
  &-logo {
    width: 300px;
  }
  &:hover{
    color:#ccc;
  }
}
```

生成
```css
#header {
  color: black;
}
#header-navigation {
  font-size: 12px;
}
#header-logo {
  width: 300px;
}
#header:hover {
  color: #ccc;
}
```

## 运算
任何数字、颜色或者变量都可以参与运算。
```css
@base: 5%;
@filler: @base * 2;
@color: #888 / 4;
@background-color: @color + #111;
@width: (@var + 5) * 2;//括号也同样允许使用
@height: 100% / 2 + @filler;
@var: 1px + 5;
div{
  height:@height;
  color:@color;
  background: @background-color;
  border: (@width * 2) solid black;//在复合属性中进行计算
}
```

编译结果：
```css
div {
  height: 60%;
  color: #222222;
  background: #333333;
  border: 44px solid black;
}
```

## 函数
Less 内置了多种函数用于转换颜色、处理字符串、算术运算等
### color
LESS 提供了一系列的颜色运算函数. 颜色会先被转化成 HSL 色彩空间, 然后在通道级别操作:
```css
lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
darken(@color, 10%);      // return a color which is 10% *darker* than @color
saturate(@color, 10%);    // return a color 10% *more* saturated than @color
desaturate(@color, 10%);  // return a color 10% *less* saturated than @color
fadein(@color, 10%);      // return a color 10% *less* transparent than @color
fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
fade(@color, 50%);        // return @color with 50% transparency
spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
spin(@color, -10);        // return a color with a 10 degree smaller hue than @color
mix(@color1, @color2);    // return a mix of @color1 and @color2
```

提取颜色信息的函数
```css
hue(@color);        // returns the `hue` channel of @color
saturation(@color); // returns the `saturation` channel of @color
lightness(@color);  // returns the 'lightness' channel of @color
```

### Math
```css
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`
percentage(0.5); // returns `50%`
```

## 命名空间
有时候，你可能为了更好组织 CSS 或者单纯是为了更好的封装，将一些变量或者混合模块打包起来，一些属性集之后可以重复使用。

```css
/*模块*/
#bundle {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white
    }
  }
  .tab { /**/ }
  .citation { /**/ }
}
/*下面复用上面的一部分代码*/
#header a {
  color: orange;
  #bundle > .button;
}
```

编译生成
```css
#bundle .button {
  display: block;
  border: 1px solid black;
  background-color: grey;
}
#bundle .button:hover {
  background-color: white;
}
#bundle .tab {
  /**/
}
#bundle .citation {
  /**/
}
```

LESS中的命名空间，属于高级语法，在日常项目中应用比较广泛。我们可以用LESS中的命名空间为自己封装一些日常比较常用的类名，以便以后做项目的时候更有效率。

## 作用域
子类里面的优先，找不到才往父类里找，类似于js中的作用域链。
```css
@var: red;
#page {
  @var: white;
  #header {
    color: @var; // 这里值是white
  }
}
```

也不会因为后面定义而影响作用域
```css
@var: red;
#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

## 导入
和css一样，你可以导入一个 .less 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 .less扩展名，则可以将扩展名省略掉：
```css
@import "library"; // library.less
@import "library.less";
```

如果你想导入一个CSS文件而且不想LESS对它进行处理，只需要使用.css后缀就可以:
```css
@import "lib.css";
```

这样的话less就会跳过这个文件不去处理它

## 函数参考
```css
color(string) 解析颜色,将代表颜色的字符串转换为颜色值
convert(value,unit) 将数字从一种单位转换到另一种单位.
第一个参数为带单位的数值,第二个参数为单位.
ceil(number)向上取整
floor(number)向下取整
percentage(number)将浮点数转换为百分比字符串
round(number)四舍五入取整
sqrt(number)计算一个数的平方根,并原样保持单位
pow(number,number)设第一个参数为A,第二个参数为B,返回A的B次方.
mod(number,number)返回第一个参数对第二参数取余的结果.
min(value1, ..., valueN)返回一系列值中最小的那个.
max(value1, ..., valueN)返回一系列值中最大的那个.
abs(number)计算数字的绝对值,并原样保持单位
sin(number)正弦函数
cos(number)余弦函数
asin(number)反正弦函数.返回以弧度为单位的角度,
区间在 -PI/2 到 PI/2之间.
acos(number)反余弦函数.区间在 0 到 PI之间.
tan(number)正切函数
atan(number)反正切函数
pi()返回圆周率 π (pi)
isnumber(value)如果待验证的值为数字则返回 true,否则返回 false
isstring(value)如果待验证的值是字符串则返回 true,否则返回 false
iscolor(value)如果待验证的值为颜色则返回 true,否则返回 false
isurl
ispixel
ispercentage
isem
```
