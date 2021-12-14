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
2.控制组件更新的“阀门”----shouldComponentUpdate(nextProps,nextState) 返回值为true才进行下去
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
## 基本使用
1.解释：在组件中需要不刷新页面来跳转部分页面。在平常我们都使用a标签来跳转页面，在这里我们可以使用路由的`<Link />`  
2.使用：
- 下载 npm i react-router-dom
- 引入路由，需要啥引入啥 import {...} from react-router-dom 
- `<App>`的最外侧包裹了一个`<BrowserRouter>`或`<HashRouter>` 
- 引入路由组件
- 在合适的地方使用路由组件 ` <Link className="list-group-item" to="/路径">路由组件</Link>`
- 在下面注册路由`	<Route path="/路径" component={About}/>`  
  
3.使用路由的时候太长了，我们可以封装一个`	<NavLink activeClassName="atguigu" className="list-group-item" {...this.props}/>`在使用路由组件的时候直接就可以`<MyNavLink to="/about">About</MyNavLink>`  
4.在注册路由的时候，当path相同的时候，会循环查找注册路由里面的，所以我们引入了`<Switch/>`，形成一一匹配，匹配好了就不向下查找了  
5.解决多级路径刷新页面样式丢失的问题
	- public/index.html 中 引入样式时不写 ./ 写 / （常用）
	- public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）
	- 使用HashRouter  
6.默认路由`<Redirect to="/about"/>`
7.嵌套路由，套娃开始  
8.父传子(大传小)传参数
- 1.传递params参数  
   - 使用路由组件时：`<Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link>`  
   - 注册路由组件时：`<Route path="/home/message/detail/:id/:title" component={Detail}/>`要声明传过去参数(id,title)  
   - 在路由组件中通过`	const {id,title} = this.props.match.params`接收参数
- 2.传递search参数
  - 使用路由组件时：`<Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link>`
  - 正常注册路由组件
  - 在路由组件中通过`	const {id,title} = this.props.location.search`接收参数
- 3.传递state参数
   - 使用路由组件时：`<Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>`  
   - 正常注册路由组件  
   - 在路由组件中通过`const {id,title} = this.props.location.state`接收参数  

9.返回是通过出栈方式还是代替方式  
   - 默认是push
   - 如果是替换(replace)的话`	<Link replace to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>`  
  
10.编程式路由导航
 不用`<Link />`和`<NavLink/>`让路由跳转，借助路由身上独有的API-->history,通过this.props.history对操作路由跳转、前进、后退  
 - this.prosp.history.push()
 - this.prosp.history.replace()
 - this.prosp.history.goBack()
 - this.prosp.history.goForward()
 - this.prosp.history.go()
  ```
  export default class Message extends Component {
	state = {
		messageArr:[
			{id:'01',title:'消息1'},
			{id:'02',title:'消息2'},
			{id:'03',title:'消息3'},
		]
	}

	replaceShow = (id,title)=>{
		//replace跳转+携带params参数
		//this.props.history.replace(`/home/message/detail/${id}/${title}`)

		//replace跳转+携带search参数
		// this.props.history.replace(`/home/message/detail?id=${id}&title=${title}`)

		//replace跳转+携带state参数
		this.props.history.replace(`/home/message/detail`,{id,title})
	}

	pushShow = (id,title)=>{
		//push跳转+携带params参数
		// this.props.history.push(`/home/message/detail/${id}/${title}`)

		//push跳转+携带search参数
		// this.props.history.push(`/home/message/detail?id=${id}&title=${title}`)

		//push跳转+携带state参数
		this.props.history.push(`/home/message/detail`,{id,title})
		
	}

	back = ()=>{
		this.props.history.goBack()
	}

	forward = ()=>{
		this.props.history.goForward()
	}

	go = ()=>{
		this.props.history.go(-2)
	}

	render() {
		const {messageArr} = this.state
		return (
			<div>
				<ul>
					{
						messageArr.map((msgObj)=>{
							return (
								<li key={msgObj.id}>

									{/* 向路由组件传递params参数 */}
									{/* <Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link> */}

									{/* 向路由组件传递search参数 */}
									{/* <Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link> */}

									{/* 向路由组件传递state参数 */}
									<Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>

									<button onClick={()=> this.pushShow(msgObj.id,msgObj.title)}>push查看</button>
									<button onClick={()=> this.replaceShow(msgObj.id,msgObj.title)}>replace查看</button>
								</li>
							)
						})
					}
				</ul>
				<hr/>
				{/* 声明接收params参数 */}
				{/* <Route path="/home/message/detail/:id/:title" component={Detail}/> */}

				{/* search参数无需声明接收，正常注册路由即可 */}
				{/* <Route path="/home/message/detail" component={Detail}/> */}

				{/* state参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>

				<button onClick={this.back}>回退</button>&nbsp;
				<button onClick={this.forward}>前进</button>&nbsp;
				<button onClick={this.go}>go</button>

			</div>
		)
	}
}
 ```
  10.BrowserRouter与HashRouter的区别
			1.底层原理不一样：
						BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。
						HashRouter使用的是URL的哈希值。
			2.path表现形式不一样
						BrowserRouter的路径中没有#,例如：localhost:3000/demo/test
						HashRouter的路径包含#,例如：localhost:3000/#/demo/test
			3.刷新后对路由state参数的影响
						(1).BrowserRouter没有任何影响，因为state保存在history对象中。
						(2).HashRouter刷新后会导致路由state参数的丢失！！！
			4.备注：HashRouter可以用于解决一些路径错误相关的问题。



























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



























