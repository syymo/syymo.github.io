---
title: Redux基本用法
date: 2017-12-19 19:36:49
tags: Redux
---

![](https://camo.githubusercontent.com/f28b5bc7822f1b7bb28a96d8d09e7d79169248fc/687474703a2f2f692e696d6775722e636f6d2f4a65567164514d2e706e67)
## Redux 是什么
> Redux is a predictable state container for JavaScript apps.
(Not to be confused with a WordPress framework –
> [Redux Framework](https://reduxframework.com/).)
>
> It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as [live code editing combined with a time traveling debugger](https://github.com/gaearon/redux-devtools).
> 
> You can use Redux together with[React](https://facebook.github.io/react/), or with any other view library.

**意思为**：
    Redux是JavaScript应用程序可预测状态的容器。（不要与WordPress框架–Redux框架。混淆）
它帮助您编写行为一致的应用程序，在不同的环境中运行（客户端、服务器和本地），并且易于测试。最重要的是，它提供了很棒的开发人员体验，例如实时代码编辑和时间运行调试器相结合。
你可以使用`React`和`Redux`一起使用，或任何其他视图的图书馆。

## 基本概念和API
### Store
Store 就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 Store。
Redux 提供`createStore`这个函数，用来生成 Store。
```
import { createStore } from 'redux';
const store = createStore(fn);  //  接收一个函数作为参数生成Store对象
```
### State
Store对象包含所有数据。如果想得到某个时点的数据，急需要Store生成快照。这种时点的数据集合，就叫做State。
state可以通过`store.getState()`拿到。
```
import { createStore } from 'redux';
const store = createStore(fn);  //  接收一个函数作为参数生成Store对象

const state = store.getState();
```
### Action
Action是一个对象。其中的type属性是必须的，表示Action的名称，其他属性可以自己设置。
```
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```
改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。

### Action Creator
View 要发送多少种消息，就会有多少种 Action。如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator。
```
const ADD_TODO = '添加 TODO';

function addTodo(text) {  //  Action Creator函数
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo('Learn Redux');
```

### store.dispatch()
store.dispath()是View发出action的唯一方法。
```
store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});
```

### Reducer
Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。
Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。
```
const reducer = function (state, action) {
  // ...
  return new_state;
};
```

### store.subscribe()
Store允许使用store.subscribe方法设置监听函数，一旦State发生变化，自动执行
```
import { createStore } from 'redux';
const store = createStore(reducer);

store.subscribe(listener);
```
### 实例
```
import { createStore } from 'redux';

// 1 新建store
// 通过reducer建立 
// 根据老的state和action 生成新的state
function counter(state = 0, action){
    switch(action.type){
        case '加人':
            return state + 1
        case '减人':
            return state - 1
        default:
            return 10
    }
}

// 1 新建store容器
const store = createStore(counter);
// 拿出此时的state
const init = store.getState();

console.log(init);
function listener(){
    const current = store.getState();
    console.log(`现在有人${current}个`);
}
// store.subscribe方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。
store.subscribe(listener)

// 派发事件 传递action
store.dispatch({type:'加人'});
store.dispatch({type:'加人'});
store.dispatch({type:'减人'});

```

## 工作流图
![image.png](http://upload-images.jianshu.io/upload_images/2088736-97d6137e9b1098cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Redux和React一起使用
### 如何一起使用？
- 把`store.dispatch`方法传递给组件，内部可以调用修改状态
- `subscribe`订阅`render`函数，每次修改都重新渲染
- `Redux`相关内容，移到单独的文件`redux.js`单独管理

### 实例
首先创建一个`redux.js`
```
const ADD_P = '加人';
const REMOVE_P = '减人';


// reducer
export function counter(state = 0, action){
    switch(action.type){
        case ADD_P:
            return state + 1
        case REMOVE_P:
            return state - 1
        default:
            return 10
    }
}

// action creator
export function addP(){
    return {type:ADD_P}
}
export function removeP(){
    return {type:REMOVE_P}
}
```
然后创建App.js
```
import React from 'react';


class App extends React.Component{
  // constructor(props){
  //     super(props)
  // }
  render(){
    const store = this.props.store;
    const num = store.getState();
    const addP = this.props.addP;
    const removeP = this.props.removeP;
    return (
      <div>
          <h1>现在有人{num}个.</h1>
          <button onClick={() => store.dispatch(addP())}>申请加人</button>
          <button onClick={() => store.dispatch(removeP())}>申请裁员</button>
      </div>
    ) 
        
  }
}

export default App;
```
创建index.js
```
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import App from './App';
import { counter,addP,removeP } from './redux';

const store = createStore(counter)

function render(){
    ReactDOM.render(<App store={store} addP={addP} removeP={removeP} />, document.getElementById('root'));
}
render()

store.subscribe(render);

```