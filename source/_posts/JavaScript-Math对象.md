---
title: JavaScript Math对象
date: 2017-10-29 17:29:37
tags: JavaScript
---
## Math对象：封装了所有数学计算的API
## 对象的属性
|   属性         |      说明                          |
|----------|---------------------------|
|Math.E  |  自然数对数的底数，即常量e的值   |
|Math.LN10  | 10的自然对数                         |
|Math.LN2|   2的自然数对数                         |
|Math.LOG2E| 以2为底e的对数                    |
|Math.LOG10E| 以10为底e的对数                    |
|Math.PI |π的值|
|Math.SQRT1_2| 1/2的平方根(2的平方根的倒数)    |
|Math.SQRT2| 2的平方根   |

## min()和max()方法
Math.min();  返回一组数的最小值
Math.min();  返回一组数的最大值
```
var arr = [2,4,1,7,6,3,8,9];
var max = Math.max.apply(Math,arr); 
console.log(max);
```

## 舍入方法
|   方法         |      说明             |
|----------|---------------------------|
|Math.ceil(num)   |向上取整    |
|Math.floor(num)   |向下取整    |
|Math.round(num)   |四舍五入取整    |

## random()方法
Math.random();     方法返回大于等于0小于1的一个随机数 。
任意min-max：Math.floor(Math.random()*(max-min+1)+min)
```
//随机生成4位验证码 
//strp1:将所有数字，字母装入一个数组备用 
var codes = []; 
//数字：48-57 
for(var i = 48; i <=5 7; codes.push(i), i++); 
//大写字母：65-90 
for(var i=6 5; i <=9 0; codes.push(i), i++); 
//小写字母：65-90 
for(var i=9 7; i <=1 22; codes.push(i), i++); 
//从0-61之间取4位随机 
function getCode(){ 
    var arr=[ ]; 
    for(var i=0 ; i < 4; i++){ 
        var index=Math.floor(Math.random()*(61-0+1)+0); 
        var char=String.fromCharCode(codes[index]); 
        arr.push(char); 
    } 
    return arr.join( ""); 
} 
function trim(str){ 
    var reg=/(^\s+)|(\s+$)/g; 
    return str.replace(reg, ""); 
} 
while(true){ 
    var code=g etCode(); 
    var input=p rompt( "输入验证码："+code); 
    var reg=/ ^[a-zA-Z0-9]{4}$/; 
    input=t rim(input); 
    if(reg.test(input)){ 
        if(input.toLowerCase()==c ode.toLowerCase()){ 
            alert( "验证成功！"); 
            break; 
        }else{ 
            alert( "验证码错误！"); 
        } 
    }else{ 
        alert( "验证码格式错误！"); 
    } 
}
```

## 其他方法
|   方法         |      说明             |
|----------|---------------------------|
|Math.abs(num)   |返回num的绝对值    |
|Math.exp(num)   |返回Math.E的num次幂    |
|Math.log(num)   |返回num的自然对数    |
|Math.pow(num,power)   |返回num的power次幂    |
|Math.sqrt(num)  |返回num的平方根    |
|Math.acos(x)    |返回x的反余弦值    |
|Math.asin(x)    |返回x的反正弦值    |
|Math.atan(x)    |返回x的反正切值    |
|Math.atan2(y,x) |返回y/x的反正切值    |
|Math.cos(x)     |返回x的余弦值    |
|Math.sin(x)     |返回x的正余弦值    |
|Math.tan(x)     |返回x的正切值    |
