---
title: React基础
date: 2017-02-13 18:44:20
categories: React
---

什么是react?
> React 是一个用于构建用户界面的JavaScript库

# 安装与配置
## 安装bower
```bash
npm install bower -g
```

## 安装react babel
```bash
bower install react babel --save
```

## 配置.babelrc
```bash
{
  "directory":"./lib"
}
```

## 配置webStorm
默认是javaScript语法，要配置为React JSX语法
File–>settings–>Languages & Frameworks–>javascript–>React JSX

# ReactDOM.render
> ReactDOM.render 是 React 的最基本方法,用于将标签模板转为HTML语言，并插入指定的DOM节点

## index.html
```javascript
<script src="lib/react/react.js"></script><!--react核心库-->
<script src="lib/react/react-dom.js"></script><!--提供与DOM相关的功能,会在window下增加ReactDOM属性-->
<script src="lib/babel/browser.js"></script><!--将JSX语法转为JavaScript语法-->
<script type="text/babel" src="js/1.react.js"></script><!--因为React独有的JSX语法,跟JavaScript不兼容，需要转义-->
```

## js/1.react.js
ReactDOM.render 是 React 的最基本方法,用于将标签模板转为HTML语言，并插入指定的DOM节点

```javascript
//将一个h1标题，插入app元素内部
ReactDOM.render(
    <h1>徐娅</h1>,
    document.getElementById('app')
);
```
**原理**：

```javascript
//babel 的作用是将JSX语法转为以下的JavaScript语法
render(
    {type:'h1',props:{className:'red'},children:'hello'},
    document.querySelector('#app')
);
```

```javascript
//React元素不等于DOM元素，但是ReactDOM元素和DOM元素有一一对应的关系
function render(element,container) {
    var ele=document.createElement(element.type);//创建元素节点//
    for(var attr in element.props){
        ele[attr]=element.props[attr];
    }
    ele.innerHTML=element.children;
    container.appendChild(ele);
}
```

# JSX语法
> 是一种JS和HTML混合的语法,将组件的结构、数据甚至样式都聚合在一起定义组件,会编译成普通的Javascript。
> JSX=Javascript+xml(html)

