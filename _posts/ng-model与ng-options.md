---
title: ng-model与ng-options
date: 2017-11-24 18:44:20
categories: AngularJS
---

二者都可用于实现下拉列表

# ng-repeat
```javascript
<select>
    <option value="x.id" ng-repeat="x in list" ng-bind="x.name"></option>
</select>
```

# ng-options
```javascript
<select ng-options="x.id as x.name for x in list" ng-model="listId"></select>
```

# 区别
- 如上所示，当在select中时ng-repeat需要写在option中，而ng-options不需要option，会自动生成。
ng-options 一定要和ng-model 搭配，ng-model获取的是列表的value值。
注意！！
- ng-options的value值的类型是number，当list.id是string类型时无法循环
- ng-repeat的value值类型是string，当list.id是number类型时无法循环
可以根据id类型不同选择，这是在最近的项目中发现的问题，通过上述方法解决，如果有不同见解，欢迎留言。

参考文档：[https://www.cnblogs.com/wolf-sun/p/4614532.html](https://www.cnblogs.com/wolf-sun/p/4614532.html)