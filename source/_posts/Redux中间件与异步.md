---
title: Redux中间件与异步
date: 2017-12-21 14:16:20
tags: Redux
---
## 为什么要异步操作
之前我们的添加删除都是同步进行，但往往需要异步进行比如：登录、获取用户列表等等。
因此我们需要处理异步、调试工具、更优雅的和react结合。
- `Redux`处理异步，需要`redux-thunk`插件
- `npm install redux-devtools-extension` 并且开启
- 使用`react-redux`优雅的链接`react`和`redux`

### 处理异步
`Redux`默认只处理同步，异步需要`react-thunk`中间件
- `npm install redux-thunk --save`
- 使用`applyMiddleware`开启`thunk`中间件
- `action`可以返回函数，使用`dispatch`提交`action`

![image.png](http://upload-images.jianshu.io/upload_images/2088736-cdf7c2318abf3512.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

index.js
```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import App from './App';
import { counter,addP,removeP,addPAsync } from './redux';

const store = createStore(counter,applyMiddleware(thunk));
```
redux.js
```
// 延迟添加 延迟2秒
export function addPAsync(){
    // thunk插件的使用，这里可以返回函数
    return dispatch => {
        setTimeout( () => {
            // 异步结束后，手动执行dispatch
            dispatch(addP());
        },2000)
    }
}
```
## chrome安装Redux DevTools
- 新建`store`的时候判断`window.devToolsExtension`
- 使用`compose`结合`thunk`和`window.devToolsExtension`
- 调试窗口的`redux`选项卡，实时看到`state`

`reduxDevTools`应该这样配置
```
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';
import App from './App';
import { counter, addP, removeP, addPAsync } from './redux';

const reduxDevtools = window.devToolsExtension?window.devToolsExtension():f=>f;

const store = createStore(counter, compose(
    applyMiddleware(thunk), 
    reduxDevtools
));

```
此时的reduxDevTools应该会出现，当执行的时候会显示数据的变化。

![image.png](http://upload-images.jianshu.io/upload_images/2088736-53cbc82686ddeff7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

