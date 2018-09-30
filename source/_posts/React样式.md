---
title: React样式
date: 2017-11-22 18:12:56
tags: React
---
## 内联样式
### 写在render函数里定义的
虽然不会造成css污染但是**写起来比较复杂**
hover等伪类、动画不能实现
```
//写在class外面或者render里面
export default class ComponentHeader extends React.Component{
    render(){
        const styleComponentHeader = {
            header: {
                backgroundColor: "#333333",
                color: "#FFFFFF",
                //"padding-top": "15px"
                //尽量写成jsx格式
                paddingTop: "15px",
                paddingBottom: "15px",
            },
            ul:{
                width: "50px"
            }
            //还可以定义其他的样式
        }
        return (
            <header style={styleComponentHeader.header}>
                <ul style={styleComponentHeader.ul}>
                    <li>首页</li>
                    <li>门票</li>
                    <li>设备</li>
                    <li>纪念品</li>
                </ul>
            </header>
        )
    }
}
```
### 文件引入css样式
`class`需要改为`className`使用`babel-plugin-react-html-attrs`可以继续使用`class`
`npm install babel-plugin-react-html-attrs `
此时的`css`为全局的`css`，当其他页面也有次`class`名称时会造成污染
```
//创建style.css样式文件
.haerderMenu{
    height:60px;
}
.haerderMenu ul li {
    float:left;
    list-style:none; 
    margin-left:30px;
}
.haerderMenu ul li a{
    color:#fff;
    text-decoration:none; 
}
```
```
//在HTML中引入
<header>
    <link rel="stylesheet" href="./src/css/style.css" />
</header>
```
```
//组件中的class名字改为className
export default class ComponentHeader extends React.Component{
    render(){
        return (
            <header className="haerderMenu">
                <ul>
                    <li>首页</li>
                    <li>门票</li>
                    <li>设备</li>
                    <li>纪念品</li>
                </ul>
            </header>
        )
    }
}
```
## 内联样式中的表达式
此时的`state`引起了样式的即时变化
通过内联样式的表单式改变页面的样式
```
paddingTop: (this.state.miniHeader) ? "15px" : "25px",
paddingBottom: (this.state.miniHeader) ? "15px" : "25px"
```
```
export default class ComponentHeader extends    React.Component{
    constructor(){
        super();
        this.state = {
            miniHeader: false   // 默认加载的是高的头部
        }
    };
    switchHeader(){
        this.setState({
            miniHeader: !this.state.miniHeader
        });
    };
    render(){
        const styleComponentHeader = {
            header: {
                backgroundColor: "#333333",
                color: "#FFFFFF",
                paddingTop: (this.state.miniHeader) ? "15px" : "25px",
                paddingBottom: (this.state.miniHeader) ? "15px" : "25px"
            },
            ul:{
                width: "50px"
            }
            //还可以定义其他的样式
        }
        return (
            <header style={styleComponentHeader.header} onClick={this.switchHeader.bind(this)}>
                <ul style={styleComponentHeader.ul}>
                    <li>首页</li>
                    <li>门票</li>
                    <li>设备</li>
                    <li>纪念品</li>
                </ul>
            </header>
        )
    }
}
```
## CSS模块化
###  CSS模块化解决的问题
- 避免全局污染
- 避免命名混乱
- 避免无法共享变量
- 避免代码压缩不彻底
###  CSS模块化使用
首先需要2个插件
[css-loader](https://github.com/webpack-contrib/css-loader)、[style-loader](https://github.com/webpack-contrib/style-loader)
全局样式和组件样式
```
// 组件内部  默认为local
:local(.normal){color:green}
// 全局各个组件共享
:global(.btn){color:red}
```
```
npm install css-loader 
npm install style-loader
```
webpack.config.js中的设置
```
module: {
    loaders: [
        //css的loader，也就是css模块化的配置方法
        {
            test: /\.css$/,
            loader: 'style-loader!css-loader?modules&importLoaders=1&localIdentName=[name]_[local]'
        },
    ]
}
```
首先创建css文件并且写样式，然后在组件中调用css
```
.miniFooter{
    background-color:#000000;
    color:#FFFFFF;
    padding-left:20px;
    padding-top:3px;
    padding-bottom:3px;
}

.miniFooter h1{
    font-size:15px;
}
```
```
.miniFooter{
    background-color:#000000;
    color:#FFFFFF;
    padding-left:20px;
    padding-top:3px;
    padding-bottom:3px;
}

.miniFooter h1{
    font-size:15px;
}
// 组件中调用
var footerCss = require("../../css/footer.css");
export default class ComponentFooter extends React.Component{
    render(){
        //console.log(footerCss);
        return (
            <footer className={footerCss.miniFooter}>
                <h1>这里是页脚，放一些版权信息。</h1>
            </footer>
        )
    }
}
```
###  CSS模块化优点
- 所有样式都是local的，解决了命名冲突和全局污染问题
- `class`名生成规则配置灵活，可以以此压缩`class`名
- 只需要引用组件的`js`就能搞定组件的所有`js`和`css`
- 依然是`css`

## JSX样式与CSS的互传

![CSS to React](https://staxmanade.com/CssToReact/images/CssToReact-logo.svg)
[CssToReact](https://staxmanade.com/CssToReact/)这个简单的小工具旨在帮助将普通CSS转换为反应式内嵌样式的特定JSON表示。使复制和粘贴成为内联反应组件变得容易。

## Ant Design 样式框架介绍
主要是封装好的UI框架，供我们直接使用
谷歌开发的UI[Meterial UI](http://www.material-ui.com/#/)、蚂蚁金服开发的UI[Ant Design](https://ant.design/index-cn)

## Ant Design 样式框架使用
### 安装
```
$ npm install antd --save
```
### 导入样式
全部倒入
```
import 'antd/dist/antd.css';
```
按需导入
- 使用`babel-plugin-import`（官方推荐）
首先下载 [babel-plugin-import](https://github.com/ant-design/babel-plugin-import)
`npm install babel-plugin-import `
配置`webpack.config.js`：
```
const config = {
    //入口
    context: __dirname + '/src',    //全局状态下的目录
    entry: './js/index.js',
    module: {
        loaders: [
            //js的loader
            {
                test: /\.(js|jsx)$/,
                exclude: /(node_modules)/,
                loader: 'babel-loader',
                options:{
                    presets: ['react', 'es2015'],
                    plugins: [

                        ['import', {libraryName: 'antd', style: 'css'}]
                    ],
                },
            },
            {
                test: /\.css$/,
                loader: 'style-loader!css-loader'
            },
        ]
    },
    //出口
    output: {
        path: __dirname +　"/src/",
        filename: 'bundle.js'
    }
};
module.exports = config;
```
或者创建`.babelrc`并配置：
```
{
  "plugins": [
    ["import", { libraryName: "antd", style: "css" }] // `style: true` 会加载 less 文件
  ]
}
```
然后只需从 antd 引入模块即可，无需单独引入样式。等同于下面手动引入的方式。
// babel-plugin-import 会帮助你加载 JS 和 CSS
import { DatePicker } from 'antd';


- 手动引入
import DatePicker from 'antd/lib/date-picker';  // 加载 JS
import 'antd/lib/date-picker/style/css';        // 加载 CSS
// import 'antd/lib/date-picker/style';         // 加载 LESS
### 使用
```
import { Input } from 'antd';

ReactDOM.render(<Input placeholder="Basic usage" />, mountNode);
```