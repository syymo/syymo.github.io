---
title: webpack笔记
date: 2018-10-03 11:53:38
tags: webpack
---
## webpack是什么?
官方的说法就是：
> [webpack](https://webpack.github.io/)其实就是一个现代的JavaScript应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

![webpack](webpack笔记/what-is-webpack.png)

[webpack 中文文档](https://www.webpackjs.com/concepts/)

当然简单点说webpack可以说是前端自动化打包工具，webpack高度可配置，要理解webpack之前我们先需要了解四个核心概念

- 入口entery
- 输出output
- loaders
- 插件plugins

## 入口entery

入口顾名思义就是进入webpack的通道，告诉`webpack`从这里进入。
入口`entery`起点指示`webpack`应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，`webpack`会找出有哪些模块和库是入口起点(直接和间接)依赖。
### 用法
```
module.export = {
  entry: {
    app: './src/app.js',
  }
}
```
可以设置一个或者多个入口`entery`。默认路径`./src`

### 单个入口(简写)语法

用法：`entry: string | Array<string>`

```
const config = {
  entry: './src/app.js'
};

module.exports = config;
```
`entry`属性的单个入口语法，是下面的简写：
```
const config = {
  entry: {
    main './src/app.js'
  }
};
```
当我们想`entry`传入一个数组的时候将创建多个主入口`(multi-main entry)`。当我们需要多个依赖文件以启注入，并且将他们的依赖导向(graph)到一个“chunk”时，出入数组的方式就很有用。

当我们正在寻找只有一个入口起点的应用程序或工具，快速设置webpack配置的时候，这回是个不错的选择，但是这样的语法在拓展配置时有失灵活性。

### 对象语法

用法：`entry: { [ entryChunkName: string ]: string | Array<string> }`
```
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```
对象语法会比较繁琐。这是程序中定义入口的最可拓展的方式。
> “可扩展的 webpack 配置”是指，可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点(concern)从环境(environment)、构建目标(build target)、运行时(runtime)中分离。然后使用专门的工具（如 webpack-merge）将它们合并。

### 常见场景
#### 分离 应用程序(app) 和 第三方库(vendor) 入口
```
const config = {
  entry: {
    app: './src/app.js',
    vendors: 'src/vendors.js'
  }
}
```
> 这是什么？从表面上看，这告诉我们 webpack 从 app.js 和 vendors.js 开始创建依赖图(dependency graph)。这些依赖图是彼此完全分离、互相独立的（每个 bundle 中都有一个 webpack 引导(bootstrap)）。这种方式比较常见于，只有一个入口起点（不包括 vendor）的单页应用程序(single page application)中。

> 为什么？此设置允许你使用 CommonsChunkPlugin 从「应用程序 bundle」中提取 vendor 引用(vendor reference) 到 vendor bundle，并把引用 vendor 的部分替换为 __webpack_require__() 调用。如果应用程序 bundle 中没有 vendor 代码，那么你可以在 webpack 中实现被称为长效缓存的通用模式。

#### 多页面应用程序
```
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
}
```
> 这是什么？我们告诉webpack需要三个独立分离的依赖图。
> 为什么？在多页面应用中，（译注：每当页面跳转时）服务器将会为你获取一个新的HTML文档。页面重新加载新文档，而且资源被重新下载。然而，这给了我们特殊的机会去做很多事：
> 使用CommonsChunkPlugin为每个页面间的应用程序共享代码创建按bundle。由于入口起点增多，多页面应用能够复用入口起点之间的大量代码/模块，从而极大地从这些技术中受益。


## 出口output

出口`output`属性是告诉webpack在哪里输出它所创建的`bundles`，以及给这些文件起什么名字，默认值为`./dist`。基本上，整个应用程序结构，都会被编译到指定的输出路径的文件夹中。即使可以存在多个入口起点，但只能指定一个输出配置。


### 用法

`filename` 用于输出`webpack bundle`的文件名。
`path`  输出文件`webpack bundle`目录`path`的绝对路径。

```
const path = reqire('path);  // Node.js用于操作文件按路径的模块

module.exports = {
  entry: {
    app: './src/app.js',
  },
  output: {
    filename: 'bundle.js',  // 文件名
    path: path.resolve(__dirname, 'dist'),  // 文件路径
  }
}
```
此配置是指将`./src/app.js` 打包成`bundle.js` 然后输出到`path.resolve(__dirname, 'dist')`目录下。

### 多个入口起点

如果配置了多个单独的“chunk”(例如，使用多个入口起点或使用像CommonsChunkPlugin这样的插件)，则应该使用占位符来确保每个文件具有唯一的名称。
```
const path = reqire('path);  // Node.js用于操作文件按路径的模块

module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/seatch.js',
  },
  output: {
    filename: '[name].[chunkhash]',  // 文件名
    path: path.resolve(__dirname, 'dist'),  // 文件路径
  }
}

// 写入到硬盘中：./dist/app.js  ./dist/search.js
```
### 高级进阶
使用CDN和资源hash的复杂实例：
```
output: {
  path: "/home/proj/cdn/assets/[hash]",
  publicPath: "http://cdn.example.com/assets/[hash]/"
}
```
在编译时不知道最终输出文件的 publicPath 的情况下，publicPath 可以留空，并且在入口起点文件运行时动态设置。如果你在编译时不知道 publicPath，你可以先忽略它，并且在入口起点设置 __webpack_public_path__。
```
__webpack_public_path__ = myRuntimePublicPath
```

#### hash 
hash是跟整个项目的构建相关，只要项目里有文件更改，整个项目构建的hash值都会更改，并且全部文件都共用相同的hash值

#### chunkhash
chunkhash和hash不一样，它根据不同的入口文件(Entry)进行依赖文件解析、构建对应的chunk，生成对应的哈希值。我们在生产环境里把一些公共库和程序入口文件区分开，单独打包构建，接着我们采用chunkhash的方式生成哈希值，那么不改动公共库的代码，就可以保证其哈希值不会受影响。

#### contenthash
当index.css被index.js引用了，所以共用相同的chunkhash值。但是这样子有个问题，如果index.js更改了代码，css文件就算内容没有任何改变，由于是该模块发生了改变，导致css文件会重复构建。
这个时候，我们可以使用extra-text-webpack-plugin里的contenthash值，保证即使css文件所处的模块里就算其他文件内容改变，只要css文件内容不变，那么不会重复构建。

## loader

由于`webpack`本身只能理解`JavaScript文件`,所以`loader`的出现就是让`webpack`能够去处理那些非`JavaScript`文件。`loader`的可以将那些其他类型的文件转换成`webpack`能够处理的有效模块，然后通过`webpack`的打包能力，对它们进行处理。

本质上`webpack loader`将所有类型的文件，转换为应用程序的依赖图(和最终的bundle)可以直接引用的模块。

>注意，loader 能够 import 导入任何类型的模块（例如 .css 文件），这是 webpack 特有的功能，其他打包程序或任务执行器的可能并不支持。我们认为这种语言扩展是有很必要的，因为这可以使开发人员创建出更准确的依赖关系图。

### 使用loader方式

有三种使用loader方式：
- 配置（推荐）：在webpack.config.js文件中指定loader。
- 内联：在每个import语句中显式指定loader。
- CLI：在shell命令中指定他们。

#### 配置

在是使用loader的时候需要安装相应的loader:
```
npm install --save-dev css-loader
npm install --save-dev ts-loader
```

在`webpack`的配置中`loader`有两个配置项:
- test 用于识别出应该被队形的loader进行转换的某个或某些文件。
- use 用于进行转换时，应该使用那个loader。

```
const path = require('path');

module.exports = {
  module: {
    reules: [
      test: /\.(less|css)$/,
      use: [
        {
          loader: MiniCssExtractPlugin.loader
        },
        {
          loader: 'css-loader',
          options: {
            sourceMap: true
          }
        },
        {
          loader: 'less-loader',
          options: {
            sourceMap: true
          }
        }
      ],
    ]
  }
  
}

module.exports = config;
```
以上配置，使用rules属性，里面包含两个必须属性：test和use。这告诉webpack编译器(compiler)如下信息：
> `webpack`编译器，当你碰到[ 在`require() / import` 语句中遇到`.css/.less` ] 时，你对它进行一次`use里面的`方法转换一下

需要注意的是，在webpack中配置loader时，需要在`module.rules`中，而不是`rules`。

#### 内联

可以在import语句或任何等效于"import"的方式中指定loader。使用`!`将资源中的loader分开。分开的每个部分都相当于当前目录解析。
`import Styles from 'style-loader!css-loader?modules!./styles.css';`
通过前置所有规则及使用`!`，可以覆盖到配置中的任意loader。

选项可以传递查询参数，例如`?key=value&foo=bar`,或者一个`JSON`对象，例如`?{"key":"value","foo":"bar"}`。

#### CLI
通过`CLI`使用`loader`：
`webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'`
这会对`.jade`文件使用`jade-loader`，对`.css`文件使用`css-loader`。

### loader特性

- `loader` 支持链式传递。能够对资源使用流水线(pipeline)。一组链式的loader将按照相反的顺序执行。loader链中的第一个loader返回值给下一个`loader`。在最后一个`loader`，返回`webpack`所预期的`JavaScript`。
- `loader` 可以是同步的，也可以是异步的。
- `loader` 运行在`Node.js`中，并且能够执行任何可能的操作。
- `loader` 接收查询参数。用于对`loader`传递配置。
- `loader` 也能够使用`options`对象进行配置。
- 除了使用`package.json`常见的`main`属性，还可以将普通`npm`模块导出为`loader`，做法时在`package.json`里定义一个`loader`字段。
- 插件(plugin)可以为`loade`r带来更多的特性。
- `loader` 能够产生额外的任意文件。
loader通过(loader)预处理函数，为JavaScript生态系统提供了更多的能力。用户现在可以更加灵活地引入细粒度逻辑，例如压缩、打包、语言翻译和[其他更多](https://www.webpackjs.com/loaders/)。

### 解析loader

`loader`遵循标准的模块解析。多数情况下，`loader`将从模块路径(通常将模块路径认为时`npm install`，`node_modules`)解析。

`loader`模块需要导出为一个函数，并且使用`Node.js`兼容的`JavaScript`编写。通常使用`npm`进行管理，但是也可以自定义`loader`作为应用程序的文件。按照约定，`loader`通常被命名为`xxx-loader`(例如`json-loader`)。详细信息，查看[如何编写loader?](https://www.webpackjs.com/contribute/writing-a-loader/)


## 插件plugins

`loader`被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用于处理各种各样的任务。

插件时`webpack`的支柱功能。`webpack`自身也是构建于，你在`webpack`配置中用到的相同的插件系统之上！插件目的在于解决`loader`无法实现的其他事。

### 剖析
`webpack`插件是一个具有`apply`属性的`JavaScript`对象。`apply`属性会被`webpack compiler`调用，并且`compiler`对象可以在整个编译生命周期访问。
ConsoleLogOnBuildWebpackPlugin.js
```
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuldWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, compilation => {
      console.log("webpack 构建过程开始!");
    });
  }
}
```
`compiler hook`的tap方法的第一个参数，应该是驼峰式命名的插件名称。建议为此使用一个常量，以便它可以在所有`hook`中复用。

### 用法

由于插件可以携带参数/选项，你必须在`webpack`配置中，向 `plugins`属性传入`new`实例。

当使用插件的时候，需要`require()`插件，然后把插件添加到`plugins`数组中。多数插件可以通过选项`option`自定义。也可以在一个配置文件中以不同的目的使用同一个插件，这时就需要new来创建它的一个实例。

#### 配置


```
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

module.exports = {
  module: {
    // loaders
    reules: [
      test: /\.(less|css)$/,
      use: [
        {
          loader: MiniCssExtractPlugin.loader
        },
        {
          loader: 'css-loader',
          options: {
            sourceMap: true
          }
        },
        {
          loader: 'less-loader',
          options: {
            sourceMap: true
          }
        }
      ],
    ]
  },

  // 入口
  entry: {
    app: './src/app.js',
  },

  // 出口
  output: {
    filename: '[name].[chunkhash].js',  // 文件名
    path: path.resolve(__dirname, 'dist'),  // 文件路径
  },

  // 插件
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
}

module.exports = config;
```

webpack提供了许多插件可以查看[插件列表](https://www.webpackjs.com/plugins/)获取信息。

### Node API

即使使用`Node API`，用户也应该在配置中传入`plugins`属性。`compiler.apply`并不是推荐的使用方式。

some-node-script.js
```
const webpack = require('webpack');
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration);
compiler.apply(new webpack.ProgressPlugin());

compiler.run(function(err, status) {
  // ...
})
```

## 模式mode
mode：string

通过选择`development`或`production`之中的一个，来设置`mode`参数，你可以启用相应模式下的`webpack`内置的优化。

### 用法
只在配置中提供mode选项：
```
module.exports = {
  mode: 'production'
}
```
或者从CLI参数中传递：
```
webpack --mode=production
```
支持以下字符串值：

| 选项        | 描述                                        |
|:-----------:| :-----------------------------------------:|
| development | 会将 process.env.NODE_ENV 的值设为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。 |
| production  | 会将 process.env.NODE_ENV 的值设为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 UglifyJsPlugin. |

**只设置NODE_ENV，则不hi自动设置mode**

#### mode: development
```
// webpack.development.config.js
module.exports = {
+ mode: 'development'
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}
```

#### mode: production
```
// webpack.production.config.js
module.exports = {
+  mode: 'production',
-  plugins: [
-    new UglifyJsPlugin(/* ... */),
-    new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
-    new webpack.optimize.ModuleConcatenationPlugin(),
-    new webpack.NoEmitOnErrorsPlugin()
-  ]
}
```