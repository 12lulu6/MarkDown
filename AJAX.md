# 基础
## 什么是AJAX?
AJAX 即 Asynchronous Javascript And XML（异步JavaScript和XML），是指一种创建交互式网页应用的网页开发技术。

## 渲染过程 
>浏览器通过地址访问服务器 然后服务器找到php文件 执行php  返回php结果给浏览器 浏览器再渲染页面

## PHP
php大多数使用函数来解决问题  .的作用是+ 拼接字符串

    1.在stardata.php中存储假数据 来模拟数据库 
    2.在index.html中利用表单元素 form来将数据传递到想要的位置 star.php 记得加name 传的是name值
    3.在star.php中利用$_GET["starname"] 超全局变量，来拿到传过来的name值 将stardata引用到star中
    根据这个值去stardata中查找相关数据 $star = $stararr[$name] 利用'<h2></h2>'等将页面布局好看
    4.利用服务器127.0.0.1中访问index.html

    如果是利用a标签来传递数据 要加上star.php?name =[value] 
    有点复杂  建议复习看22视频

    一个php文件中可以有多个<?php?>

## 关于get和post。
使用表单中的get提交方式
  --数据是拼接在url中  XXX.php?key=value1&key2=value2
  --不安全 在网址直接就能看到
  --数据太长 有些浏览器直接屏蔽
  --但是测试方便 在网址上直接更改数据能行
  --获取数据的方式使用$_GET
解决长度问题 就要使用post方式提交
  --提交的数据不在url
  --安全性好 因为数据不在网址上显示
  --没有长度限制 
  --浏览器可以传很多数据 但是服务器可以选择是否接收这么多的数据
  --上传文件要使用post
  --获取数据的方式使用$_POST

以上使用input方式或者a标签方式都是跳转页面来发送请求，如果要使用不跳转方式来发送请求，我们使用ajax。

# 使用AJAX
## 步骤：
- 创建异步对象
- 请求行（方式，url）
- 请求头（post才设置）
- 回调函数（接收返回的数据）
- 请求主体（发送请求，如果是post里面传参数key=value&key2=value2）

## 解释过程
商业网站里面有静态页面和动态页面都放在服务器中，当用户通过游览器去请求服务器的时候（请求报文），
如果请求的是静态页面，是什么就给你呈现什么，动态页面会获取用户发送的数据，去数据库中查询，给浏览器返回结果（响应报文）。

>状态改变事件 刚开始的时候使用的是onload()但是只支持火狐游览器 所以我们使用onreadstatechange()
相比之下会触发更多次 因为是在状态改变的时候就会触发(0:open()未被调用;1:send()未被调用;
2:send()被调用 获取相应头;3:响应体下载中 requestText获得部分数据;4:整个请求完成) 
所以判断在请求完成的时候(xhr.readState==4)来触发这个事件 
当请求的php不存在 但是还是会触发这个事件 所以还要判断这个请求状态是否未200(xhr.tatus==200)

使用form表单方式或者a标签传输数据都有一个特点是 点击后会跳转 因为会给你设置请求报文 和响应报文
  如果要不跳转就要使用ajax 就要自己去设置请求报文 和响应报文

  ### 什么是请求报文？
    存的游览器的一些信息和请求的方法，要将这些信息发到服务器
    --请求行（请求方式，请求地址）
    --请求头（浏览器的信息，接受的语言格式等浏览器信息 以及想要发送给服务器的信息）
    --请求主体（发送给服务器的数据 内容）
  ### 什么是响应报文？
    --状态行（请求是否成功，请求的状态）
    --相应头（服务器的一些信息，服务器想要告诉浏览器的信息）
    --响应主体（正常用户看到的内容） 
  ### 什么是HTTP协议?
    就是请求报文+响应报文
  
### XMLHttpRequest
通过XMLHttpRequest构造函数创建一个异步对象xmlhttp, IE6, IE5 使用ActiveXObject创建，创建的这个异步对象上有很多属性和方法，常用的有：

1.onreadystatechange 监听异步对象请求状态码readyState的改变，每当readyState改变时，就会触发onreadystatechange事件

2.readyState表示异步对象目前的状态，状态码从0到4：
   - 0: 表示请求未初始化，还没有调用 open()
   - 1: 服务器连接已建立，但是还没有调用 send()
   - 2: 请求已接收，正在处理中（通常现在可以从响应中获取内容头）
   - 3: 请求处理中，通常响应中已有部分数据可用了，没有全部完成
   - 4: 当readyState状态码为4时，表示请求已完成；此阶段确认全部数据都已经解析完毕，可以通过异步对象的属性获取对应数据
  
3.readyState请求状态码
  - 200 表示从客户端发来的请求在服务器端被正常处理了。
  - 204 表示请求处理成功，但没有资源返回。
  - 400 表示请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求。
  - 403 表示对请求资源的访问被服务器拒绝了
  - 404 表示服务器上无法找到请求的资源。除此之外，也可以在服务器端拒绝请求且不想说明理由时使用。

