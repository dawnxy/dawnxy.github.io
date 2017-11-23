---
title: Babel入门教程
date: 2017-02-03 18:44:20
categories: Babel
---

什么是Babel?

> Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。

这意味着，你可以现在就用ES6编写程序，而不用担心现有环境是否支持。下面是一个例子。

```javascript
// 转码前
input.map(item => item + 1);
    
// 转码后
input.map(function (item) {
  return item + 1;
});
```

上面的原始代码用了箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。

# 配置文件
Babel的配置文件是.babelrc，存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。
该文件用来设置转码规则和插件，基本格式如下。

presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。

```bash
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015
    
# react转码规则
$ npm install --save-dev babel-preset-react
    
# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

然后，将这些规则加入.babelrc。

```bash
{
    "presets": [
    "es2015",
    "react",
    "stage-2"
],
    "plugins": []
}
```
注意，以下所有Babel工具和模块的使用，都必须先写好.babelrc。

# 命令行转码babel-cli
Babel提供babel-cli工具，用于命令行转码。

它的安装命令如下。

```bash
$ npm install --global babel-cli
```

基本用法如下：

```bash
# 转码结果输出到标准输出
$ babel example.js
    
# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ babel example.js --out-file compiled.js
# 或者
$ babel example.js -o compiled.js
    
# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ babel src --out-dir lib
# 或者
$ babel src -d lib
    
# -s 参数生成source map文件
$ babel src -d lib -s
```
上面代码是在全局环境下，进行Babel转码。这意味着，如果项目要运行，全局环境必须有Babel，也就是说项目产生了对环境的依赖。另一方面，这样做也无法支持不同项目使用不同版本的Babel。

一个解决办法是将babel-cli安装在项目之中。

然后，改写package.json。

```bash
{
  // ...
  "devDependencies": {
    "babel-cli": "^6.0.0"
  },
  "scripts": {
    "build": "babel src -d lib"
  },
}
```

转码的时候，就执行下面的命令。

```bash
$ npm run build
```

# babel-node
babel-cli工具自带一个babel-node命令，提供一个支持ES6的REPL环境。它支持Node的REPL环境的所有功能，而且可以直接运行ES6代码。

它不用单独安装，而是随babel-cli一起安装。然后，执行babel-node就进入PEPL环境。

```bash
$ babel-node
> (x => x * 2)(1)
2
```

babel-node命令可以直接运行ES6脚本。将上面的代码放入脚本文件es6.js，然后直接运行。

```bash
$ babel-node es6.js
2
```

babel-node也可以安装在项目中。

```bash
$ npm install --save-dev babel-cli
```

然后，改写package.json。

```bash
{
  "scripts": {
    "script-name": "babel-node script.js"
  }
}
```

上面代码中，使用babel-node替代node，这样script.js本身就不用做任何转码处理。

# 浏览器环境
Babel也可以用于浏览器环境。但是，从Babel 6.0开始，不再直接提供浏览器版本，而是要用构建工具构建出来。如果你没有或不想使用构建工具，可以通过安装5.x版本的babel-core模块获取。

```bash
$ npm install babel-core@5
```

运行上面的命令以后，就可以在当前目录的node_modules/babel-core/子目录里面，找到babel的浏览器版本browser.js（未精简）和browser.min.js（已精简）。

然后，将下面的代码插入网页。

```javascript
<script src="node_modules/babel-core/browser.js"></script>
<script type="text/babel">
// Your ES6 code
</script>
```

上面代码中，browser.js是Babel提供的转换器脚本，可以在浏览器运行。用户的ES6脚本放在script标签之中，但是要注明type=”text/babel”。

另一种方法是使用babel-standalone模块提供的浏览器版本，将其插入网页。

```javascript
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.4.4/babel.min.js"></script>
<script type="text/babel">
// Your ES6 code
</script>
```

注意，网页中实时将ES6代码转为ES5，对性能会有影响。生产环境需要加载已经转码完成的脚本。

下面是如何将代码打包成浏览器可以使用的脚本，以Babel配合Browserify为例。首先，安装babelify模块。

```bash
$ npm install --save-dev babelify babel-preset-es2015
```

然后，再用命令行转换ES6脚本。

```bash
$  browserify script.js -o bundle.js \
  -t [ babelify --presets [ es2015 react ] ]
```

上面代码将ES6脚本script.js，转为bundle.js，浏览器直接加载后者就可以了。

在package.json设置下面的代码，就不用每次命令行都输入参数了。

```bash
{
“browserify”: {
“transform”: [[“babelify”, { “presets”: [“es2015”] }]]
}
}
```

# 在线转换
Babel提供一个REPL在线编译器，可以在线将ES6代码转为ES5代码。转换后的代码，可以直接作为ES5代码插入网页运行。`