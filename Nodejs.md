#前言
## 什么是nodejs?
是js的运行时环境 可以解析和执行js代码(运行在服务端的 JavaScript)
  以前只有游览器可以解析js代码现在能...
Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。
就是客户端向服务器请求数据，服务器从数据库中通过IO得到数据，服务器能响应给客户端。在这个过程中，IO是硬件设备，已经无法改变的慢。如果将数据库比作为厨师，刚开始的时候我们工作流程是每来一个人就给你配一个服务员(多进程),能提高效率，但是厨师慢，所有人都在那里等着厨师做好，造成了堵塞。但是我们使用nodejs相当于只雇佣一个超级服务员(单线程)，能提高性能(V8)。虽然我们nodejs是单线程，但是我可以创建多个服务器呀，因为我对服务器要求不高。

## Nodejs的组成？
引入 required 模块：我们可以使用 require 指令来载入 Node.js 模块。
创建服务器：服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。
接收请求与响应请求 服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

## 什么是REPL？
Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。
Node 自带了交互式解释器，可以执行以下任务：
    读取 - 读取用户输入，解析输入的 Javascript 数据结构并存储在内存中。
    执行 - 执行输入的数据结构
    打印 - 输出结果
    循环 - 循环操作以上步骤直到用户两次按下 ctrl-c 按钮退出。

### 1.使用REPL
  - 1.打开终端
  - 2.输入node
  - 3.开始使用

### 2.REPL语法
  - 能加减乘除
  - 能使用变量  var y = 10
  - 变量能用console.log()输出 
  - 能使用循环
  - 可以使用下划线(_)获取上   表达式的运算结果

### 3.REPL命令
ctrl + c - 退出当前终端。
ctrl + c 按下两次 - 退出 Node REPL。
ctrl + d - 退出 Node REPL.
向上/向下 键 - 查看输入的历史命令
tab 键 - 列出当前命令
help - 列出使用命令
break - 退出多行表达
clear - 退出多行表达式
save filename - 保存当前的 Node REPL 会话到指定文件
load filename - 载入当前 Node REPL 会话的文件内容。

## 什么是NPM？
伴随这Nodejs下载的一个包管理工具。
- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

# 模块
我们知道js包括ECMASScript(变量,判断,循环),DOM,BOM
还有在**nodejs中只能使用ECMAS**
仅仅使用js的一些语法还不够 还需要些模块
- 内置/核心模块：http服务,fs文件操作,url路径,path路径处理,os操作系统等
- 第三方模块
- 自定义模块：自己创建的js文件
   * 一个文件就是一个模块
   * 通过这种方式我们能同时打开多个js(虽然是命令式，一次只能打开一个.js，但是我们能使用这种方式来打开多个.js)
   * 通过exports和modul.exports来声明模块中的哪些功能可以使用
      exports.属性/方法名 = 功能
      modul.exports.属性/方法名 = 变量名
   * 通过require来加载模块( require从内存中获取数据并赋值给新的变量)
      var 对象 = require('路径不加.js');
      对象.属性/方法名;
    >>解释：就是你弄了十个模块 但是我不能都能使用这十个模块 我得先通过exports和modul.exports来声明哪些我是可以用的才能使用 最后通过require来加载

## 核心模块
### path模块
path模块提供了用于处理文件和目录的路径的实用工具。
引入：`const path = require('path');`
使用：
- 方法1：path.dirname() 
  返回 path 的目录名，类似于 Unix dirname 命令。 尾随的目录分隔符被忽略，见 path.sep。
  ```
  path.dirname('/foo/bar/baz/asdf/quux');
  // 返回: '/foo/bar/baz/asdf'
  ```
- 方法2：path.parse() 
  返回一个对象，其属性表示 path 的重要元素。
  ```
    path.parse('C:\\path\\dir\\file.txt');
     返回:
     { root: 'C:\\',
       dir: 'C:\\path\\dir',
       base: 'file.txt',
       ext: '.txt',
       name: 'file'
       }
  ```
- 方法3：path.relative() 
  根据当前工作目录返回从 from 到 to 的相对路径。
  ```
    path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb');
     返回: '..\\..\\impl\\bbb'
  ```
- 方法4：path.resolve() 
  将路径或路径片段的序列解析为绝对路径。
  ```
    path.resolve('/foo/bar', './baz');
     返回: '/foo/bar/baz'

    path.resolve('/foo/bar', '/tmp/file/');
     返回: '/tmp/file'

    path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
     如果当前工作目录是 /home/myself/node，
     则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
  ```
  ...
### url模块
url 模块提供用于网址处理和解析的实用工具。
引入: `import url from 'url';`
使用：
- 方法1：url.hash
  获取和设置网址的片段部分。
  ```
    const myURL = new URL('https://example.org/foo#bar');
    console.log(myURL.hash);
     打印 #bar

    myURL.hash = 'baz';
    console.log(myURL.href);
     打印 https://example.org/foo#baz
  ```
