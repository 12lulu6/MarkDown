#1.定义组件
## 1.1函数式组件
```
	function MyComponent(){
			console.log(this); //此处的this是undefined，因为babel编译后开启了严格模式
			return <h2>我是用函数定义的组件</h2>
		}
		ReactDOM.render(<MyComponent/>,document.getElementById('test'))
 ```
## 1.2类式组件
```
  class MyComponent extends React.Component{
			render(){
				//render是放在哪里的？—— MyComponent的原型对象上，供实例使用。
				//render中的this是谁？—— MyComponent的实例对象 <=> MyComponent组件实例对象。
				console.log('render中的this:',this);
				return <h2>我是用类定义的组件</h2>
			}
		}
		ReactDOM.render(<MyComponent/>,document.getElementById('test'))

```
## 注意
- 类式组件没有state，
- 引入组件要使用大写
- 组件要大写
- 执行类式组件过程：
1.React解析组件标签，找到了MyComponent组件。
2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
- 执行函数式组件过程：
执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
1.React解析组件标签，找到了MyComponent组件。
2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。
		
# 2.组件三大属性
## 2.1state
```
 class Weather extends React.Component{
  state = {isHot:true}
  render(){
    const {isHot} = this.state;
     return <h2 onClick={this.demo}>今天天气很{this.state.isHot?'炎热':'凉爽'}</h2>
   }
  demo = ()=>{
     const isHot = this.state.isHot;
     this.setState({isHot:!isHot})
     console.log(this);
}

 }
ReactDOM.render(<Weather/>,document.getElementById('example'))
```
解释：给h2绑定一个点击事件，把这个函数放在原型上，当点击h2的时候，就顺着原型链找到这个点击函数，赋值给onClick的回调函数，点击的时候调用这个点击函数。但是这个时候执行函数，函数中的this指向的是undefined。因为这个点击函数是类中定义的函数，在局部是严格模式，是undefined，所以我们要解决这个问题。
  我们将这个点击函数的this指向更改为实例对象，再赋值给实例对象，这是时候就在实例中创建了一个‘相同’的点击函数， 我们点击h2时候的那个是原型上的那个点击函数。这个时候就可以在点击函数中通过this来改变state的状态了
## 2.2props
```
  class Person extends React.Component{
    render() {
      const {name,age,sex} = this.props
      return (
        <ul>
          <li>姓名：{name}</li>
          <li>年龄：{age+1}</li>
          <li>性别：{sex}</li>
        </ul>
      )
    }
  }
const p = {name:'rose',age:'30',sex:'女'}
  // 因为有babel和React在这里，所以标签里面能用...p展开运算符来复制对象。别的地方不能 使用
  ReactDOM.render(<Person {...p}/>,document.getElementById('text3'))
```
解释：父子组件中通讯：在父组件中通过<子组件 {...对象}/>将这个值传过去传到子组件的props中
### 限制传值的类型，必传型
- 引入props-types
- 在子组件中使用组件.propTypes和组件.defaultProps
```
  class Person extends React.Component{
    render() {
      const {name,age,sex} = this.props
      return (
        <ul>
          <li>姓名：{name}</li>
          <li>年龄：{age+1}</li>
          <li>性别：{sex}</li>
        </ul>
      )
    }
 // 当你设置了这个之后，当创建组件就去看你的名字是不是字符串，是不是有内容，当然要引入prop_types包
  Person.propTypes = {
    name: PropTypes.string.isRequired,//限制name为字符串，且为必传
    age: PropTypes.number,            //限制age为数字
    sex: PropTypes.string.isRequired,//限制sex为字符串，且为必传
    speak: PropTypes.func             //限制speak为函数
  }
  // 设置默认值
  Person.defaultProps = {
    age:18,
    sex:'不男不女'
  }
  }
const p = {name:'rose',age:'30',sex:'女'}
  ReactDOM.render(<Person {...p}/>,document.getElementById('text3'))
```
### 类式组件使用props
```
  function Person(props){
    const {name,age,sex} = props;
   return (
        <ul>
          <li>姓名：{name}</li>
          <li>年龄：{age}</li>
          <li>性别：{sex}</li>
        </ul>
      )
  }
  // 这回这里的限制不能写在里面，要写在外面
  Person.propTypes = {
      name: PropTypes.string.isRequired,
      age: PropTypes.number,            
      sex: PropTypes.string.isRequired,
      speak: PropTypes.func          
    }
    Person.defaultProps = {
      age:18,
      sex:'不男不女'
    }
  const p = {name:'rose',age:30,sex:'女'}
  ReactDOM.render(<Person {...p}/>,document.getElementById('text3'))
```

