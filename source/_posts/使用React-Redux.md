---
title: 使用React-Redux
date: 2018-03-13 13:13:40
tags: Redux
---
## 为了方便管理，使用专门关键字负责链接
- `npm install react-redux --save`
- 忘记`subscribe`，记住`reducer`，`action`和`dispatch`即可
- 使用`React-Redux`提供的`Provider`和`connect`两个接口链接

## 具体使用
### `Provider`组件在应用在外层，传入`store`即可，只用一次
```
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';
import { Provider } from 'react-redux';
import App from './App';
import { counter } from './redux';

const reduxDevtools = window.devToolsExtension?window.devToolsExtension():f=>f;


const store = createStore(counter, compose(
    applyMiddleware(thunk), 
    reduxDevtools
));

ReactDOM.render(
    (<Provider store={store}>
        <App />, 
    </Provider>)
    document.getElementById('root')
);

```
### `Connect`负责从外部获取组件需要的参数
```
import React from 'react';
import { connect } from 'react-redux';
import {addP, removeP, addPAsync } from './redux'

class App extends React.Component{
  render(){
    return (
      <div>
          <h1>现在有人{this.props.num}个.</h1>
          <button onClick={this.props.addP}>申请加人</button>
          <button onClick={this.props.removeP}>申请裁员</button>
          <button onClick={this.props.addPAsync}>延迟两秒</button>
      </div>
    ) 
  }
}

// 需要的参数
const mapStatetoProp = (state) => {
  return { num:state }
}
const actionCreators = { addP, removeP, addPAsync }
// 传递
App = connect(mapStatetoProp, actionCreators)(App)
// 导出组件
export default App;
```
## `Connect`可以用装饰器的方式来写
### 使用装饰器优化
`npm run eject` 弹出个性化配置
![image.png](http://upload-images.jianshu.io/upload_images/2088736-cf22b320b1044cab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
npm install babel-plugin-transform-decorators-legacy 插件
并在package.json中增加如下配置
![image.png](http://upload-images.jianshu.io/upload_images/2088736-e67171ce786178d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时若报错
![image.png](http://upload-images.jianshu.io/upload_images/2088736-2b8126179e6dd616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在`.babelrc`中增加

![image.png](http://upload-images.jianshu.io/upload_images/2088736-fa86e7b0e105f890.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时代码可以改为 `@connect(mapStatetoProp, actionCreators)`
```
import React from 'react';
import { connect } from 'react-redux';
import {addP, removeP, addPAsync } from './redux'

// 需要的参数
// const mapStatetoProp = (state) => {
//   return { num:state }
// }
// const actionCreators = { addP, removeP, addPAsync }
// 传递
// App = connect(mapStatetoProp, actionCreators)(App)
// 代码精简
// @connect(mapStatetoProp, actionCreators)
@connect(
  // 你要state的什么属性放到props里
  state => ({num:state}),
  // 你要什么方法，放到props里，自动dispatch
  { addP, removeP, addPAsync }
)

class App extends React.Component{
  render(){
    return (
      <div>
          <h1>现在有人{this.props.num}个.</h1>
          <button onClick={this.props.addP}>申请加人</button>
          <button onClick={this.props.removeP}>申请裁员</button>
          <button onClick={this.props.addPAsync}>延迟两秒</button>
      </div>
    ) 
  }
}

// 导出组件
export default App;
```
如果@报错在.babelrc中加入
 "plugins": ["transform-decorators-legacy"]
![image.png](http://upload-images.jianshu.io/upload_images/2088736-8a90f1fbcb6b7288.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Reducer函数合并 使用`combinReducers`
```
// 合并所有reducer 并且返回
import { combineReducers } from 'redux';
import { counter } from './index.redux';
import { auth } from './Auth.redux';

export default combineReducers({counter,auth})
```
然后页面中导入
```
import reducers from "./reducer"
const reduxDevtools = window.devToolsExtension?window.devToolsExtension():f=>f;
const store = createStore(reducers, compose(
    applyMiddleware(thunk), 
    reduxDevtools
));
```

