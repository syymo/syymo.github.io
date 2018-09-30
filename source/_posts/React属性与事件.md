---
title: React属性与事件
date: 2017-11-18 17:46:24
tags: React
---
## State属性
state 对于模块属于 自身 属性
### 初始化state
初始化：` this.state = {name:"syymo"} `
初始化可放在
```
//初始化
constructor(){
    super(); //调用基类的所有初始化方法
    this.state = {name: "Syymo"};   //state值作用于当前作用域
}
```
react组件中的this作用于此组件中作用域。
### 修改state属性
```
render(){
    //箭头函数 也称作 匿名函数
    setTimeout(()=>{
        //更改state的时候
        this.setState({name: "syymo"});
    },3000);
}
/*
self = this;
setTimeout(function(){
    //更改state的时候
    self.setState({name: "syymo"});
},3000);
*/
```
**state的作用域只属于当前类，不污染其他模块**


## Props属性
props对于模块属于 外来 属性。
父组件向子组件传递参数：
传递参数可以通过：`<IndexMain name="Syymo"/>`
模块中接受参数：`this.props.name`
对象的属性与组件的属性一一对应，但是有一个例外`this.props.children`属性。
`...this.props`把父页面的所有属性传给子页面
### this.props.children 
 this.props.children 表示组件的所有子节点。
```
var NodeList = React.createClass({
    render: function(){
        return (
            <ol>
            {   
                //this.props.children表示组件的所有字节点
                React.Children.map(this.props.children, function(child){
                    return <li>{child}</li>
                })
            }
            </ol>
        );
    }           
});
ReactDOM.render(
    <NodeList>
        <span>hello</span>
        <span>React</span>
        <h1>JavaScript</h1>
    </NodeList>,
    document.body
)
```
##### this.props.children的值有三种可能：
 - 如果当前组件没有子节点，它返回undefined
- 如果有一个子节点，数据类型是object
- 如果有多个子节点，数组类型是array

####PropTypes
组件的属性可以接受任意值，字符串、对象、函数等等都可以。用于验证别人使用组件时，提供的参数是否符合要求。
```
import PropTypes from 'prop-types';
var MyTitle = React.createClass({
  /* getDefaultProps 方法可以用来设置组件属性的默认值。当组件有值时getDefaultProps值不会显示
 getDefaultProps : function () {
    return {
      title : 'Hello World'
    };
  },
  */
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },
  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});
```
其中`PropTypes`告诉React，MyTitle组件有一个title属性，而且title属性是一个`string`。
如果没有title为`string`的值则会报错
`isRequired`意思为必传参数。

