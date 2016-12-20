## webpack介绍
- Webpack是一款用户打包前端模块的工具。主要是用来打包在浏览器端使用的javascript
- 同时也能转换、捆绑、打包其他的静态资源，包括css、image、font file、template等
- 外可以通过自己开发loader和plugin来满足自己的需求

### webpack安装
- 在安装之前，需要先初始化一个模块的配置文件package.json，可通过如下命令创建

```
    $ npm init -y
```
- webpack的安装

```
    $ npm install webpack --save-dev
```
- 创建webpack的配置的文件```webpack.config.js```
- 一个简单的配置文件如下：

```
    var webpack = require('webpack');
    var path = require('path');
    
    var config = {
        //入口文件配置
        entry:path.resolve(__dirname,'src/main.js')，
        //文件导出的配置
        output:{
            path:buildPath,
            filename:"app.js"
        }
    }

    module.exports = config;
```
---

### ES6语法新特性解析
- 这里我们解析ES6语法使用的是babe转译器，安装如下：

```
    npm install babel-loader babel-core babel-preset-es2015 --save-dev
```
- 安装之后需要创建.babelrc配置文件，并进行配置

```
    {
        "presets":["es2015"],
        "plugins":[]
    }
```
- 对webpack.config.js进行配置，使其在打包js文件时使用babel进行转译，配置如下：

```
    {test: /\.js$/, loader: 'babel',exclude: /node_modules/}
```
### react解析与打包
- 首先需要下载react所需的包

```
    $ npm install react react-dom --save-dev
```
- 下载可以对react进行转译的预设

```
    $ npm install babel-preset-react --save-dev
```
- 对.babelrc进行配置，如下：

```
    {
        "presets":["es2015","react"],
        "plugins":[]
    }
```
- 注：默认情况下，React 将会在开发模式，很缓慢，不建议用于生产。要在生产模式下使用 React，设置环境变量 NODE_ENV 为 production （使用 envify 或者 webpack's DefinePlugin）,DefinePlugin是webpack的默认插件

```
    new webpack.DefinePlugin({
      "process.env": {
        NODE_ENV: JSON.stringify("production")
      }
    });
```
---
### 对css及less文件的打包
- 由于webpack本身只处理js模块，如需要对其它模块进行处理，需要使用loader进行转换
- css及less模块的loader下载,注：下载less-loader之前需要先安装less

```
    $ npm install css-loader style-loader --save-dev
    $ npm install less-loader --save-dev
```
- 对webpack.config.js进行配置

```
    {test: /\.css$/, loader: 'style!css',exclude: /node_modules/},
    {test: /\.less$/, loader: 'style!css!less',exclude: /node_modules/}
```
### webpack分离css单独打包
- webpack 把所有的资源都当成了一个模块, CSS,Image, JS 字体文件 都是资源, 都可以打包到一个 bundle.js 文件中.但是有时候需要把样式 单独的打包成一个文件, 然后放到 CND上, 然后缓存到浏览器客户端中
- extract-text-webpack-plugin插件安装

```
    npm install extract-text-webpack-plugin --save-dev
```
- 添加配置文件
    
```
    var ExtractTextPlugin = require("extract-text-webpack-plugin");
```
- 在plugins中添加对应插件

```
    new ExtractTextPlugin("styles.css"),
```
- modules里面对css的处理修改为如下：

```
    {
        test: /\.css$/,
        loader:  ExtractTextPlugin.extract("style-loader","css-loader")
    }
```
---

### 打包文件压缩--生产环境
```
    new webpack.optimize.UglifyJsPlugin({     // 压缩打包文件，这个是WebPack内建插件
        compress: {
            //supresses warnings, usually from module minification
            warnings: false
        }
    })
```
---
### 启动本地服务器
- webpack-dev-server插件安装

```
    $ npm install webpack-dev-server --save-dev
```
- 在webpack的配置文件中对devServer进行参数配置，如下所示：

```
    devServer: {
        historyApiFallback: true,
        hot: false,
        inline: true,
        grogress: true
    }
```
- 注：运行dev，即webpack-dev-server是通过webpack-dev-server起服务，不会产出实际的文件，都是通过node服务在内存中运行的，而执行webpack命令会在电脑上产出实际的文件

### 自动生成html文件
- html-webpack-plugin 插件安装

```
    $ npm install html-webpack-plugin --save-dev
```
- 在webpack配置文件中引入该插件并进行相关配置

```
    var HtmlWebpackPlugin = require('html-webpack-plugin');
    new HtmlWebpackPlugin({
            title:'搭建前端工作流',   // 用于生成的html文件标题
            template:'./src/index.html'    // 模板路径，这里的模板路径是src下面的index.html
        }),
```
---
### 打包完成后自动打开浏览器--开发环境
- open-browser-webpack-plugin插件下载

```
    npm install open-browser-webpack-plugin --save-dev
```
- 在webpack配置文件中对该插件进行配置

```
    var openBrowserPlugin = require('open-browser-webpack-plugin')
    
    new openBrowserPlugin({
        url: 'http://localhost:8080'
    })
```
---
### 开发调试
- 在webpack文件中添加如下配置项：

```
    "devtool":"cheap-source-map",   //告诉JS引擎原始文件位置,具体还不太清楚。。。
    resolve:{  // 在import的时候可以不需要每次都加上后缀以及目录名
        extension:['','js','css','json'],
        root:path.resolve('./src')
    }
```
---
### 环境变量（用于区分开发环境与生产环境） yargs
- yargs模块安装

```
    $ npm install yargs --save-dev
```    
- 在webpack配置文件中引入该模块

```
    var mode = require('yargs').argv.mode;
```
- 在package.json中添加命令时可以指定当前是开发环境还是生产环境，如下：

```
    "dev": "webpack --progress --colors --watch --mode=development",
    "build":"webpack --mode=production"
```
- 对于不同环境的配置做优化处理，这里是对生产环境的代码进行压缩处理，如下所示：

```
    在webpack配置文件中添加如下内容：
    var uglifyPlugin = webpack.optimize.UglifyJsPlugin;
    
    
    var plugins = [];
    var filename = '';
    console.log(mode);
    // 生产环境
    if(mode == 'production'){
        console.log('11111');
        plugins.push(new uglifyPlugin({minimize:true}));
        // sparrow.min.js
        filename = libraryName+'.min.js';
    }
    // 开发环境
    else{
        //sparrow.js
        filename = libraryName + '.js';
    }
    
    并将最新的filename和plugins添加到配置中去   
```
---
### ESlint--开发环境
- EsLint帮助我们检查Javascript编程时的语法错误。比如：在Javascript应用中，你很难找到你漏泄的变量或者方法。EsLint能够帮助我们分析JS代码，找到bug并确保一定程度的JS语法书写的正确性
- ESlint安装

```
    npm install eslint --save-dev
```
- 在安装eslint安装之后可以通过```eslint --init```初始化一个eslint的配置文件
- 由于我们使用了webpack，所以需要告诉webpack在构建时使用eslint，需要安装eslint-loader

```
    npm install eslint-loader --save-dev
```    
- 安装之后，可以在npm script添加一个检查的命令，例如检测当前目录下的src下的文件，可添加如下内容：

```
    "lint": "eslint ./src"
```
-  或者不写npm script 在加载js文件时，加载器如下：

```
    {test:/\.js$/,loader:'eslint',exclude:/node_modules/}
```
- 如果希望在检测时并修复一些简单的问题，可以增加--fix参数



























