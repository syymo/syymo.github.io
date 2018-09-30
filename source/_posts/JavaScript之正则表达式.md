---
title: JavaScript之正则表达式
date: 2017-09-23 18:03:49
tags: JavaScript
---
## 什么是正则表达式？
正则：规则、模式
字符串中字符出现的规律
正则表达式是强大的字符串匹配工具
也是一种难以读懂的文字

## 正则能做什么？
正则只能操作字符串

## 何时使用字正则表达式？
验证字符串格式，查找关键字，替换关键字

## 使用正则
RegExp对象：专门封装一条正则表达式，提供使用正则表达式的常用API
    先创建然后调用API
API：var boolean = regExp.test("被检查的字符串");
    如果验证通过返回true；否则返回false
var re = new RegExp("\\d{6}",ig);  //动态创建正则表达式
** 强调：所有的\都要改成\\\ **
var re = /正则表达式/;
例如：
```
var str = "hello world";
//js风格
var re = new RegExp('H');//参数区分大小写
alert(re.test(str));//检验str中是否有H  false
// var re = new RegExp('H','i');//加上i忽略大小写 true
 /*
perl风格
正则简写
var re = /H/;
var re = /H/i;
*/
```
## 正则表达式字母符号
| 字母符号     | 代表意思                |
|:------------ |:---------------------- |
| /g           | global全局匹配          |
| /i           | ignore 忽略大小写      |
|  &#124;      | “或”的意思             |
| []           | 定义匹配的字符范围      |
| {}           | 一般用来表示匹配的长度  |
| ()           | 分组                   |
| [^]          | “非”                   |
| ^            | 行首                   |
| $            | 行尾                   |
| [abc]        | a或者b或者c            |
| [^a]         | 除了a以外              |
| [^0-9a-z]    | 除了数字、字母以外     |
|[\u4e00-\u9fa5]| 所有汉字              |

### 预定义字符集：使用简化的字符，定义常用字符集

| 转义字符     | 代表意思                             |示例                |
| :----------- |:-----------------------------|:-------------------|
|  \d          |  digital 表示数字 0-9        |/\d/等价于/[0-9]/   |
|  \D          |  除了0-9，也就是非数字       |/\D/等价于/[^0-9]/  |
|  \w          |  匹配数字、字母、下划线      |/\w/等价于/[0-9a-zA-Z]/ |
|  \W          |  匹配除了数字、字母、下划线  |/\W/等价于/[^0-9a-zA-Z]/|
|  \s          |  匹配空白字符                |  /\s/等价于/[\n\r\t\v\f]/  |
|  \S          |  除了空白以外的东西            |  /\S/等价于/[^\n\r\t\v\f]/  |
|  .       |  匹配除了回车和换行外的任何单个字符 |/./等价于/[^\n\r\t\v\f]/  |
### 如果规则正文中出现特殊符号，用\转为原文

## 量词
什么是量词？出现的次数

| 量词符号    | 代表意思                        |示例        |
|:------------|:--------------------------------|:-----------|
| {min,max}   | 最少min次，最多max次            |/n{2,5}/    |
| {min, }     | 最少min次，最多不限             |/n{5,}/     |
| { ,max}     | 最少不限，最多max次             |/n{,5}/     |
| {x}         | 刚好x次                         |/n{x}/      |
| ?           | {0,1}0次或1次 可有可无          |/n?/        |
| *           | {0, }可有可无，不限制次数       |/n*/        |
| +           | {1, }量词(许多)至少一个         |/n+/        |

## 常用正则的例子：
校验邮箱：  **/^\w+@[a-z0-9]+\.[a-z]{2,4}$/** 或 **\w+@\w+([-]\w+)*(\\.\w+)+**
去除空格：  **/^\s+|\s+$/g**
匹配中文：  **[\u4e00-\u9fa5]**
中文姓名：  **[\u4e00-\u9fa5]{2-6}**
单词边界：  **\b**
检验手机：  **(\\+86)?\s*1[345789]\d{9}** 
检验身份证：**\d{15}(\d{2}[0-9xX])?**

