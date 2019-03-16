---
title: JavaScript常用语句
date: 2018-03-31 17:13:35
tags: JavaScript
---
## 数组方法
### 数组操作方法：
x.toString();     //任何对象都有toString方法将任何对象转为字符串 默认用,分隔  一般不主动调用，js在需要时自动调用

x.valueOf();      //方法：同toString

Array.isArray();  //检测某个值是不是数组 

Array.push();     //在数组末尾添加任意数量的项 元素下标不变 结尾入栈

Array.pop();      //移除数组最后一项           元素下标不变 结尾出栈

Array.shift();    //移除数组的第一项           所有元素下标改变 开头入栈

Array.unshift();  //在数组首端添加任意数量的项 所有元素下标改变 开头出栈

Array.reserve();  //颠倒数组项的顺序

Array.sort();	  //把数组"按照字符串"从小到大的顺序排列，配合比较函数

var newArr=Array.join("分隔符");//将数组转为字符串.可自定分隔符 将字符拼接为字符或者句子

var newArr=Array.concat(元素值,[数组],...);//将参数拆散成单个元素追加到数组中

var subArr=Array.slice(start,end+1);//截取数组下标从start开始，到end位置的元素,生成子元素组对象。*含头不含尾*

Array.splice:直接修改原数组 复制原数组中的部分元素

	删除元素：arr.splice(start,count);//从start开始删除,删除count个元素
	替换元素：arr.splice(start,count,值1,值2...);
	插入元素：arr.splice(start,0,值1，值2，...)；
	返回被删除元素组成的新数组

### 数组位置方法：(ECMAScript5)

Array.indexOf(search,fromindex); //search要查找的项(必须) fromindex查找起点(可选)

Array.lastIndexOf(search,fromindex) //用法同indexOf 都返回查找项的位置
    
    indexOf 从数组开头查找    lastIndexOf 从数组的末尾开始  没找到返回-1

### 数组迭代方法：(ECMAScript5)
Array.every()   // 对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true

Array.filter()  // 对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组

Array.forEach() // 对数组中的每一项运行给定函数。此方法无返回值，常用于便利

Array.map() // 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组

Array.some()    // 对数字中的每一项运行给定函数，如果该函数如任意一项放回true，则放回true

### 数组归并方法：(ECMAScript5)
Array.reduce()  

Array.reduceRight()

这两个方法都会迭代数组的所有项，然后构建一个返回值。其中`redue`从数组第一项开始，逐个遍历到最后一项。`reduceRight`从数组最后一项开始，逐个便利到第一项。

两个方法都接受两个参数：一个在每一项上调用的函数和(可选的)作为归并基础的初始值。传给reduce和reduceRight的函数接受4个参数：前一个项、当前项、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组的第二项。
```JavaScript
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
})
console.log(sum);   // 15
```


## 字符串
### 字符串操作：String对象的所有方法都是返回一个全新的对象，而不是修改原始字符串。
var num = str.length;      字符个数

### 字符方法
var char = str.charAt(index)        获取某个字符

var code = str.charCodeAt(index)    获取某个字符指定位置字符的unicode编号

var str = String.fromCharCode(code) 返回unicode编号对应的字符

### 字符串操作方法
var newStr = str.concat(strs)	   字符串str和strs拼接可用+代替

var subStr = str.substring(start,[end])返回从start开始到end-1范围内的子串;若省略end取到结尾

var subStr = str.split(start,[end])    返回从start到end-1范围内的子串;若省略end则取到结尾

var subStr = str.substr(start,count)   返回从start开始的count个字符;若省略count则取到结尾

### 字符串位置方法 不支持模式匹配
var index = str.indexOf(value,[fromIndex])返回第一次出现指定字符串的下标 未找到返回-1  

var index = str.lastIndexOf(value,[fromIndex])返回最后一次出现指定子串的下标 未找到返回-1

### trim方法
var newStr = str.trim()		   返回的字符串去掉了左右空格

### 字符串大小写转换方法
var newStr = str.toLowerCase() 转小写 toLocaleLowerCase()针对特定地区

