---
title: React生命周期
date: 2017-11-10 18:16:14
tags: React
---
React生命周期图
![图片来自网络](http://upload-images.jianshu.io/upload_images/2088736-2927919b3351d73e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以把组件生命周期大致分为三个阶段：

> 第一阶段：是组件第一次绘制阶段，如图中的上面虚线框内，在这里完成了组件的加载和初始化；
第二阶段：是组件在运行和交互阶段，如图中左下角虚线框，这个阶段组件可以处理用户交互，或者接收事件更新界面；
第三阶段：是组件卸载消亡的阶段，如图中右下角的虚线框中，这里做一些组件的清理工作。

生命周期回调函数总共有10个。

> object getDefaultProps() 
在组件类创建的时候调用一次，然后返回值被缓存下来。全局调用一次，所有实例共享。

>  object getInitialState() 
在组件挂载之前调用一次。返回值将会作为 this.state 的初始值。

> componentWillMount () 
在初始化渲染执行之前立刻调用

> ReactComponent render() 
这个方法是必须的，对视图进行渲染，你也可以返回 null 或者 false 来表明不需要渲染任何东西

> componentDidMount() 
在初始化渲染执行之后立刻调用一次

> componentWillReceiveProps(object nextProps) 
在组件接收到新的 props 的时候调用，也就是父组件修改子组件的属性时触发。在初始化渲染的时候，该方法不会调用。可以用于更新 state 来响应某个 prop 的改变。

> boolean shouldComponentUpdate(object nextProps, object nextState) 
在接收到新的 props 或者 state，将要渲染之前调用,如果确定新的 props 和 state 不会导致组件更新，则此处应该 返回 false。返回true将进行渲染。

> componentWillUpdate(object nextProps, object nextState) 
在接收到新的 props 或者 state 并且shouldComponentUpdate返回true时调用

> componentDidUpdate(object prevProps, object prevState) 
在组件的更新已经同步到 DOM 中之后立刻被调用

> componentWillUnmount() 
在组件从 DOM 中移除的时候立刻被调用。在该方法中执行任何必要的清理，比如无效的定时器，或者清除在 componentDidMount 中创建的 DOM 元素。

各个阶段的意义：
```
var Button = React.createClass({
    getInitialState: function() {
        return {
            data:0
        };
    },
    setNewNumber: function() {
        this.setState({data: this.state.data + 1})
    },
    render: function () {
        return (
            <div>
                <button onClick = {this.setNewNumber}>INCREMENT</button>
                <Content myNumber = {this.state.data}></Content>
            </div>
        );
    }
})
var Content = React.createClass({
    componentWillMount:function() {
        console.log('componentWillMount：在渲染前调用,在客户端也在服务端。')
    },
    componentDidMount:function() {
        console.log('componentDidMount：在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。')
        },
    componentWillReceiveProps:function(newProps) {
        console.log('componentWillReceiveProps：在组件接收到一个新的prop时被调用。这个方法在初始化render时不会被调用。')
        },
    shouldComponentUpdate:function(newProps, newState) {
        return true;
        console.log('shouldComponentUpdate：返回一个布尔值。在组件接收到新的props或者state时调用。在初始化时使用forceUpdate时不被调用。可以在你确认不需要更新组建时使用。')
    },
    componentWillUpdate:function(nextProps, nextState) {
        console.log('componentWillUpdate：在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。');
    },
    componentDidUpdate:function(prevProps, prevState) {
        console.log('componentDidUpdate：在组件完成更新后立即调用。在初始化时不会被调用。')
    },
    componentWillUnmount:function() {
        console.log('componentWillUnmount：宰祖建从DOM中移除的时候立刻被调用。')
    },

    render: function () {
        return (
            <div>
                <h3>{this.props.myNumber}</h3>
            </div>
        );
    }
});
ReactDOM.render(

    <div>
        <Button />
    </div>,
    document.getElementById('example')
);
```