- 方法2：url.hostname
  获取和设置网址的主机名部分
  ```
    const myURL = new URL('https://example.org:81/foo');
    console.log(myURL.hostname);
    // 打印 example.org
  ```
- 方法3：url.href
  获取和设置序列化的网址。  
  ```
    const myURL = new URL('https://example.org/foo');
    console.log(myURL.href);
    // 打印 https://example.org/foo

    myURL.href = 'https://example.com/bar';
    console.log(myURL.href);
    // 打印 https://example.com/bar
  ```
...

### fs模块
能通过.txt来创建.js
#### 1.写入模块
```
//1.创建fs对象（引入node内置的fs模块）
var fs = require('fs');

//2.调用函数写数据进文件
fs.writeFile('./a.txt', '你好，传智', function(err) {
    // err  为null - 则写入成功
    // err不为null - 则写入失败
if (err) {
    	console.log(err);
    	return;
    }
    console.log('写入成功');
});

```
#### 2.读取文件
```
//1.引入fs模块
var fs = require('fs')
//2.练习：读取a.txt内容
fs.readFile('a.txt', function(err, data){
	if (err) {
		console.log(err)
		return;
	}
	console.log(data)  //脚下留心：默认不是我们可识别的内容
					   //Buffer对象 要转为我们认识的data.toString()
  console.log(data.toString()) 
//3.或者直接这样
fs.readFile('a.txt', 'utf8', function(err, data){
	if (err) {
		console.log(err)
		return;
	}
	console.log(data)  //加了可选参数，不需要转换直接输出
})
```
### http模块(重点)
要使用 HTTP 服务器和客户端，则必须 require('http')。
为了支持所有可能的 HTTP 应用程序，Node.js HTTP API 是非常低层的。 它只进行流处理和消息解析。 它将消息解析为标头和正文，但不解析实际的标头或正文。
#### 基本发请求
```
//1.引入模块
var http = require('http')
//2.创建web服务器
var server = http.createServer()
// 3. 服务器要干嘛？
//    提供服务：对 数据的服务
//    发请求
//    接收请求
//    处理请求
//    给个反馈（发送响应）
//    注册 request 请求事件
//    当客户端请求过来，就会自动触发服务器的 request 请求事件，然后执行第二个参数：回调处理函数
//3.监听请求
server.on('request', function(req, res) {  //req ///  Request 请求对象
//  请求对象可以用来获取客户端的一些请求信息，例如请求路径
//  Response 响应对象
//  响应对象可以用来给客户端发送响应消息
	console.log('收到用户请求,请求地址：'+req.url)

// response 对象有一个方法：write 可以用来给客户端发送响应数据,将这个hello，itcast数据发给了客户端
// write 可以使用多次，但是最后一定要使用 end 来结束响应，否则客户端会一直等待
res.write('hello，itcast')
// 告诉客户端，我的话说完了，你可以呈递给用户了
res.end();


// 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
//响应内容只能是二进制数据或者字符串
  // res.end('hello nodejs')

})
//4.启动服务
server.listen(8080, function(){
	console.log('服务启动成功，访问：http://localhost:8080')
})
```
解释：就是把你整一个监听器server，server为request注册了一个事件监听（绑定事件函数server.on）。当你这个request事件被触发，就执行on里面那个回调函数，就能获取到request事件的数据.那么咋触发request呢？就是你监听器开启监听server.listen，当服务器发过来，就能触发这个request
#### 进阶
```
//1.引入模块
var http = require('http')
//2.创建web服务器
var server = http.createServer()
//3.监听请求
server.on('request', function(req, res) {  //req - request 请求  res - response 响应
	console.log('收到用户请求,请求地址：'+req.url)

console.log('收到请求了，请求路径是：' + req.url)
  console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)
//解释ip地址和端口号 我给服务器发请求，要带着我的ip地址(我是哪台电脑)和端口号(我要去哪个应用程序)

	 // 根据不同的请求路径发送不同的响应结果
  // 1. 获取请求路径
  //    req.url 获取到的是端口号之后的那一部分路径
  //    也就是说所有的 url 都是以 / 开头的
  // 2. 判断路径处理响应
	if (req.url == '/') {
		$msg = 'this is index'
	} else if (req.url == '/login') {
		$msg = 'this is login'
	} else {
		$msg = '404'
	}

  // res.end('hello 世界')
	//留心：有请求就必须有响应，没有响应网页无法打开
	res.setHeader('Content-Type', 'text/html;charset=utf-8');
	// res.write("哥哥来抓我呀，<a href='http://nn.com'>点击进入我的世界</a>");
	res.write($msg);
	res.end();
})
//4.启动服务
server.listen(8080, function(){
	console.log('服务启动成功，访问：http://localhost:8080')
})
```
里面能判断请求回来数据的地址了