4.responseText：后台返回的字符串形式的响应数据

5.responseXML：后台返回的XML形式的响应数据

# 使用XML
 ## 为什么要使用xml?
  因为数据量大都传过来的字符串不好切 所以改为使用xml来传输数据

  ## 步骤：
  1.在html中对backxml.php发出请求
  2.在backxml.php中对xml文件进行解析为字符串，返回给浏览器
  3.在回调函数中通过xrl.responseText获取到这个字符串，xrl.responseXML获取到的是xml

  ## 缺点：
    --传输数据量大->但是现在网络号=好，不太重要
    --解析略微复杂
  
  所以引入了json
# JSON
1.json是一种数据格式
2.json跟编程语言无关
3.json的载体是字符串 只要不管哪种语言只是字符串都能使用json
1.  语言简洁 基本上所有的编程语言都提供相应的方法 来解析json
5.json格式的字符串解析完毕变为 数组 or 对象

json的写法---用来表示对象
对象使用{}包裹
属性名 属性值都要用""包裹 之间用：
var jsonObjact = '{"name":"张三","excel":"演电影"}'
这里的jsonObjact 是一个字符串,无法通过jsonObjact.name访问到张三，要转换为对应的对象或者数组
通过JSON.parse来转换
var obj = JSON.parse(jsonObjact);
这里的obj是一个对象


# 封装ajax
可以将代码写到js中 然后通过引入使用ajax({})函数
但是在jqurey中可以直接使用$.ajax({})  是一样的
$.get(url,{name:'黑猫警长',skill:'扎老鼠'},function(data){})
$.post(url,{name:'喜羊羊',skill:'聪明'},function(data){})
使用$.ajax({})如果省略type默认的是get 那么使用get的时使用ajax中type可以省略

# 使用JQurey中的AJAX
```
$(function(){
        $("input").first().click(function(){
            $.ajax({
                // 使用网络上的json api的接口
                url:"http://api.asilu.com/phone",
                data:{
                   phone:'18311564246'
                },
                type:'get',
                success:function(data){
                    console.log(data);
                },
                dataType:'jsonp'
            })
        })
    })
```
Promise版本的
```
const getJSON = function (url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url, false);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.onreadystatechange = function () {
      if (xhr.readyState !== 4) return;
      if (xhr.status === 200 || xhr.status === 304) {
        resolve(xhr.responseText);
      } else {
        reject(new Error(xhr.responseText));
      }
    };
    xhr.send();
  });
};
```
# 模板引擎
在GitHub里面搜索 art-template 下载js文件
在<script type="text/html></script>里面写入模板
type写为其他不会解析为js
<script type="text/html  id="template">
<ul>
   <li>名字{{name}}</li>
   <li>技能{{skill}}</li>
   <li>朋友{{friend}}</li>
</ul>
</script>
以上的内容不会在页面中显示
{{name}} 这是挖坑 给坑取名字 根据数据来取

然后再写script
<script>
var data={
  name:"黑猫警长",
  skill:"抓老鼠",
  friend:"robot"
};
下面是填坑 将数据填入相对性的坑中
template();
参数一：是模板的id
参数二：是填充的数据
var result = template('template',data);
console.log(result);
document.body.html(result);
</script>


这里将数据更改更为方便 再更进一步 使用ajax来取数据 在回调函数中填坑

使用模板引擎的场所是 长的差不多但是里面的数据不一样 不如逛淘宝时只是商品的图片 名字 信息不同 
但是模板是差不多的  就是数据很多需要被填充的时候 一条一条的很麻烦

查看ajax下部的最后一天的7

05
瀑布流 一种布局模式
在GitHub里面搜索masonry 下载瀑布流
导入
按照使用

综合：
使用不跳转页面更新页面 使用ajax
--首先其他的弄好(例如点击事件 定时器等)
--使用ajax
--当数据格式相差不多 使用模板引擎
  *引入模板引擎
  *根据数据来定义模板 
  *挖坑
  *填坑  在ajax中的回调函数中

要实现点一下追加瀑布流 点一下在来
首先要先看看在github里面的案例 根据案例去找方法
然后运用 结果没用上 看看是不是字符串 元素等转化问题

# 跨域问题
同源：协议(HTTP协议://) 地址 端口号(就是我写的在同一个服务器中的)都相同叫做同源
不同源： 协议 地址 端口号 有一个不同
跨域：就是两个不同源之间相互发送请求
如何解决跨域
 ## cors
 cross origin resouse sharing  但是这个 有兼容性 需要h5才支持
   解释：在请求头中添加这个 在php中添加 header('Access-Control-Allow-Origin:*')
 # jsonp
 dom元素中的src是支持跨域来获取资源的 例如：img a 等 还有csript 
        其中jsonp就是利用的是script的src 来跨域获取资源

jquery中动态创建一个script 
与ajax没有关系 jquery利用script中的src来发送 ajax利用XMLhttprequest发送
只能发get请求 不能发post