- 遇到HTML标签(以 < 开头)，就用HTML规则解析
- 遇到代码块(以 { 开头)，就用JavaScript规则解析
- 使用样式时可以让style等于一个样式对象
- 使用样式类时只能使用className=类名,因为class是Javascript关键字

```javascript
var persons = ['张三', '李四', '王五'];
var style = {color:'red'};
ReactDOM.render(
  <div>
  {
    persons.map(function (person) {
      return <div style={style}>Hello, {person}!</div>
    })
  }
  </div>,
  document.getElementById('app')
);
```

# React组件
> 我们可以很直观的将一个复杂的页面分割成若干个独立组件,每个组件包含自己的逻辑和样式 再将这些独立组件组合完成一个复杂的页面。 这样既减少了逻辑复杂度，又实现了代码的重用

## 定义组件
React允许将代码封装成组件，然后像插入普通HTML标签一样，在网页中插入这个组件

- 组件类的第一个字母必须大写
- 组件类能且只能包含一个顶层标签
```javascript
var Message = React.createClass({
    render: function() {
        return <h1>Hello</h1>;
    }
});
ReactDOM.render(
    <Message/>,
    document.getElementById('app')
);
```

## 组件的属性this.porps
- 每个组件可以有自己的属性,一般用来存放组件初始后不变的数据,比如人的性别，姓名等
- 属性一般用作组件的数据源，一般由父组件传入,比如你的名字一般是由你父母取的
- 属性可以通过this.props中取出
- propTypes可以用来定义传入组件属性的名称和类型
- getDefaultProps函数可以用来定义组件的默认属性

```javascript
var Person = React.createClass({
    //类似于约定了一个接口文档,这是用于验证传递给组件的属性，
    propTypes: {
        name: React.PropTypes.string.isRequired,//定义name的属性类型为字符串，必须传入
        gender: React.PropTypes.string.isRequired,
        age:React.PropTypes.number.isRequired
    },
    getDefaultProps:function(){
        return {name:'无名氏'}
    },
    render: function() {
        //属性可以通过属性对象this.props中取出
        return (
            <h1>
                {this.props.name}
                {this.props.gender}
                {this.props.age}
            </h1>
        );
    }
});
var props = {
    gender:'男',
    age:18
}
ReactDOM.render(
    <Person {...props} />,//属性可以在使用组件时传入
    document.getElementById('app')
);
```

## this.props.children
this.props对象的属性与组件实例的属性一一对应,但this.props.children属性表示组件的所有子节点

```javascript
var Home=React.createClass({
    render(){
        //处理不是数组的情况
        var arr;
        if(Array.isArray(this.props.children)){
            arr=[this.props.children];//this.props.children是三个span组成的数组
        }else{
            arr=[];
        }
        return (
            <ul>
                {
                    arr.map(function (item,index) {
                        return <li key={index}>{item.props.children}</li>
                    })
                }
            </ul>
        )
    }
});
ReactDOM.render(
    <Home>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </Home>,
    document.querySelector("#app")
);
```

## 组件的状态
- 组件一开始有一个初始状态,然后用户互动,导致状态变化，从而触发界面重新渲染
- getInitialState用来定义初始状态
- 可以给按钮绑定事件，当事件发生的时候调用对应的方法改变组件的状态

```javascript
var Clock=React.createClass({
    //组件内，需要变化的用状态，不需要变化的用属性
    getInitialState(){//获取初始状态的值
        return {time:new Date()};
    },
    handleClick(){
        //修改状态对象并重新渲染视图（调用render方法）
        this.setState({time:new Date()});
        // this.state.time=new Date();//这样不会自动调用render方法更新视图
    },
    render(){
        return (
            <p>
                {this.state.time.toLocaleString()}{/*time是个对象需要转换为字符串*/}
                <button onClick={this.handleClick}>更新</button>
            </p>
        )
    }
});
ReactDOM.render(
    <Clock/>,
    document.querySelector("#app")
);
```

常用的事件：

- 剪贴板事件 onKeyDown onKeyPress onKeyUp
- 键盘事件 onFocus onBlur
- 焦点事件 onChange onInput onSubmit
- 鼠标事件 onClick onDoubleClick onMouseDown onMouseEnter onMouseLeave

## 表单元素双向数据绑定
```javascript
var Foucs=React.createClass({
    getInitialState(){//获取初始状态
        return {value:"初始值"};
    },
    handleChange(event){//处理改变事件
        this.setState({value:event.target.value});
    },
   render(){
        var style={width:"100px", height:"30px",border:"1px solid red"};
       return (
           <div>
               <input style={style} type="text" value={this.state.value} onChange={this.handleChange}/>
               <p>{value}</p>
           </div>
       )
   }
});
ReactDOM.render(
    <Foucs/>,
    document.querySelector("#app")
);
```

注意: 如果给表单元素设置了value属性，则必须指定onChange事件处理函数，否则 此字段会变成只读状态

# 复合组件
多个简单的组件嵌套，可构成一个复杂的复合组件，从而完成复杂的交互逻辑

```javascript
var Panel=React.createClass({render(){
        //console.log(this.props);//Object {head: "头部", body: "主体", footer: "底部", style: "success"}
        return (
            <div className={"panel panel-"+this.props.style}>
                <PanelHead content={this.props.head}></PanelHead>{/*<PanelHead content='头部'></PanelHead>*/}
                <PanelBody content={this.props.body}></PanelBody>
                <PanelFooter>{this.props.footer}</PanelFooter>{/*<PanelHead>底部</PanelHead>*/}
            </div>
        )
    }
});
var PanelHead=React.createClass({render(){
        //console.log(this.props);//Object {content: "头部"}
        return (
            <div className="panel-heading">{this.props.content}</div>
        )
    }
});
var PanelBody=React.createClass({
    render(){
        //console.log(this.props);//Object {content: "主体"}
        return (
            <div className="panel-heading">{this.props.content}</div>
        )
    }
});
var PanelFooter=React.createClass({ render(){
        //console.log(this.props);//Object {children: "底部"}
        return (
            <div className="panel-heading">{this.props.children}</div>
        )
    }
});
ReactDOM.render(
    <Panel head="头部" body="主体" footer="底部" style="success"/>,
    document.querySelector("#app")
);
```

# 组件的生命周期
React中可以指定在组件的生命周期的不同阶段执行的函数

## 渲染前
- getDefaultProps 在组件类创建的时候调用一次,则此处返回的对象中的相应属性将会合并到this.props
- getInitialState 在组件挂载之前调用一次。返回值将会作为this.state的初始值。
- componentWillMount 在首次渲染之前触发

## 渲染
- render 当调用的时候，会检测this.props和this.state，返回一个组件

## 渲染后
- componentDidMount 在初始化渲染执行之后立刻调用一次
- shouldComponentUpdate 在接收到新的props或者state，将要渲染之前调用,返回false则不更新组件
- componentWillUpdate 做一些更新之前的准备工作
- componentDidUpdate 更新之后触发
- componentWillReceiveProps 在组件接收到新的props的时候调用

## 移除
- componentWillUnmount 在组件从DOM中移除的时候立刻被调用
- componentDidUnmount 组件移除之后调用

# DOM操作
给组件加上ref=”xxx”后，可在父组件中通过this.refs.xxx获取该DOM元素

```javascript
var Focus=React.createClass({
    handleClick(){
        //console.log(this.refs);//{myTxt: input}
        //this.refs是一个对象，属性是引用名，也就是虚拟DOM元素ref属性的值，值是此参数名对应的DOM对象
        this.refs.myTxt.focus();
    },
    render(){
        return (
            <div>
                <input type="text" ref="myTxt" name="a"/>
                <button onClick={this.handleClick}>得到焦点</button>
            </div>
        )
    }
});
ReactDOM.render(
    <Focus/>,
    document.querySelector("#app")
);
```

# 通过AJAX获取数据
```javascript
/*
* 1、定义初始状态对象{words:[]}
* 2、给input框改变事件，当值改变的时候，先取出文本框的内容，然后调用百度JSON联想词接口，得到返回值，修改状态对象
* 3、用状态对象中的words渲染页面
* https://www.baidu.com/su?wd=b&cb=cc
* */
var Suggest=React.createClass({
    getInitialState(){
        return {words:[]}
    },
    handleChange(event){
        /*修改this的方法：
        * 1.箭头函数
        * 2.var that=this
        * 3.context:this
        * */
        //获取输入框的值
        var wd=event.target.value;
        console.log({wd})//Object {wd: "d"}
        $.ajax({
            url:'https://www.baidu.com/su?',
            type:"get",
            dataType:'jsonp',//返回数据转换为jsonp
            data:{wd},//{wd: wd}对象的扩张 es6的写法
            jsonp:'cb', //callback=fn  ===>   cb=fn
            context:this,//指定回调函数中的this指针
            success:function (result) {
                console.log(result);
                this.setState({words:result.s});
            }
        });
    },
    render(){
        return (
            <div className="panel panel-default">
                <div className="panel-heading">
                    <input type="text" className="form-group" onChange={this.handleChange}/>
                </div>
                <div className="panel-footer">
                    <ul className="list-group">
                        {
                            this.state.words.map(function (item,index) {
                                return <li key={index} className="list-group-item">{item}</li>;
                            })
                        }
                    </ul>
                </div>
            </div>
        )
    }
});
ReactDOM.render(
    <Suggest/>,
    document.querySelector("#app")
);
```

# 实战：留言板
```javascript
//定义MessageBox组件
var MessageBox=React.createClass({
    getInitialState(){//设置/获取初始状态的值
        return {message:this.props.message};
    },
    addMessage(msg){//添加留言
        this.state.message.push(msg);
        this.setState({message:this.state.message});
    },
    render(){//渲染页面
        return (
            <div className="container">
                <div className="row">
                    <div className={"panel panel-"+this.props.panelStyle}>
                        <PanelHead title={this.props.title}></PanelHead>
                        <PanelBody message={this.state.message}></PanelBody>
                        <PanelFooter addMessage={this.addMessage} msg={this.props.msg}></PanelFooter>
                    </div>
                </div>
            </div>
        )
    }
});
//定义PanelHead组件
var PanelHead=React.createClass({
    render(){
        return (
            <div className="panel-heading">
                <h1>{this.props.title}</h1>
            </div>
        )
    }
});
var PanelBody=React.createClass({
    render(){
        return (
            <div className="panel-footer">
                <ul className="list-group">
                    {
                        this.props.message.map(function (item,index) {
                            return <li className="list-group-item" key={index}>{item}</li>;
                        })
                    }
                </ul>
            </div>
        )
    }
});
//定义PanelFooter组件
var PanelFooter=React.createClass({
    handleKeyUp(event){
        event.preventDefault();
        var msg=this.refs.msgTxt.value;
        if(msg==""){
            alert('内容不能为空！');
            return;
        }
        this.props.addMessage(msg);
        this.refs.msgTxt.value="";
    },
    render(){
        var style={margin:"20px"};
        return (
            <form className="form" onSubmit={this.handleKeyUp}>
                <div className="form-group" style={style}>
                    <input type="text" className="form-control" ref="msgTxt" placeholder={this.props.msg}/>
                </div>
            </form>
        )
    }
});
var data={panelStyle:"success",title:'留言板',msg:'请输入留言内容,按回车进行发送',message:[]};
ReactDOM.render(
    <MessageBox {...data}/>,
    document.querySelector("#app")
);
```
