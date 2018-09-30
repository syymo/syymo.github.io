---
title: React入门之JSX用法
date: 2017-09-24 14:07:36
tags: React
---
### 理解JSX 什么是JSX？
JSX=JavaScript Xml
它是基于ECMAscript的一种新特性
一种定义带属性树结构的语法
JSX不是XML，也不是HTML

### JSX的特点
- 类XML语法容易接受
- 增强JS语义
- 结构清晰
- 抽象程度高
- 代码模块化

### 如何使用JSX？

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2088736-4d2bad837a954f5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 首字符大写
- 嵌套 
- 求值表达式
- 驼峰命名
- htmlFor和className取代(html和classname)

### 如何注释？
标签里注释
//单行注释
/*    */多行注释

- 在标签内部的注释需要花括号
- 在标签外的的注释不能使用花括号

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2088736-b1e0909f8ac5e44d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 样式
React推荐使用内联样式，使用camelCase 语法来设置内联样式.React会在指定数字元素后面自动加上px

### 属性
由于JSX就是JavaScript，一些标识符不建议作为XML属性名，ReactDOM使用className代替class，htmlFor代替for。

### 独立文件
可以利用如下方式导入React JSX文件
`
<script type="text/babel" src="react_text.js"></script>
`
### JavaScript表达式
在JSX中使用JavaScript表达式需要放在花括号中{}


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2088736-46c073c610cef0b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### HTML标签 和 React组件
React可以渲染html标签，只需要在JSX里使用小写字母的标符号。
```
var myDivElement = <div className="foo" />;
ReactDOM.render(myDivElement, document.getElementById('example'));
````
渲染React组件，只需要在JSX里使用一个大写字母开度的本地变量
```
var MyComponent = React.createClass({/*...*/});
var myElement = <MyComponent someProperty={true} />;
ReactDOM.render(myElement, document.getElementById('example'));
```