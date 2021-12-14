#前言
## 什么是nodejs?
是js的运行时环境 可以解析和执行js代码(运行在服务端的 JavaScript)
  以前只有游览器可以解析js代码现在能...

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

3.REPL命令
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





