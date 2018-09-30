---
title: React路由(Router)
date: 2017-12-02 13:38:50
tags:
---
## 基本用法
### 安装
```
$ npm install react-router -g
$ npm install react-router --save
```
如果装的是v2 v3`$ npm install react-router@3 --save `
```
import {Router, Route, hashHistory} from 'react-router';
export default class Root extends React.Component{
    render(){
        console.log(hashHistory);
        return(

            //这里替换之前的indnx，变成了程序的入口
            <Router history={hashHistory}>
                <Route component={Index} path="/"></Route>
                <Route component={ComponentList} path="list"></Route>
            </Router>
        )
    }
}

ReactDOM.render(<Root/>,document.getElementById('example'));
```
v4中 `hashHistory`  `console.log(hashHistory); //undefined`
## React Router V4
`< BrowserRouter >`创建一个浏览器的历史，
`< HashRouter >`创建一个哈希的历史，
`< MemoryRouter >`创建一个记忆中的历史。

### 所有组件更改为react-router-dom导入
之前的所有路由组件均是从react-router中导入,在我之前的项目中,导入相关组件如下:
```
//v3
import {Router,Route,hashHistory} from 'react-router';
```
在react-router4 开始,所有的router组件均是从react-router-dom中导入, 所以一下的需要更改为以下代码:
```
//v4
import {Route,BrowserRouter, Switch} from 'react-router-dom';
```
细心的你发现在,到导入的过程中,我删除了Router,但是增加了BorwerRouter和Switch,下面我会一步步的说明.
### 将所有<Router>替换为<BrowserRouter>
```
//v4
<BrowserRouter>
  <div>
    <Route path='/about' component={About} />
    <Route path='/contact' component={Contact} />
  </div>
</BrowserRouter>
```
`v4`中`BrowserRouter`仅能有一个子节点，因此可以选用`div`等进行包裹。y因此选用`switch`
### <Switch>
在V3中，可以指定一些子路由，并且只有第一个匹配的路径将被呈现。
```
// v3
<Route path='/' component={App}>
  <IndexRoute component={Home} />
  <Route path='about' component={About} />
  <Route path='contact' component={Contact} />
</Route>
```
V4提供了类似的功能与<Switch>组件。当呈现<Switch>，它只会渲染与当前位置相匹配的第一个子<Router>与当前位置匹配。
```
// v4
const App = () => (
  <Switch>
    <Route exact path='/' component={Home} />
    <Route path='/about' component={About} />
    <Route path='/contact' component={Contact} />
  </Switch>
)
```
### Link用法
```
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

const BasicExample = () => (
  <Router>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>

      <hr />

      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/topics" component={Topics} />
    </div>
  </Router>
);
```
[Migrating from v2/v3 to v4](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/migrating.md3)

## Router嵌套
页面中局部进行路由
![image.png](http://upload-images.jianshu.io/upload_images/2088736-477bfc4def18120e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Router参数传递
路由中写“:参数名”
![image.png](http://upload-images.jianshu.io/upload_images/2088736-42774980c9b66ad2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Link中接参数值
![image.png](http://upload-images.jianshu.io/upload_images/2088736-500711d14e206551.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Component中接收参数
`this.props.match.params.id
`
![image.png](http://upload-images.jianshu.io/upload_images/2088736-f7688e3bcf29f893.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时的浏览器中会显示 **参数值**
`http://localhost:8080/#/ticket/123456`