# 打包
## 通过npm run build(webpack中的)  
通过打包将react转化为js,将一个项目打包上线运行
- 在生产环境打开服务器，用当前文件夹作为服务器使用serve build

# 扩展
## 1.setState()的两种用法
  ### 方法一，传对象  
  我们知道原来使用setState({count:99})里面直接调用对象，但是在后面要查看修改后的count值。显示的是修改前的，因为它是异步的，调用了setState()之后，它说等会，我先执行后面的(就是先执行查看再去修改),但是setState({},()=>{查看状态}),里面有回调函数。  

### 方法二，传函数  
  在setState中不止能传对象，还能传函数setState(()=>{},[(state,props)=>{}])---(函数，回调函数)。  
使用这个函数的好处在于能接收到state和props,返回的是方法一的对象
```
export default class Demo extends Component {

	state = {count:0}

	add = ()=>{
		//对象式的setState
		/* //1.获取原来的count值
		const {count} = this.state
		//2.更新状态
		this.setState({count:count+1},()=>{
			console.log(this.state.count);
		})
		//console.log('12行的输出',this.state.count); //0 */

		//函数式的setState，这是简写
		this.setState( state => ({count:state.count+1}))

    this.setState((state,props)=>{
      return {count:state.count+1}
    })
	}

	render() {
		return (
			<div>
				<h1>当前求和为：{this.state.count}</h1>
				<button onClick={this.add}>点我+1</button>
			</div>
		)
	}
}
```
## 2.lazyLoading  
当我们写了很多的组件，但是一进入网页一刷新，就将所有的组件都加载进来了，使用lazy能解决这个问题，当要使用某个组件的时候才加载该组件。
 - 引入lazy import React, { Component,lazy,Suspense} from 'react'
 - 使用懒加载引入组件  const Home = lazy(()=> import('./Home') )
 - 配合Suspense使用，因为当网速慢的时候能显示个 Loading组件(该组件使用普通加载)
  ```
  <Suspense fallback={<Loading/>}>
		{/* 注册路由 */}
			<Route path="/about" component={About}/>
			<Route path="/home" component={Home}/>
	</Suspense>
  ```

## 3.hook
就是能让函数式组件能使用state和props.通过  
- `const [count,setCount] = React.useState(count值)`和  
- `const myRef = React.useRef()`
- `React.useEffect(()=>{},[])`相当于**componentDidMount** []意思是什么都不检测
- `React.useEffect(()=>{},[count])`相当于**componentDidUpdate**，[count])意思是检测count的更新，取决于[]的内容。
- `React.useEffect(()=>{return ()=>{}},[])`中第一个参数函数的返回的那个函数相当于**componentWillUnmount**

