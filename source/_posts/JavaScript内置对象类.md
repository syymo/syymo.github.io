---
title: JavaScript内置对象类
date: 2017-09-29 19:03:09
tags: JavaScript
---
## 内置对象
什么是内置对象？
- 内置对象就是ECMAScript标准中已经定义好的，由浏览器厂商已经实现的标准对象！
- 内置对象中封装了专门的数据和操作数据常用的API
- JavaScript中内置对象列表：
  - String、Boolean、Nmuber、Array、Date、Math、Error、Function、Object、Global

## 包装类型
什么是包装类型？
- 专门封装原始类型的数据，并提供数据常用操作的内置类型。
```
var str = "Hello";//原始类型数据，存栈
// strObj = new String(str);//包装类型
str += "world!";
// strObj = null; //帮助拼接完成后js自动释放

var num = 5.5;
//var numObj = new Number(num);
num = num.toFixed(2);
//numObj = null;
```
为什么要有包装类型？
- 让原始类型的数据也可以像引用类型一样，拥有方法和属性。
- JavaScript中的包装类型：
  - String类型、Number类型、Boolean类型

何时使用包装类型？
- 只要有原始类型的数据调用方法或访问属性，js引擎都会自动创建对应的包装类型对象。

#### 包装类型是内置对象的一个子集。
方法调用完，包装类型对象自动释放。


## 原始类型
ECMAScript 有 5 种原始类型（primitive type），即
- #### Undefined、Null、Boolean、Number 和 String。

创建<font color="red">原始类型</font>字符串变量：
```
//直接创建
var stuN = 'Syymo';
var sex = '男';

var priceString = String(150.5); //可能放置栈中
```
创建<font color="red">引用类型</font>字符串对象：
```
var carType = new String('BMW528Li'); //可能放置堆中
```