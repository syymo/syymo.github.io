---
title: JavaScript this关键字
date: 2017-11-08 21:04:49
tags: JavaScript
---
## this关键字
this是什么？
在还没接触到闭包等东西之前this就是这些意思
当前方法、函数的调用对象
通过事件调用函数的对象
在全局中this是window
```
console.log(this);  // object window
//window 相当于是js 的“老大”
window.alert(this); //object window
```
## 谁让函数执行this就指向谁
```
oBtn.onclick = function(){
    alert(this);  //按钮
}
```
## 函数嵌套函数中的this指向
```
function fn1(){
    alert(this);  //谁让函数动 this指的就是谁
}
var oBtn = document.getElementById("btn1");
oBtn.onclick = function(){
    //alert(this);  //按钮
    fn1();  //object window
}
```
## this运用
### this选取当前元素
此方法中的this直接指向了`aBtn[i]`从而改变了其背景颜色。
```
var aBtn = document.getElementsByTagName("input");
for(var i=0;i<aBtn.length;i++){
    aBtn[i].onclick = function(){
      this.style.background = "yellow";
}
```
而这种方法则是用`that`变量把`this`存起来，下面调用函数的时候直接用`that`，而此时`this`指向的是`window`对象
```
var aBtn = document.getElementsByTagName("input");
var that = null;
for(var i=0;i<aBtn.length;i++){
    aBtn[i].onclick = function(){
      this.style.background = "yellow";
      that = this;
      fn1();
    }
}

function fn1(){
    // this = > window
    that.style.background = "yellow";
}
```
这种发放则是直接在点击的时候执行函数，而此时`this`指向的是点击的按钮、因此三种方法均能达到效果。
```
var aBtn = document.getElementsByTagName("input");
for(var i=0;i<aBtn.length;i++){
    aBtn[i].onclick = fn1;
}

function fn1(){
    // this = > window
    this.style.background = "yellow";
}
```
### this选取当前元素的子元素
当this中存储的是某元素对象，则可以通过this获取元素的子元素。
```
    <style>
        li{
            width:100px;
            height:100px;
            float:left;
            margin-right:30px;
            margin-top:50px;
            background:#f1f1f1;
            position:relative;
            list-style:none;
        }
        div{
            width:80px;
            height:150px;
            background:red;
            position:absolute;
            top:75px;
            left:10px;
            display:none;
        }
    </style>
</head>
<body>
    <ul>
        <li><div></div></li>
        <li><div></div></li>
        <li><div></div></li>
        <li><div></div></li>
        <li><div></div></li>
    </ul>
</body>
<script>
    window.onload = function(){
        var aLi = document.getElementsByTagName("li");
        var that = null;
        for(var i=0;i<aLi.length;i++){
            aLi[i].onmouseover = function(){
                //this.getElementsByTagName('div')[0].style.display = "block";
                that = this;
                fn1();
            }
            aLi[i].onmouseout = function(){
                this.getElementsByTagName('div')[0].style.display = "none";
            }
        }
        function fn1(){
            that.getElementsByTagName('div')[0].style.display = "block";
        }
        
    }
</script>
```
而此时的this指向的是`li`，可用在循环内部直接用`this`找到它的子元素，或者用`that`把`this`保存起来，然后在找它的子元素。