```
/类式组件
 class Demo extends React.Component {
	state = {count:0}
	myRef = React.createRef()

	add = ()=>{
		this.setState(state => ({count:state.count+1}))
	}

	unmount = ()=>{
		ReactDOM.unmountComponentAtNode(document.getElementById('root'))
	}

	show = ()=>{
		alert(this.myRef.current.value)
	}

	componentDidMount(){
		this.timer = setInterval(()=>{
			this.setState( state => ({count:state.count+1}))
		},1000)
	}

	componentWillUnmount(){
		clearInterval(this.timer)
	}

	render() {
		return (
			<div>
				<input type="text" ref={this.myRef}/>
				<h2>当前求和为{this.state.count}</h2>
				<button onClick={this.add}>点我+1</button>
				<button onClick={this.unmount}>卸载组件</button>
				<button onClick={this.show}>点击提示数据</button>
			</div>
		)
	}
} 




//函数式组件
function Demo(){
	const [count,setCount] = React.useState(0)
	const myRef = React.useRef()

	React.useEffect(()=>{
		let timer = setInterval(()=>{
			setCount(count => count+1 )
		},1000)
		return ()=>{
			clearInterval(timer)
		}
	},[])

	//加的回调
	function add(){
		//setCount(count+1) //第一种写法
		setCount(count => count+1 )
	}

	//提示输入的回调
	function show(){
		alert(myRef.current.value)
	}

	//卸载组件的回调
	function unmount(){
		ReactDOM.unmountComponentAtNode(document.getElementById('root'))
	}

	return (
		<div>
			<input type="text" ref={myRef}/>
			<h2>当前求和为：{count}</h2>
			<button onClick={add}>点我+1</button>
			<button onClick={unmount}>卸载组件</button>
			<button onClick={show}>点我提示数据</button>
		</div>
	)
}

export default Demo
```
## 4.Fragment  
这是啥呢？就是阿，你看App包了个div，组件里面还会有div，包的太多了，为了符合jsx语法。
我们可以将这个div该为`<Fragment>代码<Fragment/>`或者`<></>`空标签(代码量小)

## 5.Context
在隔代组件之间，祖组件给后代传数据，使用context，也是在实例对象上，通过this.context获取  
使用：  
- 创建context对象`const MyContext = React.createContext()`和`const {Provider} = MyContext`
- 第一种：在祖组件中将后代组件用`<Provider value="状态参数">后代组件<Provider/>`包裹，当这个后代组件包括自己的全部后代组件只要举手(`static contextType = MyContext`)都能通过this.context接收.这种只能是类组件使用。
- 第二种：类式组件函数式组件都能使用，写为函数式组件(后代组件)
   - 引入Consumer `const {Provider,Consumer} = MyContext`
   - 使用。。。。还是直接看代码吧

```
//创建Context对象
const MyContext = React.createContext()
const {Provider,Consumer} = MyContext
export default class A extends Component {
	state = {username:'tom',age:18}

	render() {
		const {username,age} = this.state
		return (
			<div className="parent">
				<h3>我是A组件</h3>
				<h4>我的用户名是:{username}</h4>
				<!-- 传数据 -->
				<Provider value={{username,age}}>
					<B/>
				</Provider>
			</div>
		)
	}
}

class B extends Component {
	render() {
		return (
			<div className="child">
				<h3>我是B组件</h3>
				<C/>
			</div>
		)
	}
}
//第一种
/* class C extends Component {
	//声明接收context
	static contextType = MyContext
	render() {
		const {username,age} = this.context
		return (
			<div className="grand">
				<h3>我是C组件</h3>
				<h4>我从A组件接收到的用户名:{username},年龄是{age}</h4>
			</div>
		)
	}
} */
//第二种
function C(){
	return (
		<div className="grand">
			<h3>我是C组件</h3>
			<h4>我从A组件接收到的用户名:
			<Consumer>
				{value => `${value.username},年龄是${value.age}`}
			</Consumer>
			</h4>
		</div>
	)
}
```
## 6. 组件优化

### Component的2个问题 

> 1. 只要执行setState(),即使不改变状态数据, 组件也会重新render()
>
> 2. 只当前组件重新render(), 就会自动重新render子组件 ==> 效率低

### 效率高的做法

>  只有当组件的state或props数据发生改变时才重新render()

### 原因

>  Component中的shouldComponentUpdate()总是返回true

### 解决

	办法1: 
		重写shouldComponentUpdate()方法
		比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false
	办法2:  
		使用PureComponent
		PureComponent重写了shouldComponentUpdate(), 只有state或props数据有变化才返回true
		注意: 
			只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false  
			不要直接修改state数据, 而是要产生新数据
	项目中一般使用PureComponent来优化

