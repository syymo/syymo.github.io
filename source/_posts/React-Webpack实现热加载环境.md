---
title: React+Webpack实现热加载环境
date: 2017-09-24 16:16:16
tags: React
---
## 首先下载node
首先下载[nodejs](http://nodejs.cn/)并且安装好。


紧接着在nodejs路径下新建
node_global(全局存放路径)和node_cache(全局缓存路径)文件夹


![](http://upload-images.jianshu.io/upload_images/2088736-2a6bf2cc34086397.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

并且配置环境变量

![node环境变量](http://upload-images.jianshu.io/upload_images/2088736-4d593c22df13fd54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时在cmd窗口分别输入
```
npm config set prefix "d:\nodejs\node_global"

npm config set cache "d:\nodejs\node_cache"
```
最后在nodejs的安装目录中找到node_modules\npm\.npmrc文件
修改如下即可：
```
prefix = E:\nodejs\node_global
cache = E:\nodejs\node_global
```
######运行后无结果则是成功。



## 更改npm下载源 官方源太慢
### 1.临时使用
```
npm --registry https://registry.npm.taobao.org install express
```

### 2.持久使用

```
npm config set registry https://registry.npm.taobao.org
```

配置后可通过下面方式来验证是否成功
    `npm config get registry`
 或
    `npm info express`
    `npm config list`


### 3.通过cnpm使用

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
使用
    `cnpm install express`




现在下载webpack

![安装webpack](http://upload-images.jianshu.io/upload_images/2088736-b694c325a4ebe8e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
//安装全局webpack
npm install webpack -g
```
-g表示全局 
显示图中所示即安装成功

`npm install webpack-dev-server -g` 
此插件不为必须安装 node官方推荐使用

安装并环境变量配置完成后
![](http://upload-images.jianshu.io/upload_images/2088736-7d5df678a99d5f08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
显示版本号则成功

若有人输入webpack提示不是内部命令则说明未配置NODE_PATH环境变量。


等待一切准备完成之后
## 开始新建react项目

### 1. 新建项目文件夹
此处我建立的react文件夹，dos`cd react`进入新建的react目录下

### 2. 创建一个package.json文件
在练习文件夹中创建一个package.json文件，这是一个npm说明文件，里面蕴含了丰富的信息，包括当前项目的依赖模块，自定义的脚本任务等等。在终端中使用npm init命令可以自动创建这个package.json文件
```
npm init
```

![初始化过程](http://upload-images.jianshu.io/upload_images/2088736-ec840ae0825ce0c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
安装过程中一路回车即可

此时练习文件夹中会生成
![生成的](http://upload-images.jianshu.io/upload_images/2088736-e88ec3927b13e3b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3. 安装webpack模块，react模块，react-dom模块
此时在练习文件夹下安装webpack模块，react模块，react-dom模块

```
//安装项目目录
npm install --save-dev webpack
npm install --save-dev webpack-dev-server(非必须)
npm install --save-dev react
npm install --save-dev react-dom
```

![react react-dom](http://upload-images.jianshu.io/upload_images/2088736-091d8ffe08e06fc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![webpack](http://upload-images.jianshu.io/upload_images/2088736-5d47e5e32155a6c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4. 安装babel支持
`npm install babel-preset-es2015 babelify babel-preset-react babel-plugin-react-html-attrs babel-loader --save`

### 5. 新建index.html,index.html
- 在根目录下新建index.html,index.html代码如下
```
<div id="example">Hello react</div>
<script src="./src/bundle.js"></script>
```

![运行结果](http://upload-images.jianshu.io/upload_images/2088736-dc372d19427f1974.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 在根目录下新建src文件夹
- 在src文件夹下新建js文件夹
- 在js文件夹下新建index.js文件, index.js内容如下
```
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(
    <h1>Hello World!</h1>,
    document.getElementById('example')
)
```
- 在根目录新建webpack.config.js,webpack.config.js内容如下
```
var debug = process.env.NODE_ENV !== "production";
var webpack = require('webpack');
var path = require('path');

module.exports = {
  context: path.join(__dirname),
  devtool: debug ? "inline-sourcemap" : null,
  entry: "./src/js/index.js", //这里是入口文件地址,可根据自身的位置进行修改
  module: {
    loaders: [
      {
        test: /\.js?$/,
        exclude: /(node_modules)/,
        loader: 'babel-loader',
        query: {
          presets: ['react', 'es2015'],
          plugins: ['react-html-attrs'], //添加组件的插件配置
        }
      },
      //下面是使用 ant-design 的配置文件
      { test: /\.css$/, loader: 'style-loader!css-loader' }
    ]
  },
  output: {
    path: __dirname,
    filename: "./src/bundle.js"
  },
  plugins: debug ? [] : [
    new webpack.optimize.DedupePlugin(),
    new webpack.optimize.OccurenceOrderPlugin(),
    new webpack.optimize.UglifyJsPlugin({ mangle: false, sourcemap: false }),
  ],
};
```


```
![文件夹结构](http://upload-images.jianshu.io/upload_images/2088736-d2b4d1ca901e10c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
新建`.babelrc`配置如下

```
{
    "plugins": [
      ["import", { libraryName: "antd", style: "css" }] // `style: true` 会加载 less 文件
    ],
    "presets": [
        "es2015",
        "react",
        "stage-3"  //有些语法需要次
    ],
}

### 6. webpack

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2088736-cc21e6f6584e8ff3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行index.html
![运行界面](http://upload-images.jianshu.io/upload_images/2088736-cac677ba61b37d68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 7. 实现热加载
热加载：每次修改代码不需要在进行webpack打包

webpack-dev-server会创建一个本地服务器,我们在编辑器中编辑后,就能实时的在浏览器中看到效果

### 8.webpack打包项目
npm run webpack

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2088736-2853b75de2d5aa08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


此文章参考[Lee_tanghui](http://www.jianshu.com/p/6e18ad661540)