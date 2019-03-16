---
title: 01_灵活的语言 JavaScript
date: 2018-10-03 22:24:04
tags: JavaScript设计模式
---
开始我们遇到一些需求的时候都是直接`function`开始局。
例如：
```
function CheckName () {
  // 验证名字
} 
function CheckEmail () {
  // 验证邮箱
}
function CheckPassword () {
  // 验证密码
}
......
```
## 函数的另一种创建方式
但是遇到团队开发时候问题就产生了，我们在创建**函数**的的时候创建了许许多多个全局变量。
```
var CheckName = function () {
  // 验证名字
} 
var CheckEmail = function () {
  // 验证邮箱
}
var CheckPassword = function  () {
  // 验证密码
}
......
```
这样写法和上面写法基本一样，只不过是这样写法在用的时候需要先声明而已。此时便创建了3个全局变量。
这样的写法功能能够实现但是在团队中就会影响他其他人，当别人也定义了此类方法时候就会出现覆盖。

## 用对象收编变量

此时我们就需要把这三个函数放在一个变量里，那么就到了**对象**上场的时候了，对象就是由属性和方法组成的。
```
var CheckObject = {
  checkName : function () {
    // 验证名字
  },
  checkEmail : function () {
    // 验证邮箱
  },
  checkPassword : function  () {
    // 验证密码
  },
  ......
}
```
现在我们已经将所有函数作为了CheckObject的方法，我们只创建了一个全局变量，当使用的它们的时候也很简单了`CheckObject.checkName()`。
当我们使用`对象.方法`使用的时候也可以`对象.方法`给对象创建方法。

## 对象的另一种形式
首先需要声明一个对象，下面的对象声明方式称之为**无参构造函数**创建对象。
```
var CheckObject = () {}
CheckObject.checkName = function () {
  // 验证名字
} 
CheckObject.checkEmail = function () {
  // 验证邮箱
}
CheckObject.checkPassword = function  () {
  // 验证密码
}
......
```
这样和上面使用方式是一样的，比如`CheckOb.checkName()`，现在这样虽然是可以满足我们需求，但是这样别人使用我们的方法是就比较麻烦了，这个对象不能复制一份，也就是说当别人`new CheckObject`时，这些方法并不会被复制。就比如你买了一本**《JavaScript设计模式》**这本书，你朋友也想要，但是书只有一本。但是如果我们买一个打印机的话不会出现这样的情况了。

## 真假对象
```
var CheckObject = {
  return {  
    checkName : function () {
      // 验证名字
    }, 
    checkEmail : function () {
      // 验证邮箱
    },
    checkPassword : function  () {
      // 验证密码
    },
    ......
  }
}
```
现在可以使用：
```
var a = CheckObject();
a.checkName();
```
实际上我们我们每次调用的时候都返回一个新的对象，每个人使用就不会出现冲突了。

## 类也可以
虽然我们创建的对象满足了我们的需求，但是这并不是真正意义上类的创建方式，返回出来的对象本身与CheckObject对象无关，我们需要稍微改造。
```
var CheckObject = {
  this.checkName = function () {
    // 验证名字
  }
  this.checkEmail = function () {
    // 验证邮箱
  }
  this.checkPassword = function  () {
    // 验证密码
  }
  ......
}
```
现在这样的对象就可以看做是类了，既然是类就可以使用`new`来创建了。
```
var a = new CheckObject();
a.checkName();
```
如果每个人都对类实例化（用类创建对象）我们每个人都会创建一套自己的方法了。

## 一个检测类
现在通过所有的方法放在函数内部了，通过this定义的，所以每一次通过new关键字创建新对象的时候，新创建的对象都会对类的this上的属性进行复制。所以这些方法都会有自己的一套方法，这样造成的消耗是很奢侈的，因此需要处理一下。
```
var CheckObject = function(){};
CheckObject.prototype.checkName = function() {
  // 验证名字
}
CheckObject.prototype.checkEmail = function() {
  // 验证邮箱
}
CheckObject.prototype.checkPassword = function() {
  // 验证密码
}
```