## 7.render props
刚开始我们定义父子关系,是像如下代码，一个组件中嵌套一个组件
```
export default class Parent extends Component {
	render() {
		return (
			<div>
				<h3>我是Parent组件</h3>
				<A/>
			</div>
		)
	}
}

class A extends Component {
	state = {name:'tom'}
	render() {
		return (
			<div>
				<B/>
			</div>
		)
	}
}

class B extends Component {
	render() {
		console.log('B--render');
		return (
			<div>
				<h3>我是B组件</h3>
			</div>
		)
	}
}
```
除了通过这样还可以通过这样
```
export default class Parent extends Component {
	render() {
		return (
			<div>
				<h3>我是Parent组件</h3>
				<A>
				   <B/>
				<A/>
			</div>
		)
	}
}

class A extends Component {
	render() {
		return (
			<div>
				<h3>我是A组件</h3>
				<!-- 在这里声明使用B组件，因为B组件放在这里面 -->
				{this.props.children}
			</div>
		)
	}
}

class B extends Component {
	render() {
		return (
			<div>
				<h3>我是B组件</h3>
			</div>
		)
	}
}
```
以上还能借助插槽的思想
```
export default class Parent extends Component {
	render() {
		return (
			<div>
				<h3>我是Parent组件</h3>
				<!-- 到时候可以随便更改B组件这个位置为任何组件，返回值为一个组件 -->
				<A render={(name)=><B name={name}/>}/>
			</div>
		)
	}
}

class A extends Component {
	state = {name:'tom'}
	render() {
		console.log(this.props);
		const {name} = this.state
		return (
			<div>
				<h3>我是A组件</h3>
				<!-- 调用一下render函数，并将A的状态传过去 -->
				{this.props.render(name)}
			</div>
		)
	}
}

class B extends Component {
	render() {
		console.log('B--render');
		return (
			<div className="b">
				<h3>我是B组件,{this.props.name}</h3>
			</div>
		)
	}
}
```
## 8.书写错误边界
就是吧，为了防止一个组件的字符串解析错误呀等导致整个项目出错，我们使用错误边界，将这个错误限制在一定范围内(使用该组件时)。写在这个组件的父亲身上。
使用这个生命周期钩子getDerivedStateFromError(error)，并携带错误
```
export default class Parent extends Component {

	state = {
		hasError:'' //用于标识子组件是否产生错误
	}

	//当Parent的子组件出现报错时候，会触发getDerivedStateFromError调用，并携带错误信息
	static getDerivedStateFromError(error){
		console.log('@@@',error);
		return {hasError:error}
	}

	componentDidCatch(){
		console.log('此处统计错误，反馈给服务器，用于通知编码人员进行bug的解决');
	}

	render() {
		return (
			<div>
				<h2>我是Parent组件</h2>
				{this.state.hasError ? <h2>当前网络不稳定，稍后再试</h2> : <Child/>}
			</div>
		)
	}
}
```
经过打包这个错误边界就起作用了  
只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误

##### 使用方式：

getDerivedStateFromError配合componentDidCatch

```js
// 生命周期函数，一旦后台组件报错，就会触发
static getDerivedStateFromError(error) {
    console.log(error);
    // 在render之前触发
    // 返回新的state
    return {
        hasError: true,
    };
}

componentDidCatch(error, info) {
    // 统计页面的错误。发送请求发送到后台去
    console.log(error, info);
}
```
## 9. 组件通信方式总结

#### 方式：

		props：
			(1).children props
			(2).render props
		消息订阅-发布：
			pubs-sub、event等等
		集中式管理：
			redux、dva等等
		conText:
			生产者-消费者模式

#### 组件间的关系

		父子组件：props
		兄弟组件(非嵌套组件)：消息订阅-发布、集中式管理
		祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(用的少)

# v6react-router
>1.单独有一个router文件写组件,不在App中直接写
>>- 在router中注册路由
>>- 在router中要准备一个默认组件,通过`<Route path="/"  element={<Home/>}/>` 
```
//这是router
export default function Router() {
  return (
    <div>
      <Routes>
        <Route path="/"  element={<Home/>}/>
        <Route path="/about"  element={<About/>}/>
      </Routes>
    </div>
  )
}

//这是Home指定的默认组件(点击跳到About组件)
export default function Home() {
  return (
    <div>
      <h2>我是Home组件</h2>
      <Link to="/about">点击</Link>
    </div>
  )
}
```
>>- 
