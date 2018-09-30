---
title: JavaScript Error对象
date: 2017-10-31 19:42:05
tags: JavaScript
---
## Error错误：
什么是错误：导致程序运行停止的运行时异常状态
什么是错误处理：在出现异常状态时，保证程序不停止的机制

## 错误类型：
Error类型：所有错误对象的父类型
6种子类型：EvalError，RangeError，ReferenceError，SyntaxError，TypeError，URIError
### EvalError
EvalError类型的错误会在使用eval()函数发生异常时抛出。
简单的说：**如果没有吧eval()当函数调用，就会抛出错误。**
```
new eval();  //抛出EvalError
eval = foo;  //抛出EvalError
```

### RangeError
RangeError类型错误会在参数超出范围触发。
```
num.toFixed(-5);  //抛出RangeError
var arr = new Array(-10);  //抛出RangeError
```

### ReferenceError
ReferenceError类型错误会在访问不存在的变量时触发。
比如：只要使用未声明的变量时，都抛出ReferenceError
```
var obj = x;  //在x并未声明的情况抛出ReferenceError
       
```

### SyntaxError
SyntaxError类型错误还在语法错误时触发，需要修改源代码。
```
eval("a+b");  //抛出SyntaxError
```
### TypeError
TypeError类型错误会在错误的使用了类型和类型的方法！
```
var o = new 10;  //抛出TypeError
alert("name" in true);  //抛出TypeError
Function.prototype.toString,call("name");  //抛出TypeError
```
### URIError
URIError错误类型会在URI错误时触发。
这种错误类型很少见。

## 错误处理try-catch-[finally]

```
try{
    可能出错的代码
}catch(error){  //只要抛出错误都会创建一个Error对象
    错误处理的代码
    1.获得错误的信息：err.name类型err.message
    2.根据错误的类型，执行不同的处理
}[finally{ 可有可无，程序中使用了大对象！一定要在finally中主动释放
    无论是否出错都必须执行的代码
}]
```
当然也可以这样使用
```
try{
    someFunction()
}catch(error){  
    if(error instanceof TypeError){
      //处理错误类型
    }else if(error instanceof ReferenceError){
      //处理引用错误
    }else{
      //处理其他类型错误
    }
}
```

**能用if...else解决的问题，就不用try catch！**
**何时使用：try catch处理的是无法预料的问题**


主动抛出异常：告知方法调用者出错
throw new Error("自定义的错误信息")