这种方式创建出来的对象所拥有的方法就是一个了，他们全都要依赖`prototype`原型依次寻找，而找到的方法都是一个，都绑定在`CheckObject`对象的原型上。现在需要将prototy写很多遍，因此可以这样写：

```
var CheckObject = function() {}
CheckObject.prototype = {
  checkName : function() {
    // 验证姓名
  },
  checkEmail: function() {
    // 验证邮箱
  },
  checkPassword: function() {
    // 验证密码
  }
}
```

上面两种方法不能混着使用，如果混着用，后面的方法就会覆盖前面`prototype`的方法。

使用方法：
```
var a = new CheckObject();
a.checkName();
a.checkEmail();
a.checkPassword();
```

## 方法还可以这样用

现在调用方法时，写了3遍，这种也是可以避免的，我们只需要在每一个方法末尾将当前对象返回，在JavaScript中this指向就是当前对象，所以可以直接返回。
```
var CheckObject = {
  checkName : function () {
    // 验证名字
    return this;
  },
  checkEmail : function () {
    // 验证邮箱
    return this;
  },
  checkPassword : function  () {
    // 验证密码
    return this;
  },
  ......
}
```
现在可以这样调用：

`CheckObject.checkName().checkEmail().checkPassword();`

可用同样的方式放到原型对象中。

```
var CheckObject = function() {}
CheckObject.prototype = {
  checkName : function() {
    // 验证姓名
    return this;
  },
  checkEmail: function() {
    // 验证邮箱
    return this;
  },
  checkPassword: function() {
    // 验证密码
    return this;
  }
}
```

当使用的时候需要先创建一下：
```
var a = new CheckObject();
a.checkName().checkEmail().checkPassword();
```

## 函数的祖先

在prototype.js中对源对象(JavaScript语言提供的对象类，如Function、Array、Object等)的拓展，每个函数都可以添加其他方法。

```
Function.prototype.checkEmail = function() {
  // 验证邮箱
}
```

这样使用的时候就比较方便了，如果习惯函数的形式，可以这样写：

```
var f = function() {};
f.checkName();
```

但是这种写法其实大多数情况时不被允许的，这样就污染了原生对象Function，别人创建的函数也会被你创建的函数污染，造成不必要的开销，但是可以抽象出一个类来统一添加方法的功能方法。

```
Function.prototype.addMethod = function(name, fn) {
  this[name] = fn;
}
```

这样需要添加方法就可以这样：

```
var methods = function() {};
// 或者
var methods = new Function();

methods.addMethod('checkName', function() {
  // 验证姓名
})
methods.checkName();
```

## 可以链式添加吗？

当然时可以的

```
Function.prototype.addMethod = function(name, fn) {
  this[name] = fn;
  return this;
}
```

现在添加方法就是这样了 

```
var methods = function() {};
methods.addMethod('checkName', function() {
  // 验证姓名
}).addMethod('checkEmail', function() {
  // 验证邮箱
})
```

既然可以链式添加那么需要链式使用的时候就需要将`this`返回。

```
var methods = function() {};
methods.addMethod('checkName', function() {
  // 验证姓名
}).addMethod('checkEmail', function() {
  // 验证邮箱
})

// 使用
methods.cbeckName().checkEmail();
```

## 换一种方式使用方法

之前我们使用的函数方式使用，对于习惯使用类方式使用的小伙伴，就可以这样简单改一下了：

```
Functhion.prototype.addMethod = function(name, fn) {
  this.prototype[name] = fn;
  return this;
}

var Methods = function() {};
Methods.addMethods('checkName', function() {
  // 验证名字
}).addMethod('checkEmail', function() {
  // 验证邮箱
})
```
现在使用的时候就需要先`new`来创建对象。

```
var m = new Methods();
m.checkEmail();
```

“JavaScript是一种灵活的语言，当然函数在其中扮演着一等公民。所以使用JavaScript，你可以编出更多优雅的艺术代码。”