#### 进阶升级版
```
//1.引入模块
var http = require('http')
//2.创建web服务器
var server = http.createServer()
//3.监听请求
server.on('request', function(req, res) { 
  // 在服务端默认发送的数据，其实是 utf8 编码的内容
  // 但是浏览器不知道你是 utf8 编码的内容
  // 浏览器在不知道服务器响应内容的编码的情况下会按照当前操作系统的默认编码去解析
  // 中文操作系统默认是 gbk
  // 解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的
  // 在 http 协议中，Content-Type 就是用来告知对方我给你发送的数据内容是什么类型
  // res.setHeader('Content-Type', 'text/plain; charset=utf-8')

	// res.statusCode = 404;
	// res.statusMessage = 'Not Found';
	res.writeHeader(404, 'Not Found2', {
		'Content-Type' : 'text/html; charset=utf8'
	})
	// res.write("哥哥来抓我呀，<a href='http://nn.com'>点击进入我的世界</a>");
	res.write('打印request对象');
	res.end();
})
//4.启动服务
server.listen(8080, function(){
	console.log('服务启动成功，访问：http://localhost:8080')
})
```
对请求回来的数据进行请求头的解析

**来一个完整的，好好理解下**
当我数据请求回来将一个页面响应给客户端
```
// 1. 结合 fs 发送文件中的数据
// 2. Content-Type

var http = require('http')
var fs = require('fs')

var server = http.createServer()

server.on('request', function (req, res) {
  // / index.html
  var url = req.url

  if (url === '/') {
    // 肯定不这么干,这样干太蠢了
    // res.end('<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><title>Document</title></head><body><h1>首页</h1></body>/html>')

    // 我们要发送的还是在文件中的内容
    fs.readFile('我有这个index.html', function (err, data) {
      if (err) {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      } else {
        // data 默认是二进制数据，可以通过 .toString 转为咱们能识别的字符串
        // res.end() 支持两种数据类型，一种是二进制，一种是字符串
        res.setHeader('Content-Type', 'text/html; charset=utf-8')
        res.end(data)
      }
    })
  } else if (url === '/xiaoming') {
    // url：统一资源定位符
    // 一个 url 最终其实是要对应到一个资源的
    fs.readFile('./resource/ab2.jpg', function (err, data) {
      if (err) {
        res.setHeader('Content-Type', 'text/plain; charset=utf-8')
        res.end('文件读取失败，请稍后重试！')
      } else {
        // data 默认是二进制数据，可以通过 .toString 转为咱们能识别的字符串
        // res.end() 支持两种数据类型，一种是二进制，一种是字符串
        // 图片就不需要指定编码了，因为我们常说的编码一般指的是：字符编码
        res.setHeader('Content-Type', 'image/jpeg')
        res.end(data)
      }
    })
  }
})

server.listen(3000, function () {
  console.log('Server is running...')
})

```
以上的第三方模块就介绍到这里
下面使用nodejs中的框架express.加快项目开发，便于团队协作
#   Express
解释：类似于内置http模块，专门用来创建web服务器，是一个第三方模块。核心就是中间件，就是一系列中间件的调用。
## 执行流程
> 当一个请求到达express服务器之后，可以连续调用多个中间件，从而对这次请求进行预处理。
## 中间件
什么是中间件？
>- 就是传递给express的一个回调函数，有三个参数(req,res，next).
>- 它最大的特点就是，一个中间件处理完，再传递给下一个中间件。App实例在运行过程中，会调用一系列的中间件。
>- 每个中间件都可以对HTTP请求（request对象）进行加工，并且决定是否调用next方法，将request对象再传给下一个中间件。
> - 多个中间件共享同一份req和res。基于这样的特性，我们可以在上游的中间件中，统一为req或 res对象添加自定义的属性或方法，供下游的中间件或路由进行使用。

我的理解就是创建web服务器来监听请求，当请求过来会调用多个中间件，分为全局生效的中间件和局部生效的中间件.
#### 全局生效的中间件
客户端发起的任何请求，到达服务器之后，都会触发的中间件
```
const mw = (req, res, next) => {
    ...
    next()
}

const mw1 = (req, res, next) => {
    ...
    next()
}

// 全局生效的中间件,中间件调用顺序以传入顺序为准
// 通过调用server.use(中间件函数)，即可定义一个全局生效的中间件
app.use(mw,mw1)
```
#### 局部生效的中间件
```
const mw = (req, res, next) => {
    ...
    next()
}
const mw1 = (req, res, next) => {
    ...
    next()
}
// 局部生效的中间件
app.get('/',mw,(req,res)=>{
    res.send('路径：/')
})

// 定义多个局部生效的中间件
// 1、直接逗号分隔
app.get('/',mw,mw1,(req,res)=>{
    res.send('路径：/')
})
// 2、或者使用数组包含
app.get('/',[mw,mw1],(req,res)=>{
    res.send('路径：/')
})
```
## express执行
- 创建目录
- 在目录中下载express
  >npm init -y
  npm install express
