# 前言
## 是什么？
Webpack是前端资源构建工具，是一个静态打包工具。
就是我们在使用less/jqury/xxx等当渲染到页面的时候，浏览器是不能识别的，我们需要借助各种工具，将less转化为css，将jqury转换为js等，比如将less转化为css我们可以使用less-css插件，但是这么多，我们引入了构建（大的，里面包括了各种转化工具）Webpack。

## 过程
在使用webpack的时候，我们会指定一个入口文件，里面有这个项目的各种引入，webpack会扫描入口文件将这个依赖引入形成一个chunk块，在将这个chunk进行各种处理(less->css,ES6->ES5,,jq->js等)称为打包，打包好输出形成bundle。

# webpack五个核心概念
* entry(入口)，给webpack指定一个入口起点开始打包，分析构建依赖图。
* output(出口)，就是bundle输出去哪里？如何命名？
* Loader,可以理解为webpack能识别js，json，不能识别其他，使用Loader来让less img ES6处理
* plugins(插件)，打包优化，压缩等
* mode(模式)，分为development(开发模式):能让代码本地调试运行的环境和production(生产模式):代码优化上线运行的环境
  
# 使用webpack
## 1.在npm安装 在无中文目录安装 
    npm init -y 初始化node
    npm install webpack@3.6.0 -g
    一般情况能在package.json中的dependencies 但是这个没看见 使用webpack -version 查看是否安装成功

## 2.创建目录(src build)
指定一个入口文件index.js。
在终端中使用webpack打包 运行指令: webpack ./src/main.js ./dist/bundle.js -mode=development/production
    意思是将./src/main.js打包到./dist/bundle.js 这样会在dist中自动生成bundle.js,整体打包环境为development/production
    最后在index.html中引用这个bundle.js包 <script src="./dist/bundle.js"></script>或者使用nodejs来启动js文件
- development(未压缩)/production(压缩)：能将ES6转化为浏览器能够识别的模块化
***   
但是修改数据每次都要执行webpack ./src/main.js ./dist/bundle.js 查看入口出口，太麻烦 我们直接在index.html同目录创建一个webpack.config.js来说明入口和出口
  将入口和出口放在一起 就不用执行webpack ./src/main.js ./dist/bundle.js 直接写webpack
## 3.webpack.config.js配置文件
提示webpack该干哪些活？(当运行webpack的时候就会加载里面的配置)
```
const path = require('path');
module.exports = {
  entry:'./src/main.js',
  output:{
    //输出文件名
    filename:'bundle.js'
    //输出路径
    path:path.resolve(__dirname,'dist'),
  },
  //loader的配置
  module:{
    rules:[
      ...
    ]
  },
  plugins:[
    ...
  ],
  mode:'development'
```
**在webpack.config.js中的module中写入在官网中相应的。。。**
### loader
1.下载相应的xxx
2.配置使用
- 引入图片资源
  - 下载npm install --save-dev url-loader file-loader
  - 使用
```
   module:{
    rules:[
      {
        //除了使用text还可以使用exclude将...排除在外
        text:/\.(jpg|img|gif)$/,
        //使用一个loader
        loader:'url-loader',
        option:{
          limit:8 * 1024，
          name:'[hash:10].[ext]'
        }
      }
    ]
  },
```
### plugins
####  HtmlWebpackPlugin
  - 下载npm install --save-dev html-webpack-plugin
  * 引入`var HtmlWebpackPlugin = require('html-webpack-plugin');`
  * 配置`plugins: [new HtmlWebpackPlugin()]`
  * 结果为：在打包中生成一个新的index.huml，将src中的index.html复制到dist里这个新的index.html中---`plugins: [new HtmlWebpackPlugin({templete:'./src/index.html'})]` 
#### UglifyjsWebpackPlugin
  * 下载npm i -D uglifyjs-webpack-plugin
  * 引入`const UglifyJsPlugin = require('uglifyjs-webpack-plugin')`
  * 配置使用`plugins: new UglifyJsPlugin()]`
#### MiniCssExtractPlugin
  打包生成css文件夹
  * 下载npm i -D mini-css-extract-plugin
  * 引入`const MiniCssExtractPlugin = require('mini-css-extract-plugin');`
  * 配置
      ```
        module.exports = {
      entry: './src/js/index.js',
      output: {
        filename: 'js/built.js',
        path: resolve(__dirname, 'build')
      },
      module: {
        rules: [
          {
            test: /\.css$/,
            use: [
              // 创建style标签，将样式放入
              // 'style-loader', 
              // 这个loader取代style-loader。作用：提取js中的css成单独文件
              MiniCssExtractPlugin.loader,
              // 将css文件整合到js文件中
              'css-loader'
            ]
          }
        ]
      },
      plugins: [
        new HtmlWebpackPlugin({
          template: './src/index.html'
        }),
        new MiniCssExtractPlugin({
          // 对输出的css文件进行重命名
          filename: 'css/built.css'
        })
      ],
      mode: 'development'
    };
      ```
  
#### CssMinimizerWebpackPlugin
  优化和压缩 CSS。
  * 下载 npm install css-minimizer-webpack-plugin --save-dev
  * 使用
      ```
        const MiniCssExtractPlugin = require("mini-css-extract-plugin");
        const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
        module.exports = {
          module: {
            rules: [
              {
                test: /.s?css$/,
                use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
              },
            ],
          },
          optimization: {
            minimizer: [
              // 在 webpack@5 中，你可以使用 `...` 语法来扩展现有的 minimizer（即 `terser-webpack-plugin`），将下一行取消注释
              // `...`,
              new CssMinimizerPlugin(),
            ],
          },
          plugins: [new MiniCssExtractPlugin()],
        };
      ```
#### EslintWebpackPlugin
该插件使用eslint来查找和修复 JavaScript 代码中的问题。
 * 下载npm install eslint-webpack-plugin --save-dev
 * 
...
***
每次都要重新webpack,可以选择搭建一个服务器,就能实时监听我们的代码改变。先放在内存里面，你不断测试，当真正发布的时候才到磁盘上(dist文件夹)。使用的是express框架
## devserver
- 下载npm i webpack-dev-server -D
- 在webpack.config.js中devServer:{contentBase:'.dist'(只能服务文件夹),inline:true(是否需要实时监听)}
```
/*
  开发环境配置：能让代码运行
    运行项目指令：
      webpack 会将打包结果输出出去
      npx webpack-dev-server 只会在内存中编译打包，没有输出
*/
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      // loader的配置
      {
        // 处理less资源
        test: /\.less$/,
        use: ['style-loader', 'css-loader', 'less-loader']
      },
      {
        // 处理css资源
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
      {
        // 处理图片资源
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10].[ext]',
          // 关闭es6模块化
          esModule: false,
          outputPath: 'imgs'
        }
      },
      {
        // 处理html中img资源
        test: /\.html$/,
        loader: 'html-loader'
      },
      {
        // 处理其他资源
        exclude: /\.(html|js|css|less|jpg|png|gif)/,
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          outputPath: 'media'
        }
      }
    ]
  },
  plugins: [
    // plugins的配置
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  mode: 'development',
  devServer: {
    contentBase: resolve(__dirname, 'build'),
    compress: true,
    port: 3000,
    open: true
  }
};
```