![报错信息](http://upload-images.jianshu.io/upload_images/2088736-218f2757cc84f4f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
import PropTypes from 'prop-types';
MyComponent.propTypes = {
    //  可以声明 prop 为指定的 JS 基本类型。默认 情况下，这些 prop 都是可传可不传的。  
    optionalArray: PropTypes.array,  
    optionalBool: PropTypes.bool,  
    optionalFunc: PropTypes.func,  
    optionalNumber: PropTypes.number,  
    optionalObject: PropTypes.object,  
    optionalString: PropTypes.string,  
  
    //  可以呈现的任何东西：数字、字符串、元素或数组(或片段)包含这些类型。
    optionalNode: PropTypes.node,  
  
    // React 元素  
    optionalElement: PropTypes.element,  
  
    // 用 JS 的 instanceof 操作符声明 prop 为类的实例。  
    optionalMessage: PropTypes.instanceOf(Message),  
  
    // 用 enum 来限制 prop 只接受指定的值。  
    optionalEnum: PropTypes.oneOf(['News', 'Photos']),  
  
    // 指定的多个对象类型中的一个  
    optionalUnion: PropTypes.oneOfType([  
        PropTypes.string,  
        PropTypes.number,  
        PropTypes.instanceOf(Message)  
    ]),  
  
    // 指定类型组成的数组  
    optionalArrayOf: PropTypes.arrayOf(React.PropTypes.number),  
  
    // 指定类型的属性构成的对象  
    optionalObjectOf: PropTypes.objectOf(React.PropTypes.number),  
  
    // 特定形状参数的对象  
    optionalObjectWithShape:PropTypes.shape({  
        color: React.PropTypes.string,  
        fontSize: React.PropTypes.number  
    }),  
  
    // 以后任意类型加上 `isRequired` 来使 prop 不可空。  
    requiredFunc: PropTypes.func.isRequired,  
  
    // 不可空的任意类型  
    requiredAny: PropTypes.any.isRequired,  
  
    // 自定义验证器。如果验证失败需要返回一个 Error 对象。不要直接  
    // 使用 `console.warn` 或抛异常，因为这样 `oneOfType` 会失效。  
    customProp: function(props, propName, componentName) {
        if (!/matchme/.test(props[propName])) {
            return new Error(
                'Invalid prop `' + propName + '` supplied to' +' `' + componentName + '`. Validation failed.'
            );
        }
    },
    //  你也可以去`arrayOf`和`objectOf`对象提供自定义验证器。
    //  如果验证失败，它应该返回一个错误对象。
    //  验证器将调用数组或对象中的每个键。
    //   前两个参数验证程序是数组或对象本身，和当前项的键。
    customArrayProp: PropTypes.arrayOf(function(propValue, key,       
        componentName, location, propFullName) {
            if (!/matchme/.test(propValue[key])) {
                return new Error(
                    'Invalid prop `' + propFullName + '` supplied to' +        ' `' + componentName + '`. Validation failed.'
                );
            }
        }
    })
};
```
## 事件与数据的双向绑定
### 事件的绑定
可以再构造函数中绑定
`
this: this.forceUpdateHandler = this.forceUpdateHandle.bind(this);
`
调用时绑定
```
changeUserInfo(){
    this.setState({age : 22});
}
return (
    <div>
        <p>{this.state.age}</p>
        <input type="button" value="提交" onClick={this.changeUserInfo.bind(this)}/>
    </div>
)
```
### 双向绑定
```
changeChildValueChange(event){
    this.setState({age : event.target.value});
};
render(){
    return (
        <div>
            <div>
                <p>{this.state.age}</p>
                <p>页面输入：<input type="text" onChange={this.props.handleChildValueChange}/></p>
            </div>
        </div>
    )
}
```

## Refs
在react中获取dom节点有多种方式
```
方式一
<input id="submitButton" ref="submitButton" type="button" value="提交" onClick={this.changeUserInfo.bind(this)}/>

var mySubmitButton = document.getElementById("submitButton");
ReactDOM.findDOMNode(mySubmitButton).style.color = 'red';
```
此方式基本使用了原生JavaScript所以不推荐使用。

```
方式二
<input id="submitButton" ref="submitButton" type="button" value="提交" onClick={this.changeUserInfo.bind(this)}/>

this.refs.submitButton.style.color = 'red';
```
`Refs`是访问到组件内部DOM节点唯一可靠的方法
`Refs`会自动销毁对子组件的引用
- 不要在`render`或`render`之前对`Refs`进行调用
- 不要滥用`Refs`

## 独立组件共享Mixin
- 不同的组件之间公用功能、共享代码
- 和页面有相同的生命周期
`es`中不支持`mixin`但是安装`react-mixin`插件可以进行支持：
`
npm install react-mixin@2.0.2 --save
`
建立的minins.js文件
```
const MixinLog = {
    log(){
        console.log("abcdefg...");
    }
};
export default MixinLog;
```
在组件中引用
```
import ReactMixin from 'react-mixin'
import MixinLog from './mixins';

//  其他组件中调用方法
changeUserInfo(){
    MixinLog.log();
}
ReactMixin(BodyIndex.prototype, MixinLog);
```
