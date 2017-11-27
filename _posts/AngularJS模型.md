---
title: AngularJS模型
date: 2017-11-27 18:44:20
categories: AngularJS
---

> ng-model 指令用于绑定应用程序数据到 HTML 控制器(input, select, textarea)的值。

```html
<!--双向数据绑定-->
<div ng-init="name='xy'">{{name}}</div>
<input type="text" ng-model="name">
<form name="myForm" ng-init="myText = 'test@runoob.com'">
    Email:
    <input type="email" name="myAddress" ng-model="myText" required></p>
    <!--验证用户书输入-->
    <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>
    <!--应用状态-->
    <h1>状态</h1>
    {{myForm.myAddress.$valid}}<!--表单是否通过验证-->
    {{myForm.myAddress.$dirty}}<!--表单是否修改-->
    {{myForm.myAddress.$touched}}<!--用户是否和控件进行过交互-->
</form>
```

```css
/*css类*/
input.ng-valid {/*验证通过*/
    background-color: lightblue;
}
input.ng-invalid {/*验证不通过*/
    background-color: red;
}
input.ng-empty {/*内容为空*/
    background-color: blue;
}
```

ng-model 指令根据表单域的状态添加/移除以下类：
- ng-empty 内容为空  与之相对的是ng-not-empty
- ng-touched 控件已失去焦点 与之相对的是ng-untouched
- ng-valid 通过验证 与之相对的是ng-invalid
- ng-dirty 是否修改了表单 修改过为true，没有修改过为false
- ng-pristine 是否修改了表单 没有修改过为true，修改过为flase  pristine：意思是原始的
- ng-pending 满足 $asyncValidators 的情况