## 2.3refs
### 方法一：使用字符串
```
 class Mycomponent extends React.Component{
    showData = ()=>{
      // 通过this.refs来获取组件中标签写了ref = XXX的标签
      const {input1} = this.refs;
      console.log('我被点击了');
      alert(input1.value);
    }
    onblurData = ()=>{
      const {input2} = this.refs;
      alert(input2.value);
    }
    render(){
      return (
        // input1包的双引号，说明是字符串，还有其他形式的
        // 这种形式已经过时了，因为效率低
        <nav>
          <input ref="input1" type="text" placeholder = '请输入' />
          <button onClick={this.showData}>点我提示左侧数据</button>
          <input ref="input2" onBlur={this.onblurData} type="text" placeholder="失去焦点跳出"/>
        </nav>
      )
    }
  }
  ReactDOM.render(<Mycomponent />,document.getElementById('text'))
```
### 方法二：使用回调函数
```
 class Mycomponent extends React.Component{
    showData = ()=>{
      // 这里是this(实例 )
      const {input1} = this;
      alert(input1.value);
    }
    onblurData = ()=>{
      const {input2} = this;
      alert(input2.value);
    }
    render(){
      return(
        // 通过回调函数的形式来书写ref
        // 解释：当创建组件的时候调用render发现，有一个回调函数，会主动给你调用，将当前结点传进去
        //       将结点赋值给当前实例(this,箭头函数中的this指向上面有this的那个this就是render的this就是实例)并取名叫input1(.input1)
        //       将这个ref挂在了实例身上了
        // 这个注释属实鸡肋
        <nav>
          { /*<input ref={(currentNode)=>{this.input1 = currentNode}} type="text" placeholder = '请输入' />*/}
          <input ref={currentNode => this.input1 = currentNode} type="text" placeholder = '请输入' />
          <button onClick={this.showData}>点我提示左侧数据</button>
          <input ref={c=>this.input2 = c} onBlur={this.onblurData} type="text" placeholder="失去焦点跳出"/>
        </nav>
      )
    }
  }
  ReactDOM.render(< Mycomponent/>,document.getElementById('text'))
```
### 方法三：使用ref容器
```
class Mycomponent extends React.Component{
    // 通过React.createRef调用能返回一个ref容器，该容器能存储被ref标识的节点，
    // 该容器一个节点只能用一个
    myref = React.createRef()
    myref2 = React.createRef()
    showData = ()=>{
      console.log(this.myref);
      alert(this.myref.current.value);
    }
    onblurData = ()=>{

      alert(this.myref2.current.value);
    }
    render(){
      return(
    // 1.点击事件Click的C是大写，因为不是像js一样操纵的是原生DOM，而是自定义(合成)事件 为了更高的兼容性
    // 4.React中的事件是通过事件委托的形式来给最外层的元素处理的  为了高效
      <nav>
          <input ref={this.myref} type="text" placeholder = '请输入' />
          <button onClick={this.showData}>点我提示左侧数据</button>
          <input ref={this.myref2} onBlur={this.onblurData} type="text" placeholder="失去焦点跳出"/>
        </nav>
      )
    }
  }
  ReactDOM.render(< Mycomponent/>,document.getElementById('text'))
```
# **当我们要在事件函数中传递参数的时候要使用高阶函数 合理化**
```
 class Login extends React.Component{
    state = {
      username:'',
      password:''
    }
  // 所以我们要将这个函数的返回值为一个函数作为onChange的回调函数!!实在要夸一句妙啊
    saveFormData = (datatype)=>{
     return (e) => {
      //  更妙的是这里，使用[]来读对象下的属性，我们一般情况都是使用对象.属性
      // 直接datatype就是将datatype这个属性添加到state中，而不是将username等传进去
      this.setState({[datatype]:e.target.value})
     }
    }
    handleSubmit = (e) =>{
      e.preventDefault();
    }
    render(){
      return (
        // 在这里面，用户名，密码都要调用函数，当还有邮箱，复选框，文本框等，太多了。所以我们onChange调用事件改变一下
        // 在这里我们saveFormData后面不能加小括号，因为加了小括号的意思是：
        //  将调用saveFormData这个函数，username传过去会被e接收，再将返回值交给onChange的回调函数
        // 而我们想要的是将这个saveFormData函数作为onChange的回调函数，当改变的时候就调用
        <form action="#" onSubmit = {this.handleSubmit}>
           <input type="text" onChange={this.saveFormData('username')} placeholder="请输入..." name="username" />
           <input type="password" onChange={this.saveFormData('password')} placeholder="请输入..." name="password" />
           <button>提交</button>
        </form>
      )
    }
  }
  ReactDOM.render(<Login/>,document.getElementById('text'))
</script>
```
解释：
 高阶函数：符合下面其中一个就是高阶函数
    1.传入的参数是函数
    2.返回的是一个参数
    我们这里的saveFormData就是一个高阶函数
    还有Promise setTimeOut map set filter 等等

  函数的柯里化：通过函数调用的方式继续返回函数的方式，实现多次接收参数最后统一处理
  比如这里的saveFormData：先将username传进来，调用那个箭头函数，将e传进来，当username和e两个
  参数都传进来后同意进行处理。
  ```
  function sum(a){
   ruturn (b)=>{
     return (c)=>{
       return a+b+c
     }
   }
  }
  const result = sum(1)(2)(3)
  console.log(result)
```
# 3.生命周期
我们之前在讲类式组件和函数式组件的时候讲了两个的执行过程
- 执行类式组件过程：
1.React解析组件标签，找到了MyComponent组件。
2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
...
***
但是当我们new出该实例到呈现到页面上组件经历了许多的过程
- **初始化阶段**: 由ReactDOM.render()触发---初次渲染
1.constructor()
2.在更新之前获取快照----getDerivedStateFromProps 
3.render()
4.组件挂载完毕的钩子----**componentDidMount()** =====> 常用
一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息

