---
title: JavaScript Function类型
date: 2017-11-01 16:22:22
tags: JavaScript
---
## 函数与function对象
**“函数是对象，函数名是指针”**
如果要访问函数的指针而不执行函数的话，必须去掉函数名后面的那对圆括号。
### 以声明方式定义方法
此方法会被放到源代码树的顶部提前解析。
```
function 方法名(参数列表){
   方法体;
     return 返回值;
}
function compare(a, b){
    return  a - b;
}
```
### 以创建对象方式定义方法
```
var 方法名 = new Function("参数1","参数2","方法体; return 返回值")
```
只有**声明方式**定义的方法会被**放到源代码树的顶部解析**
```
//var compare = new Function("a","b","return a-b;"); //此函数只能放在调用之前

var arr = [1,22,31,123,13,3];
arr.sort(compare);
console.log(arr);  //[1, 3, 13, 22, 31, 123]

function compare(a, b){
    return a - b;
}

```
使用不带圆括号的函数名是访问函数指针，而非调用函数
```
function sum(num1, num2){
    return sum1 + sum2;
}
var num = sum;  
```
此时num和sum都指向同一个函数，因此num()可以调用并返回结果。即使sum设置为null，让它与函数“断绝关系”，但是仍可以调用num()。

### 使用匿名函数赋值的方法定义方法
匿名函数：没有方法名的函数定义。
```
var compare = function(a,b){
    return a - b;
}
```
 等价于以声明方式定义方法方式定义函数。
匿名函数的2个用途：
- 回调函数：函数何时执行，不需要控制，由所在环境自动调用执行。例如：比较器！
`arr.sort(function(a, b){return a - b});`
- 自调函数：匿名函数自己调用自己
  当函数不需要重复使用时，使用匿名函数自调。
```
(function(参数...){
    方法体;
})(参数值...);
```
## ECMAScript中函数没有重载
重载：方法，根据传入的参数列表不同，执行不同的任务。
比如：
```
function pay(money){
    //现金结账：验钞，找零
}
function pay(cardId, pwd){
    //刷卡结账：验证卡号验证密码
}
//Java C++等会根据参数个数执行不同的函数，这种方式加 重载
pay(); //JavaScript中第二个函数会覆盖前面的函数
```
语法不支持重载，但是可以通过方法模拟实现。

## 函数内部属性
两个特殊对象：`arguments`和`this`。
`arguments`对象有一个`callee`的属性，该属性是一个指针。
### arguments对象
`arguments`对象：方法对象中保存所有参数的类数组对象。
类数组对象：长的像数组的对象。
可以用`aruguments`对象来模拟方法重载
```
function calc(){
    //如果传入的参数是1个则执行平方
    if(arguments.length == 1){
    var n = parseFloat(arguments[0]);
        console.log(n);
        alert(n*n);
    }else if(arguments.length == 2){  //如果传入的参数是1个则执行加法
        var m = parseFloat(arguments[0]);
        var n = parseFloat(arguments[1]);
        alert(m+n);
    }
}
```
`arguments`对象中的`callee`属性，该属性是一个指针。
定义阶乘函数一般都要使用递归算法，在函数有名字，而且名字以后不会变化的情况下，定义基本没问题。但问题是这个函数的执行和函数名紧紧耦合在一起，`arguments.callee`可以消除这种耦合的现象。
```
function factorial(num){
    if(num <= 1){
        return 1;
    } else {
        return num * factorial(num - 1);  //函数的执行和函数名factorial紧紧耦合在一起
        return num * arguments.callee(num - 1);  //消除耦合
    }
}
```
### this对象
this的引用是函数根据以执行的环境对象——或者也可以说是this值（当在网页的全局作用域中调用函数时，this对象引用的就是window）

### caller对象
这个属性保存着调用当前函数的函数引用，如果实在全局作用域中调用当前函数，它的值为null。
```
function outer(){
    inner();
}
function inner(){
    alert(inner.caller);
    // alert(arguments.callee.caller); //消除耦合
}
outer();
```
以上代码会导致警示框中显示outer()函数的源代码。因为`outer()`调用了`inner()`，所以`inner.caller`就指向了`outer()`。为了实现更松散的耦合可以通过`arguments.callee.caller`来访问。
不能为`caller`属性赋值，否则会导致错误。
## 判断函数传入参数是否为空
```
function isEmpty(str){
    if(str === undefined){
        return true;
    }else if(str == null){
        return true;
    }else{
        var reg = /^\s*$/;  //判断是否有空格
        var lreg = /^\s*/;  //判断左边是否有空格
        var rreg = /\s*$/;  //判断右边是否有空格
        return reg.test(str);
    }
}
while(true){
    var input = prompt("输入");
    if(!isEmpty(input)){
        console.log(input);
        break;
    }else{
        alert("输入不能为空");
    }
}
```
## 作为值的函数
ECMAScript中的函数名本身就是变量，所以函数可以最为值来使用。也就是说函数不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以将一个函数作为另一个函数的结果返回。
```
function callSomeFunction(someFunction, someArgument){
    return someFunction(someArgument);
}
```
此函数第一个参数为函数，第二个参数应该是要传递给该函数的一个值。

## 函数的属性和方法
每个函数都有`length`和`prototype`。
`length`属性表示函数希望接受的命名参数的个数。
`prototype`是保存所有实例的真正所在。`toString()`和`valueOf()`等方法实际上都保存在`prototype`名下，只不过是通过各自对象的实例访问。
函数都有两个非继承来的方法：`apply()`和`call()`
### apply()
`apply()`方法接收两个参数：一个参数是在其运行函数的作用域，另一个参数是参数数组。其中第二个参数可以是`Array`的实例，也可以是`arguments`对象。
```
function sum(num1, num2){
    return num1 + num2
}
function callSum(num1, num2){
    return sum.apply(this, arguments);  //传入arguments对象
    // return sum.apply(this, [num1,num2]);  //传入数组
}
```

### call()
`call()`方法与`apply()`方法的作用相同，它们区别在于接收参数的方式不同。对于`call()`而言，第一个参数this值没有变化，变化的是其余参数都直接传递给函数，换句话来说，使用`call()`方法时，传递给函数的参数必须逐个列举出来。
```
function sum(num1, num2){
    return num1 + num2
}
function callSum(num1, num2){
    return sum.call(this, num1, num2);
}
```

**apply()和call()最大的用处是能够扩充函数依赖运行的作用域，对象不需要与方法有任何耦合关系。**
```
window.color = "blue";
var o = {color: "red"};
function sayColor(){
    alert(this.color);
}
sayColor();  //blue

sayColor().call(this);  //blue
sayColor().call(window);  //blue
sayColor().call(0);  //red
```
### bind()
`bind()`方法会创建一个函数的实例，其this值会被绑定到传给`bind()`函数的值。

## 闭包
闭包是指有权访问另一个函数作用域中的变量的函数。也就是该函数外使用了不属于自己的局部变量。
何时使用闭包？保护局部变量
闭包应用特征：
> 1.局部变量：
函数中定义了有共享意义（如：缓存、计算器等）的局部变量（注意：全局变量会对外造成
污染）
 2.函数内嵌
在函数(f)中声明有内嵌函数(g)，内嵌函数(g)对函数(f)中的局部变量进行访问
3.外部使用
函数(f)向外返回此内嵌函数(g)，外部可以通过此内嵌函数持有并访问声明在函数(f)的局部
变量，而此变量在外部是通过其他途径无法访问的

如下例子：
```
function createComparisonFunction(propertyName){
    
    return function(object1, object2){
        var value1 = object1[propertyName];  //内部函数访问了外部函数的propertyName
        var value2 = object2[propertyName];  //内部函数访问了外部函数的propertyName

        if(value1 < value2){
            return -1;
        } else if (value1 > value2){
            return 1;
        }else{
            return 0;
        }
    }

}
```
或者这样
```
function number(){
    var n = 0; //局部变量
    function num(){
        return n++;  //0
    }
    return num;
}
        
var c = number(); //c中保存的是可以执行的方法对象
console.log(c);  //ƒ getUnique(){return n++;  //0   }
console.log(c());  //0
console.log(c());  //1
console.log(c());  //2
n = 0; //自动声明为全局
console.log(n);  //0
c=null; //释放闭包占用的资源！
```