var newStr = str.toUpperCase() 转大写 toLocaleUpperCase()针对特定地区

### 字符串模式匹配方法
var str= "cat, bat, sat, fat";

var pattern = /.at/;

var arr = str.match(pattren);   返回符合条件的数组和pattern.exec(text)相同

var index = str.search(a);      查找字符a在字符串第一次出现的位置 若无返回-1

var newStr = str.replace(reg,b);用规则reg替换字符b第一次出现的位置  /a/g全部替换

var subarr = str.split("分隔符",[count]); 用指定分隔符将一个字符串分隔成多个子字符串，并放入数组

	   	  

## 正则表达式：
var str= "cat, bat, sat, fat";

var pattern = /.at/;

var arr= regExp.exec("被查找的内容");返回数组，但是有两个额外的属性：index、input

				index表示匹配项在字符串的位置
				input表示应用正则表达式的字符串

var boolean = regExp.test("被检查的字符串"); 如果验证通过返回true；否则返回false
    


## Math对象：封装了所有数学计算的API
Math.PI 	  π的值

Math.round(num)   四舍五入取整

Math.ceil(num)    向上取整

Math.floor(num)   向下取整

Math.pow(底数,幂);

Math.sqrt(num);   平方根

Math.abs(num);	  绝对值

Math.random();    方法返回大于等于0小于1的一个随机数 

任意min-max：Math.floor(Math.random()*(max-min+1)+min)

## Date对象：保存的是从1970年1月1日 0:00:00到现在的毫秒数
var date = new Date();   获取当前时间

API:

    1.每个分量都有一对get/set方法，获取/设置该分量的值
    2.命名：年月日 没有s, 时分秒 有s
    3.取值/赋值：除了每月中的日之外，其余都是从0开始到-1结束
            每月中的日从1开始，到31结束
            月份：取值时要+1修正，赋值时-1修正
            星期：日 一 二 三 四 五 六
                0  1  2  3  4  5  6

### 日期计算：
1.两日期对象相减，得到毫秒

2.时间+-：var nowMs = date.getTime() 返回时间毫秒数

	var nextMs = mowMs +/- 5*60*1000;
	var next = new Date(nextMs);
使用毫秒做计算，不改变原时间对象！需要重新封装时间对象
最大仅能算到“天数”。再算“月数”无法确定没有天数

3.任意分量的计算

  先取出分量值，做计算，再set回去

  setXXX()：
    1.自动调整进制
	2.*直接修改原日期对象*

如何保留原日期对象呢？先new出新Date对象，再set

var newDate = new Date(oldDate.getTime());

使用毫秒做计算，不改变原时间对象！需要重新封装时间对象

最大仅能算到“天数”。再算“月数”无法确定没有天数

### 日期格式转换：
date.toString()	返回带有时区信息的日期和时间（0-23）

date.toLocaleString()	返回日期和时间的本地格式（AM、PM）

date.toDateString()	以特定于实现的格式显示星期几、月、日和年

date.toTimeString()	以特定于实现的格式显示时、分、秒和时区

date.toLocaleDateString()	以特定于地区的格式显示星期几、月、日和年

date.toLocaleTimeString()	以特定于的格式显示时、分、秒

date.toUTCString()	以特定于实现的格式完整的UTC日期

## Number对象：表示数值数据和数字常数，主要用于对数字进行制定格式的输出
toExponential()     把数字转为指数计数法表示的字符串，并具有指定的小数位数

toFiexed()          把数字转为定点表示法表示的字符串，并具有指定的小数位数

toPrecision()       把数字格式化为具有指定有效位数的字符串

toString()          把数字转为指定进制（默认十进制）表示的字符串

## Error对象：导致程序运行停止的运行时异常状态
Error类型：所有错误对象的父类型

EvalError	在使用eval()函数而发生异常

RangeError      参数超出范围 比如：num.toFixed(n) n<0

ReferenceError  引用错误：找不到对象只要使用未声明的变量时

SyntaxError     语法错误！修改源代码

TypeError       错误的使用了类型和类型的方法

URIError        URI格式不正确