## 指定匹配位置
| 重复字符    | 含义                          |示例        |
|:------------|:------------------------------|:-----------|
| ^           | 匹配字符串的开头              | /^a/       |
| $           | 匹配字符串的结尾              | /a$/       |
| \b          | 匹配单词的边界                | /\bhis\b/  |
| \B          | 匹配单词的非边界              | /\Bhis\B/  |
| ?=X         | 匹配其后紧接x的字符串         | /do(?=not)/|
| ?!X         | 匹配其后没有紧接x的字符串     | /do(?not)/ |
- ** ?=:预判，前一个字符之后，必须紧跟xx **
- ** ?!:预判，前一个字符之后，必须不能跟xx **

## JavaScript中regExp的test方法
test方法默认：只要找到就返回true
var boolean = regExp.test("被检查的字符串"); 如果验证通过返回true；否则返回false
```
//var regExp = /\w+@\w+([-]\w+)*(\.\w+)+/;
var regExp = /^\w+@\w+([-]\w+)*(\.\w+)+$/;
while(true){
  var input = prompt("输入邮箱：");
    if(regExp.test(input)){
      document.write("注册成功！")
      break;
    }else{
      alert("邮箱格式有误！");
    }
}
```
**正则表达式前加^，后加$**解决test默认匹配

## JavaScript中regExp的exec()查找方法
var arr= regExp.exec("被查找的内容");
返回数组，但是有两个额外的属性：index、input;
在没有找到或者找完后返回`null`;
**arr[0]:找到的关键字内容**
**arr.index表示匹配项在字符串的位置**
**arr.input表示应用正则表达式的字符串**
**每次只能找一个，但是每次自动修改regExp的lastIndex属性！**
**regExp对象的lastIndex属性：表示下次开始匹配的位置！**
**regExp设置与不设置全局标志(g),每次也只会返回一个匹配项**
```
var str = "cat, bat, sat, fat";
var regExp = /.at/;
var arr = regExp.exec(str);
console.log(arr);
console.log(regExp.lastIndex);

var arr1 = regExp.exec(str);
console.log(arr1);
console.log(regExp.lastIndex);
//增加全局变量并且需要多次调用，可放在循环中实现
var regExp = /.at/g;
var arr2 = regExp.exec(str);
console.log(arr2);
console.log(regExp.lastIndex);

var arr3 = regExp.exec(str);
console.log(arr3);
console.log(regExp.lastIndex);
```
![输出结果](http://upload-images.jianshu.io/upload_images/2088736-3d8048d881ad00f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
使用循环：
```
var str = "有时重要的东西总是迟来一步,但却一定会到,生活和爱情都是如此,程序当然也不例外."
var regExp = /生活|程序/g;
var arr = [];
while((arr=regExp.exec(str))!=null){
    //arr[0]:关键字的内容
    //arr.index:关键字的位置
    document.write("在位置"+arr.index+"发现："+arr[0]+"<br>");
}
```
## JavaScript中regExp的compile()方法
编译/重新编译正则表达式，将pattern转为内部格式，加快执行速度
一般程序引擎自动调用

## 贪婪模式和懒惰模式
贪婪模式：`.+`  `.*`  默认先匹配整个字符串，再缩小范围
兰多模式：`(.+?)` `(.*?)` 从第一个字符开始，向后扩展范围

## RegExp.$n
```
var arr = []
var str = "<a class='sy' id='syy' href=\"www.syymo.cn\" target='_blank'></a><a class='sy' ' href=\"syymo.cn\" target='_blank'></a>"
var regExp = /<a(.+?)href\s*=\s*['"](.*?)['"](.*?)>/ig;
while((arr=regExp.exec(str))!=null){
    console.log(arr[0]);
    console.log(RegExp.$2)
}
```
从正则表达式匹配的内容中，取一部分：RegExp.$n
n:正则表达式中的第n个分组，其实就是第n个圆括号
强调：分组从1开始
必须通过RegExp类型，直接调用$n，不能使用对象

