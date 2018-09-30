---
title: JavaScript字符串
date: 2017-10-23 20:40:40
tags: JavaScript
---
## 字符串的使用
JavaScript中字符串的内容都是不可变的。String对象的所有方法都是返回一个全新的对象，而不是修改原始字符串。
```
var s1 = new String("Hello");
var s2 = s1.toLowerCase(); //把字符串s1转换为小写形式
var s3 = s1.toUpperCase(); //把字符串s1转换为大写形式

console.log(s1); //Hello
console.log(s2); //hello
```
length属性返回字符串中字符的个数
```
var s3 = new String('abc字符 串');
console.log(s3);  // 7
```
### 字符串类型底层其实都是用字符字符数组实现的
比如：
```
var str = 'Hello World!'
console.log(str[1]); //e
```
## 获取指定位置的字符charAt()
charAt(index) 方法用于获取指定下标出的字符
```
var str = 'Hello World!'
console.log(str.charAt(0)); // H
console.log(str.charAt(11)); // !
```
charCodeAt(index) 方法用于获取指定小标出的字符的Unicode码 字符编码
```
var str = 'aA一龥';
console.log(str.charCodeAt(0)); // 97
console.log(str.charCodeAt(1)); // 65
console.log(str.charCodeAt(2)); // 19968
console.log(str.charCodeAt(3)); // 40869
console.log(String.fromCharCode(str.charCodeAt(3))); // 龥
```
注意：
频繁对字符串+= 要用字数组代替！
```
var result;
for(var i=0;i<input.length;i++){
	result += input[i]; //将i位置的字符转为一个数
}
console.log(result.join(" "));
```
- Step1：每个子字符串放入数组
- Step2：join("")拼接数组元素
```
var result = [];
for(var i=0;i<input.length;i++){
	result.push(input.charCodeAt(i)); //将i位置的字符转为一个数
}
console.log(result.join(" "));
//解码
var fromChar = [];
for(var i=0;i<result.length;i++){
	fromChar.push(String.fromCharCode(result[i]))	;
}
console.log("解码"+fromChar.join(""));
```

## 字符串三大操作：
### 1. 查找关键字indexOf()
var index=str.index("关键字");//返回位置没有找到返回-1
**indexOf 只找第一个关键词位置  默认从0开始找**
var index=str.indexOf("关键字",from);
	from：开始查找位置
```
var str = "朋友说要来，我急忙去车站。结果她又说车在车站已经停了！";
//var index = -1;
//index+1从上次找的位置之后开始
//while((index = str.indexOf("车站",index+1)) != -1){
var index = str.length;
while((index = str.lastIndexOf("车站",index-1)) != -1){ //从后往前找
	console.log("位置："+index+"发现关键字");
        if(index==0){//防止字符出现在第一个出现死循环
            break
        }
}
```
#### lastIndexOf(value,[fromIndex])	返回最后一次出现指定子串的下标

### 2. 截取子字符串slice()、substring()
str.slice(start, [end])    返回从start到end-1范围内的子串；若省略end，则直接取到字符串结尾
str.substring(start, [end])    返回从start到end-1范围内的子串；若省略end，则直接取到字符串结尾
slice可以用负数 而substring不可以
str.substr(start,count)    返回从start开始的count个字符;若省略count则取到结尾
```
var pid = "110102198006282151";
var birth = pid.substring(6,14); //19800628
var birth = pid.slice(-4-8,-5+1); //19800628
var birth = pid.substr(6,8); //19800628
```

### 3. 分隔字符串split()
split(separator,[count])使用指定分隔符对字符串进行拆分。
```
//将单词首字母换为大写，然后拼第二个字母之后的所有字符
var str = "you can you up";
var subarr = str.split(/\s+/);
console.log(subarr);
for(var i=0;i<subarr.length;i++){
  subarr[i] = subarr[i][0].toUpperCase() + subarr[i].substring(1);
}
str = subarr.join(" ")
console.log(str); //You Can You Up
```
### 4. 连接字符串concat()
concat(str1,str2,...)将多个字符串拼接起来str1+str2可以代替。

## 注意
可以使用unicode位数判断字符的类型。

| 数字编号| 字符|类型            |
| :-----------:|:-------:|:------------:|
| 48-57      | 0-9    | 数字        |
| 65-90      | A-Z    |大写字母  |
| 97-122    | a-z     |小写字母  |
| 19968-40869|汉字|              |
||其他为特殊字符| |

### 5. 模式匹配：
什么是模式匹配：可以设置查找或替换的规则！
何时使用模式匹配：要查找的关键字可能发生有规律的变化。
如何使会用模式匹配：
#### replace()
var newStr = str.replace(reg,b);
用规则reg替换字符b第一次出现的位置  /a/g全部替换
第二个参数可以是函数
- 先定义模式：/关键字/模式
```
var str = "You can you up";
var reg = /You/ig; //i不区分大消息g全局匹配
console.log(str.replace(reg,"**")); //** can ** up
```
#### match
按模式*替换*关键字的*内容*
var arr = str.match(reg);
```
var str = "You can you up";
var reg = /You/ig; //i不区分大消息g全局匹配
console.log(str.match(reg)); //打印出数组含有You和you
```
只能取得关键字的内容，无法确定每个关键字的位置！
**如果未找到返回`null`**  只要有可能返回null 都要先判断!=null,再处理

#### search()
var index=str.search(reg); 和indexOf完全相同！
但是indexOf不支持模式匹配

infdexOf不支持正则
search支持正则，但是每次只能找一个
match所有内容但是得不到位置！


### 查找：
仅判断有没有，或者仅查找位置：indexOf() 支持正则：search()
仅找所有关键字的内容：match()
既找位置又找内容：[exec()](http://www.syymo.cn/2017/09/23/JavaScript之正则表达式#JavaScript中regExp的exec-查找方法)

### trim()方法
var newStr = str.trim()        
返回的字符串去掉了左右空格
IE8不支持trim()
用正则表达式代替
function trim(str){
    var regExp = /(^\s+)|(\s+$)/g;
    str = str.replace(regExp,""); 
    return str;
}

## String对象与正则表达式
var str = str.replace(/正则表达式/ig,"替换值");
var arr = str.match(/正则表达式/ig);
var index = str.search(/正则表达式/);
var arr = str.split(/正则表达式/);