- **更新阶段**: 由组件内部this.setSate()或父组件重新render触发
1.getDerivedStateFromProps
2.控制组件更新的“阀门”----shouldComponentUpdate() 返回值为true才进行下去
3.render()
4.getSnapshotBeforeUpdate
5.组件更新完毕的钩子----**componentDidUpdate(preProps,preState,snapshotValue)** =====> 常用

- **卸载组件**: 由ReactDOM.unmountComponentAtNode()触发
1.组件将要卸载的钩子-----**componentWillUnmount()**  =====> 常用
 一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息
			
# 4.diff算法
 1). react/vue中的key有什么作用？（key的内部原理是什么？）
      2). 为什么遍历列表时，key最好不要用index?
      
			1. 虚拟DOM中key的作用：
					1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

					2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 
                                      随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

									a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
												(1).若虚拟DOM中内容没变, 直接使用之前的真实DOM
												(2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM

									b. 旧虚拟DOM中未找到与新虚拟DOM相同的key
												根据数据创建新的真实DOM，随后渲染到到页面
									
			2. 用index作为key可能会引发的问题：
								1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
												会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

								2. 如果结构中还包含输入类的DOM：
												会产生错误DOM更新 ==> 界面有问题。
												
								3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，
									仅用于渲染列表用于展示，使用index作为key是没有问题的。
					
			3. 开发中如何选择key?:
								1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
								2.如果确定只是简单的展示数据，用index也是可以的。

		慢动作回放----使用index索引值作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=0>小张---18<input type="text"/></li>
					<li key=1>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=0>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

	-----------------------------------------------------------------

	慢动作回放----使用id唯一标识作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=3>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

案例

	class Person extends React.Component{

		state = {
			persons:[
				{id:1,name:'小张',age:18},
				{id:2,name:'小李',age:19},
			]
		}

		add = ()=>{
			const {persons} = this.state
			const p = {id:persons.length+1,name:'小王',age:20}
			this.setState({persons:[p,...persons]})
		}

		render(){
			return (
				<div>
					<h2>展示人员信息</h2>
					<button onClick={this.add}>添加一个小王</button>
					<h3>使用index（索引值）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj,index)=>{
								return <li key={index}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
					<hr/>
					<hr/>
					<h3>使用id（数据的唯一标识）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj)=>{
								return <li key={personObj.id}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
				</div>
			)
		}
	}

	ReactDOM.render(<Person/>,document.getElementById('test'))
**总结**：最好数据中要有唯一确定的值来确定这个标识

# 5.脚手架
（这里使用的是v5的）
## 5.1如何开启脚手架
- 安装
npm i -g create-react-app 为创建react项目脚手架库
- 创建
create-react-app 项目名
- 启动
npm start
项目的整体技术架构为:  react + webpack + es6 + eslint
## 5.2react项目内容介绍

	public ---- 静态资源文件夹
		favicon.icon ------ 网站页签图标
		index.html -------- 主页面
		logo192.png ------- logo图
		logo512.png ------- logo图
		manifest.json ----- 应用加壳的配置文件
		robots.txt -------- 爬虫协议文件
     src ---- 源码文件夹
		App.css -------- App组件的样式
		App.js --------- App组件
		App.test.js ---- 用于给App做测试
		index.css ------ 样式
		index.js ------- 入口文件
		logo.svg ------- logo图
		reportWebVitals.js
			--- 页面性能分析文件(需要web-vitals库的支持)
		setupTests.js
			---- 组件单元测试的文件(需要jest-dom库的支持)

## 5.3父子组件之间互传数据
##### 一层层的传
- 我们一般在src中创建组件components
- 可以将.js写为.jsx
- 下载ES7-react插件，使用rcc创建类式组件，rfc创建函数式组件
- 在v5中一般使用类式组件，因为父子之间能通过props来传数据
- 状态在哪里，操作状态的方法就在哪里
- 子传夫数据：在父中定义函数，<子组件 函数={this.函数}/>，然后在子组件中调用函数(函数在this.props中)，就能将数据传到父组件
- 遍历数组使用map((pre,now)=>{})
- 解构+重命名
let obj = {a:{b:1}}
const {a} = obj; //传统解构赋值
const {a:{b}} = obj; //连续解构赋值
const {a:{b:value}} = obj; //连续解构赋值+重命名
- 通过let newList = [dolistObj,...dolist]来将dolistObj追加到dolist的第一行
##### 关系老远组件之间传
使用PubSub
1.下载 npm install pubsub-js --save
2.引入：import PubSub from 'pubsub-js'
3.使用：
```
在需要数据的组件中
 componentDidMount(){
    // 订阅一个MY TOPIC
        //{isFirst:false,isLoading:true}这个数据对象能被stateObj接收
    var token = PubSub.subscribe('MY TOPIC', (_,stateObj)=>{
      this.setState(stateObj)
    })
  }
  componentWillMount(){
    // 在渲染组件之后取消订阅
    PubSub.unsubscribe(this.token)
  }


在传递数据的组件中
// 发布这个MY TOPIC，传递数据
    PubSub.publish('MY TOPIC',{isFirst:false,isLoading:true});
原理就是能够发现这个MY TOPIC，并检测
```
## 5.4axios
前提：
1.	React本身只关注于界面, 并不包含发送ajax请求的代码
2.	前端应用需要通过ajax请求与后台进行交互(json数据)
3.	react应用中需要集成第三方ajax库(或自己封装)
所以使用axios

使用：
- 下载 npm i axios 
- 引入 import axios from 'axios'
- 使用：
```
axios.get('http://localhost:5000/students').then(
			response => {console.log('成功了',response.data);},
			error => {console.log('失败了',error);}
		)
```
但是我们现在是3000的本地要去请求来自5000的数据，存在跨域问题。解决办法：react脚手架配置代理
##### 方法一：
在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1. 优点：配置简单，前端请求资源时可以不加任何前缀。
2. 缺点：不能配置多个代理。
3. 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）



##### 方法二

1. 第一步：创建代理配置文件

   ```
   在src下创建配置文件：src/setupProxy.js
   ```

2. 编写setupProxy.js配置具体代理规则：

   ```js
   const proxy = require('http-proxy-middleware')
   
   module.exports = function(app) {
     app.use(
       proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
         target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
         changeOrigin: true, //控制服务器接收到的请求头中host字段的值
         /*
         	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
         	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
         	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
         pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
       }),
       proxy('/api2', { 
         target: 'http://localhost:5001',
         changeOrigin: true,
         pathRewrite: {'^/api2': ''}
       })
     )
   }
   ```
说明：
1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。

项目
  ```
export default class Search extends Component {

	search = ()=>{
		//获取用户的输入(连续解构赋值+重命名)
		const {keyWordElement:{value:keyWord}} = this
		//发送请求前通知App更新状态
		this.props.updateAppState({isFirst:false,isLoading:true})


		//发送网络请求
		axios.get(`/api1/search/users?q=${keyWord}`).then(
			response => {
				//请求成功后通知App更新状态
				this.props.updateAppState({isLoading:false,users:response.data.items})
			},
			error => {
				//请求失败后通知App更新状态
				this.props.updateAppState({isLoading:false,err:error.message})
			}
		)


	//发送网络请求---使用fetch发送（优化）
		try {
			const response= await fetch(`/api1/search/users?q=${keyWord}`)
			const data = await response.json()
			console.log(data);
			PubSub.publish('atguigu',{isLoading:false,users:data.items})
		} catch (error) {
			console.log('请求出错',error);
			PubSub.publish('atguigu',{isLoading:false,err:error.message})
	   	 }
       }
}
	render() {
		return (
			<section className="jumbotron">
				<h3 className="jumbotron-heading">搜索github用户</h3>
				<div>
					<input ref={c => this.keyWordElement = c} type="text" placeholder="输入关键词点击搜索"/>&nbsp;
					<button onClick={this.search}>搜索</button>
				</div>
			</section>
		)
	}
}

```
# 6.路由
# redux
  - 有啥用？当多个组件要共享一个状态数据的时候，我们将这个数据放在redux中，那个组件需要就去redux中取
- 原理是啥？
![redux原理图.png](https://upload-images.jianshu.io/upload_images/27278342-eaa4b7ca353c087a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
解释下：就是我需要数据的那个组件调用store的dispatch方法将action(type,data)传给reducers，reducers根据type确定如何去加工数据，但是reducers只负责简单的工作，将这个新状态返回给store。
1.action:负责对需要数据那个组件生成action对象，里面有type,data,分为同步和异步，服务员
`export const createIncrementAction = data => ({type:increment,data}) //我需要加来操作数据`
2.store:主要就是负责管理整个redux,能使唤reducers来操作，大boss
3.reducers：主要负责获取到action对数据进行操作，就是个打工仔，码农，厨师...

- 怎么用？
1.在src下创建一个redux再在下面创建store reductor action
2.action还分为同步和异步就是返回值不同，返回值是对象就是同步，返回值为函数就是异步
```
//同步action，就是指action的值为Object类型的一般对象
export const createIncrementAction = data => ({type:increment,data})
export const createDecrementAction = data => ({typ,data})

//异步action，就是指action的值为函数,异步action中一般都会调用同步action，异步action不是必须要用的。
export const createIncrementAsyncAction = (data,time) => {
	return (dispatch)=>{
		setTimeout(()=>{
			dispatch(createIncrementAction(data))
		},time)
	}
}
```
store：
```

```
reductor:
