---
title: React组件基本用法
date: 2017-11-15 08:48:35
tags: React
---
## React.createClass
使用React.createClass方法生成一个组件类并输出。
例如：
```
//生成组件
var HelloMessage = React.createClass({
  render: function() {
    return <h1>Hello React! </h1>;
  }
});
//输出组件
ReactDOM.render(
  <HelloMessage  name=“React”/>,
  document.getElementByID('example');
)
```
> 注意：原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。

> 组件名不一定用单标签:
> <HelloMessage /> == <HelloMessage></HelloMessage>
## 向组件传递参数使用this.props对象
如果我们需要向组件传递参数，可以使用this.props对象
```
var HelloMessage = React.createClass({
  render:function(){
    return <h1>Hello {this.props.name}!</h2>;
  }
})
```

## 复合组件
React可以创建多个组件来合成一个组件，把组件不同功能进行分离。
```
var WebSite = React.createClass({
  render: function(){
    return (
      <div>
        <Name name={this.props.name} />
        <Link site={this.props.site} />
      </div>
    )
  }
})
var Name = React.createClass({
  render: function() {
    return (
      <h1>{this.props.name}</h1>
    );
  }
});
 
var Link = React.createClass({
  render: function() {
    return (
      <a href={this.props.site}>
        {this.props.site}
      </a>
    );
  }
});
 
ReactDOM.render(
  <WebSite name="博客" site=" http://syymo.cn" />,
  document.getElementById('example')
);
```
## 父组件向子组件传值
```
//父组件
return (
  <div>
    <BodyIndex userId={220}/>
  </div>
)

//子组件
render(){
  return (
    <p>{this.props.userId}</p>    
  )
}
```
## 子组件向父组件传值
在子页面通过调用父页面传递过来的事件`props`进行组件间的参数传递
```
// 子组件
render(){
  return(
    <div>
      <p>子页面输入：<input type="text" onChange={this.props.handleChildValueChange}/></p>
    </div>
  )
}

//父组件
changeChildValueChange(event){
  this.setState({age : event.target.value});
};
return (
  <div>
    <h2>这里是主体部分。</h2>
    <p>{this.state.name}{this.state.age}</p>
    <BodyChild handleChildValueChange = {this.changeChildValueChange.bind(this)}/>
  </div>
)
```

<!-- 此教程学习参考[菜鸟学院](http://www.runoob.com/react/react-tutorial.html) -->