- 使用express
  ```
  1.导入express模块
    var express = require('express');
  2.创建web服务器
    var app = express();
  // 监听get或者post请求，并响应数据
    app.get('/',function(req,res，next){
      参数一：客户端请求的URL地址
      参数二：请求对应的处理函数 中间件函数
      参数三：用于执行下一个中间件的函数
           req:请求对象（包含与请求相关的属性和方法）
           res:响应对象（包含与响应相关的属性和方法）
               使用end() 响应字符串（会乱码）
               使用send() 响应字符串（自动识别）将处理好的内容发送给客户端
               render() 响应字符串 （自动识别，只能打开指定文件字符串并响应 ）
              res.send("<a href='#'>点击</a>");
  })
  
  3.启动web服务器
  app.listen(8080,function(){
      console.log("启动成功，访问：http://localhost:8080");
  })
  ```
  ## 修改完代码自动重启
  使用一个第三方命令行工具：nodemon
  - 下载
     `npm i --glabal nodemon`
  - 使用
     `nodemon app.js`
  ## 使用模板引擎 
- 下载 
  > npm install art-template
npm install express-art-template
- 使用在项目中创建view下面创建index.html写入模板引擎
  ```
  var tplStr = `
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Document</title>
  </head>
  <body>
    <p>大家好，我叫：{{ name }}</p>
    <p>我今年 {{ age }} 岁了</p>
    <h1>我来自 {{ province }}</h1>
    <p>我喜欢：{{each hobbies}} {{ $value }} {{/each}}</p>
  </body>
  </html>
  ```
  在项目中使用模板引擎
  ```
  //1.引入express框架模块
  var express = require('express')
  //2.创建框架核心app对象
  var app = express()
  //###声明所使用的模板引擎（ps. 使用render方法必须）
  app.engine('html', require('express-art-template'))
  //3.路由
  app.get('/',  function(req, res) {

  	//语法：res.render(模板文件, {数组})
  	//练习
  	res.render('test1.html', {
  		username: '传智播客',
  		age: 5,
  		orders: [
  			{id:1, title: '标题1', price: 30},
  			{id:2, title: '标题2', price: 33},
  			{id:3, title: '标题3', price: 12},
  		], 
  	})

  })
  //4.启动服务
  app.listen(8080, function(){
  	console.log('Running...')
  })

  ```

## 使用路由
### 语法
- 普通语法：app.HTTP请求类型（请求路径，回调函数）
  - 发送  GET请求：app.get（请求路径，回调函数）
  - 发送POST请求：app.post（请求路径，回调函数）
  - 发送  任意请求：app.all（请求路径，回调函数）
....
在路由模块中
```
// 1、导入express模块
const express = require('express')
// 2、创建路由对象
const router = express.Router()
// 3、挂载具体的路由
router.get('/user/list', (req, res) => {
    res.send('Get User List')
})
router.post('/user/add', (req, res) => {
    res.send('Add new user')
})

// 4、向外导出路由
module.exports = router

```
在使用路由模块的模块中
```
const express = require('express')
const server = express()

// 1、导入路由模块
const router = require('./router/router')
// 2、注册路由模块
server.use(router)

server.listen(80, () => {
    console.log('server running at http://127.0.0.1');
})
```

### 静态资源处理
express 提供了一个非常好用的函数，叫做 express.static()，通过它，我们可以非常方便地创建一个静态资源服务器，例如，通过如下代码就可以将 public 目录下的图片、CSS 文件、JavaScript文件对外开放访问了：
`app.use(express.static('./public/'))`
现在，你就可以访问 public 目录中的所有文件了

### 挂载路径前缀
如果希望在托管的静态资源访问路径之前，挂载路径前缀，则可以使用如下的方式：
`app.use('/public/', express.static('./public/'))`
现在，你就可以通过带有 /public 前缀地址来访问 public 目录中的文件了

# MVVM
什么是MVVM？MVVM是Model-View-ViewModel的缩写。
解释：就是当我们使用node.js有了一整套后端开发模型之后，我们就能尝试使用js在浏览器中操作html。刚开始的时候使用的是js源码撸，后来jqurey的出现解决了兼容性等问题，第三阶段使用的是MVC模式，配合服务器使用，但是随着交互性的增加，变得复杂，出现MVVM模型（vue等）不关心DOM的结构，只关心数据的存储








