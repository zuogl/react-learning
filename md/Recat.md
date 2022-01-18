# 一、React简介
### 1.Recat介绍
- recat是一个用于构建用户界面的jacascript库。
- 是一个将数据渲染为HTML视图的开源jacascript库。
- 由Facebook开发，且开源。
### 2.原生JavaScript的痛点
- 原生JavaScript操作DOM繁琐（==DOM-API操作UI==）
- 原生JavaScript操作DOM效率低，浏览器会进行大量的==重绘重排==。
- 原生JavaScript没有==组件化==编码方案，代码复用率低。

### 3.React的特点
1. 采用==组件化==模式、==声明式编码==，提高开发效率及组件复用率。
2. 在React Native中可以使用React语法进行==移动端开发==
3. 使用==虚拟DOM==和优秀的==Diffing算法==，尽量减少与真实DOM的交互

# 二、React API
### 1.虚拟dom解释
1. 其本质是一个object类型的对象
2. 虚拟dom比较“轻”，真实dom比较“重”；因为虚拟dom是React内部在用的，无序真实dom上那么多的属性。
3. 虚拟dom早晚会被React转为真实dom，并呈现在页面中。

### 2.jsx语法规则
1. 定义虚拟dom时，最外层不能写引号

2. 标签结构中，混入js表达式时，要用花括号`{}`包裹。（表达式和语句的区别详见附录）
```js
  <h2 id={myid} className ='mes'>我是小左</h2>
```
3. 样式的类名指定不要用class，要用className
```js
<h2 className="demo1" id={myId.toLowerCase()}>我是小左</h2>
```
4. 行内样式，要用`style = {{}}`这种形式，且像font-size这种属性要转为fontSize
```js
<span style={{color:'red',fontSize:'30px'}}>{myMessage}</span>
```
5. 只能有一个根标签。如果出现多个根标签，直接报错。
6. 标签必须闭合
```js
 <input type="text"/>;//向input这种标签可以采用自闭合的方式
````
7. 标签首字母：
- 若标签首字母小写：则将该标签转为HTML中同名的元素，若HTML中无该元素，则报错。

```js
<peiqi style={{color:'red',fontSize:'30px'}}>{myMessage}</peiqi>
;//Warning: The tag <peiqi> is unrecognized in this browser.
```
- 若首字母大写，则React去渲染对应的组件，若没有定义过该组件，报组件未定义。

```js
ReactDOM.render(<Peiqi/>,document.getElementById('test'));//Uncaught ReferenceError: Peiqi is not defined
```


### 3. ReactDOM.render()
该方法用于将虚拟DOM转换为真实的DOM，并将其放到指定的页面位置。
- 第一个参数：虚拟DOM
- 第二个参数：要放置的位置

## 4. 组件
### 1. 组件的定义
用来实现局部功能的代码和资源的集合
没有状态的就是简单组件。有状态的是复杂组件。

### 2. 函数式组件：
用`function`定义的组件称为函数式组件。其具有以下几点必须要注意的事项：
1. 定义时首字母必须大写
2. 必须有`return`返回值
3. 渲染的时候要有组件标签`<组件名/>`

#### 渲染函数式组件的步骤
执行了`ReactDOM.render(< Demo/>，document.getElementById('test'))`以后，发生了什么？
1. React解析组件标签,寻找Demo组件，若没找到，则报错：Demo is not definded ; 若找到了，则进行下一步：
2. React发现Demo是用函数定义的组件；React调用Demo函数，将返回的虚拟DOM结构渲染到页面。

### 3. 类式组件
用class定义的组件称为类式组件。其具有以下几点必须要注意的事项：
1. 类式组件必须要继承`React.Component`
2. 必须写`render`方法，==render中的this指向的是Demo的实例对象==
3. render方法必须要有`return`返回值（一般为页面结构）

#### 渲染类式组件的步骤
执行了`ReactDOM.render(< Demo/>，document.getElementById('test'))`以后，发生了什么？
1. React解析组件标签,寻找Demo组件，若没找到，报错：Demo is not definded ; 若找到了，则进行下一步：
2. React发现Demo是用类定义的，随后React帮我们去`new`了一个Demo的实例：d；
3. 通过d调用==Demo原型上的render方法==，即d.render()。
4. 将返回的虚拟DOM结构渲染到页面。

#### 关于类式组件的其他注意事项
1. 类中的构造器不是必须写的，要对实例进行初始化时，添加相对应的属性时才写。
2. 如果A类继承了B类，且A类中写了构造器，那么A类构造器中必须调用`super`方法。
3. 关于类式组件的实例对象，我们常见以下几种称谓：
xxx类的实例对象 === xxx组件的实例对象=== xxx组件对象

<p style ='color:red;font-weight:bold;'>类式组件之所以强大，是因为其具备三大核心属性---state、props、refs</p>


### 4. 组件之间的关系及使用
组件之间和HTML标签之间的关系一样，存在祖孙关系、兄弟关系、后代关系。
```js
class A extends React.Component{
	state = {carName:'阿特兹'}
//组件将要接收props（第一次不算）
	componentWillReceiveProps(){
		console.log('A---componentWillReceiveProps---')
	}
	render(){
		return (
			<div className="a">
			<h2>我是A组件，我有一台车：{this.state.carName}</h2>
			<h4>我家在：{this.props.address}</h4>
			<button onClick={this.changeCar}>发达了，换车！</button>
			// 在A组件的定义中使用B组件；B组件则称为A组件的后代组件。
			<B/>
		</div>
		)
	}
	changeCar = ()=>{
		this.setState({carName:"奔驰c63"})
	}
}
//定义一个B组件
class B extends React.Component{
	render(){
		console.log('B---render---')
		return (
		<div className="b">
			<h2>我是B组件，我收到了父亲给我的车：{this.props.cheming}</h2>	
		</div>
		)
	}
}

```

### 5.组件实例的三大核心属性
#### 1. state
组件的状态state能够驱动页面的显示。
1. state中的回调函数中this丢失原因。
定义回调函数时，其是在堆内存中的；当该回调被当作事件回调时，react中的事件容器会收集该回调（并放入栈内存中）。当事件触发时，直接从事件容器中调用，this指向window，但是，由于babel编译，全局严格模式；严格模式不允许自定义函数中this==默认==指向window所以this为undefined。
2. 解决state中的回调函数中this丢失的办法：
- bind
```js
        class Weather extends React.Component {
            constructor(props) {
                super(props);
                this.state = { isHot: true }
                this.changeHot = this.changeHot.bind(this);//用Weather原型上的changhot，将其this改变为指向实例对象，并将其添加到实例对象身上，还命名为changhot方法。
            }
            changeHot() {
                const { isHot } = this.state
                this.setState({ isHot: !isHot })
            }
            render() {
                return <h2 onClick={this.changeHot}>今天天气很{this.state.isHot ? '炎热' : "凉爽"}</h2>
            }
        }
```
- 赋值语句加箭头函数
```js
class Weather extends React.Component{
        // class类里边可以直接写赋值语句
        state = {
            isHot:true,
            wind:"七级大风"
        }
        render(){
            const {isHot,wind} = this.state
            return(
                <div>
                    <button onClick = {this.changeWeather}>天气预报 </button>
                    <h1>今天天气很{isHot === true? '炎热':"凉爽"}</h1>   
                    <h3>今天的风力为：{wind}</h3>
                </div>
            )
        }
        // 采用直接赋值的方式，对应的属性和方法都是直接加在实例对象身上的。用箭头函数解决了this指向丢失的问题
        changeWeather = () =>{
            const {isHot,wind} = this.state;
            this.setState({isHot: !isHot,wind:"我哪儿知道啊？"})
        }
    }
```


3. setState() 
   state不能直接更改，需要调用setState({})方法。
   this.setState({})方法主要有两个作用：
   
    1. 更改状态；
    2. 重新调用render
   
   <span style="color:red">setState本身是同步的，但是它的两个动作改状态/调render都是异步的。</span>
   3.2 setState的两种写法
   
    (1). setState(stateChange, [callback])------对象式的setState
   ​      1.stateChange为状态改变对象(该对象可以体现出状态的更改)
   ​      2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用<span style="color:red">最常用于在控制台输出最新的状态</span>
   
   ```js
   this.setState({sum:99})
   ```
    (2). setState(updater, [callback])------函数式的setState
   
   ​      1.updater为返回stateChange对象的函数。
   ​      2.updater可以接收到state和props。
   ​      3.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。<span style="color:red">最常用于在控制台输出最新的状态</span>
   
   ```js
   //函数式的setState，适用于：新状态依赖于原状态
   this.setState( state =>({sum:state.sum+1}))
   ```
   
   
   
   总结:
   
   ​    1.对象式的setState是函数式的setState的简写方式(语法糖)
   
   ​    2.使用原则：
   
   ​        (1).如果新状态不依赖于原状态 ===> 使用对象方式
   
   ​        (2).如果新状态依赖于原状态 ===> 使用函数方式
   
   ​        (3).如果需要在setState()执行后获取最新的状态数据, 要在第二个callback函数中读取
#### 2. props
该属性用于接收从组件外部传递的数据。
1. 传参的两种方式
- 直接在标签中一个一个的传参
```js
ReactDOM.render(<Person name="小左" sex="男" age="25"/>,document.getElementById('test'))
```
- 先定义一个需要传递的数据的对象，然后用展开运算符（...）展开
在原生的js语法中，展开运算符是不能够展开对象的。
react+babel可以让...展开一个对象，但仅仅适用于标签属性的传递。
```js
const p1 = {
    name:'小左',
    sex:'男'
    age:25
}
ReactDOM.render(<Person {...p1}/>,document.getElementById('test'))
```
2. 获取传入的参数
用解构赋值获取props中的数据。
```js
const {name,sex,age} = this.props
```

3. 对props进类型、必要性的行限制
在对props进行限制时，需要引入`prop-types.js`库。很少用到限制。
- 类式组件的限制
对类式组件的限制是在定义类时，在类的静态属性`propTypes`和`defaultProps`中进行限制的。
```js
class Person extends React.Component{
	//对传递的props进行:类型、必要性的限制
	static propTypes = {
	    name:PropTypes.string.isRequired,
	    age:PropTypes.number.isRequired,
	    sex:PropTypes.string
	}
	//给props设置默认值
	static defaultProps = {
		sex:'男',
	}
	render(){
		const {name,sex,age} = this.props
		return (
			<ul>
			    <li>姓名：{name}</li>
			    <li>性别：{sex}</li>
			    <li>年龄：{age+1}</li>
			</ul>
		)
	}
}
```
- 函数式组件的限制
对函数式中的props进行限制时，是在定义完了组件后，在组件之外进行的。这点违反了模块化的基本概念，所以基本不用。下边的例子中Person是定义的组件名称。
```js
//对传递的props进行:类型、必要性的限制
Person.propTypes = {
    name:PropTypes.string.isRequired,
    age:PropTypes.number.isRequired,
    sex:PropTypes.string
}
//给props设置默认值
Person.defaultProps = {
	sex:'男',
}
```

#### 3. refs
在进行事件操作时，收集对应的节点。==收集到的是真实的DOM节点==。
#### 1. 字符串形式的ref
会被refs自动收集。
```js
class Demo extends React.Component{
	render(){
		return(
            <div>
            // 在理解上，可以将ref当作id来对待。
			    <input ref="input1" type="text"/>
			    <button onClick={this.showData}>点左侧数据</button>&nbsp;
			    <input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
	    </div>
		)
	}
	//点击按钮的回调
	showData = ()=>{
		alert(this.refs.input1.value)
	}
	//失去焦点的回调
	showData2 = ()=>{
		alert(this.refs.input2.value)
	}
}
```
#### 2. 回调形式的ref
该类型的ref直接加载在事件对象身上，不会被refs收集；回调建议写成箭头函数类型，避免this的问题。
```js
class Demo extends React.Component{
	render(){
		return(
            <div>
            // 将当前节点传递给实例对象的input1属性。
			    <input ref={ currentElement => this.input1 = currentElement}  type="text"/>
			    <button onClick={this.showData}>点我提示左侧数据</button>&nbsp;
			    <input ref={ c => this.input2 = c } onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
	    </div>
		)
	}
	showData = ()=>{
		alert(this.input1.value)
	}
	showData2 = ()=>{
		alert(this.input2.value)
	}
}
```
#### 3. React.createRef()
createRef()会创建一个容器，该容器装着当前节点。容器的结构为对象｛current:节点名｝但是一个容器只能装一个节点
```js
class Demo extends React.Component{
	//准备好一个容器存储节点---容器“专人专用”
	r1 = React.createRef()
	r2 = React.createRef()
	render(){
		return(
		<div>
		    <input ref={this.r1} type="text"/>
		    <button onClick={this.showData}>点我提示左侧数据</button>&nbsp;
		    <input ref={this.r2} onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
	    </div>
		)
	}
	showData = ()=>{
        //对应的节点在current属性中，所有在获取节点时，必须要用current属性
		alert(this.r1.current.value)
	}
	showData2 = ()=>{
		alert(this.r2.current.value)
	}
}
```

## 5. React的事件处理
1. React 底层并没有使用原生的DOM事件，而是采用了合成事（自定义事件）
2. React自己实现了一套高效的事件系统，包含：事件注册、绑定、存储、分发等全套逻辑 ；在原生DOM事体系的基础上有了很大的改进；减少了内存的消耗，最大程度上决了浏览器的兼容性问题。
3. React中事件都是通过事件委托的方式实现的。
4. React中通过onxxx去绑定事件（注意大小写）
5. 可以通过event.target得到发生事件的事件对象

## 6. 受控组件和非受控组件
1. 非受控组件
如果组件中的表单数据，是通过节点.value“现用现取”那么就是非受控组件。
```js
class Login extends React.Component{
	render(){
		return(
	    <form onSubmit={this.handleLogin}>
		    用户名：<input ref="username" type="text"/>
		    密码：<input ref="password" type="password"/>
		    <button>登录</button>
	    </form>
		)
	}
	handleLogin = (event)=>{
		event.preventDefault() //阻止默认行为
		const {username,password} = this.refs //此处获取的是节点！！
		alert(`用户名为：${username.value}，密码为：${password.value}`)
	}
}
```
2. 受控组件
表单中输入类的DOM，随着用户的输入，就会把输入的值维护到状态中的组件，称之为受控组件。
```js
class Login extends React.Component{
	//初始化状态（表单收集几个数据，我就在状态中准备几组key-value）
	state = {
		username:'', //存储用户名-初始值为空
		password:'' //存储密码-初始值为空
	}
	render(){
		return(
	    <form onSubmit={this.handleLogin}>
		    用户名：<input onChange={this.saveUserName} type="text"/>
		    密码：<input onChange={this.savePassword} type="password"/>
		    <button>登录</button>
	    </form>
		)
	}
	//用户名发生改变的回调
	saveUserName = (event)=>{
		this.setState({username:event.target.value})
	}
	//密码发生改变的回调
	savePassword = (event)=>{
		this.setState({password:event.target.value})
	}
	//登录按钮的回调
	handleLogin = (event)=>{
		event.preventDefault() //阻止默认行为
		const {username,password} = this.state //不再节点.value获取输入了，去状态中获取
		alert(`用户名为：${username},密码为：${password}`)
	}
}
```

## 7. 高阶函数和柯里化
1. 高阶函数：如果有一个函数符合下面2个规范中的任何一个，那么该函数就是高阶函数。
- 1.若A函数，接收的参数是一个函数，那么A函数就可以称之为高阶函数
- 2.若A函数，调用的返回值依然是一个函数，那么A函数就可以称之为高阶函数
常见的高阶函数有哪些？ Promise、bind、setTimeout、arr.map

2. 函数的柯里化技术：通过函数调用继续返回函数的方式，实现多次接收参数，最后统一处理的编码形式。
```js
function sum (a){
	return (b)=>{
		return (c)=>{
			return a+b+c
		}
	}
}
const res = sum(3)(4)(5)
console.log(res);//12
```
## 8. React的生命周期函数
一般的，我们称呼React的生命周期函数有如下称呼：
生命周期回调函数 <=> 生命周期钩子函数 <=>  生命周期函数 <=>  生命周期钩子
#### 1. 经典生命周期
下边是旧生命周期的原理图（经典版本;面试建议画这个图）
![](../img/2_react生命周期(旧).png)	
```js
1. 初始化阶段: 由ReactDOM.render()触发 或 父组件(A)中用到子组件(B)---初次渲染
	1.	constructor();//初始化
	2.	componentWillMount();// 组件即将挂载
	3.	render();//初始化+ 状态更新时调用
	4.	componentDidMount();//组件挂载完毕，只调用一次
		在钩子中常做一些初始化的事儿：
			- 在页面上一上来就要展示一些信息，可以在此发送网络请求
			- 订阅消息
			- 开启定时器
2. 更新阶段: 由组件内部this.setSate() 或 父组件重新render 触发
	1.	shouldComponentUpdate();//控制state的状态是否可以更改，返回的值为布尔值；如果为false，组件的状态仍旧会更新，但是render不会被执行。
	2.	componentWillUpdate();//组件将要更新
	3.	render();//状态更新时调用
	4.	componentDidUpdate(preProps,preState,snapshotValue);//组件更新完毕
			该钩子可以接收三个参数，更新前的props，state，快照值。
3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
	1.	componentWillUnmount();//组件即将卸载，只调用一次
		在该钩子中常做一些收尾的事儿。
			- 关闭与服务器的长连接
			- 清除之前开启的定时器
			- 取消消息订阅
forceUpdate:不改变任何state里的东西，强制更新一下
```

#### 2.新生命周期
下边是新生命周期的原理图
![](../img/react生命周期(新).png)	
在新生命周期中即将被取消的三个生命周期;被重命名且被不推荐！！！
```js
componentWillMount(); 被重命名为 UNSAFE_componentWillMount()
componentWillUpdate();被重命名为 UNSAFE_componentWillUpdate()
componentWillReceiveProps(); 被重命名为 UNSAFE_componentWillReceiveProps();//一次不接收，第二次才开始接收
```
在生命周期中新加入的两个生命周期 --也不常用
1. getDerivedStateFromProps(从props得到一个派生的状态)
- 该方法是对象的静态方法，所以必须要有`static`关键字
- 该钩子可以接收传递的数据存到props中，并且可以获取初始化时的state数据
- 该钩子必须返回一个状态对象；返回的数据会自动合并到state的状态里边
- 任何时候state的值都取决于props
	- 直接赋值props到state上；
	- 比较props和state的不同，然后去更新state
```js
static getDerivedStateFromProps(props，state){
	return props  
}
```
2. getSnapshotBeforeUpdate(在更新前获取一个快照)
- 此方法必须配合componentDidUpdate使用
- 只有返回的是DOM相关的值，才有意义
```js
getSnapshotBeforeUpdate(){
	// 要想给哪个节点拍照就得给哪个节点添加ref。
	return this.refs.title.innerText
}
```
新的声明周期总结
```js
1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
	1.	constructor()
	2.	getDerivedStateFromProps 
	3.	render()
	4.	componentDidMount()
2. 更新阶段: 由组件内部this.setSate()或父组件重新render触发
	1.	getDerivedStateFromProps
	2.	shouldComponentUpdate()
	3.	render()
	4.	getSnapshotBeforeUpdate
	5.	componentDidUpdate()
3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
	1.	componentWillUnmount()
```

## 9.Diffing算法原理
面试常问的方式：
- react/vue中的key有什么作用？（key的内部原理是什么？）
- 为什么遍历列表时，key最好不要用index?
- 请你简单的聊聊DOM的Difffing算法？
					
1. 虚拟DOM中key的作用：
- 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。

- 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：
	- 旧虚拟DOM中找到了与新虚拟DOM相同的key：
		- 若虚拟DOM中内容没变, 直接使用之前的真实DOM
		- 若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM
	- 旧虚拟DOM中未找到与新虚拟DOM相同的key根据数据创建新的真实DOM，随后渲染到到页面
											
2. 用index作为key可能会引发的问题：
	- 若对数据进行：逆序添加、逆序删除等破坏顺序操作:会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。
	- 如果结构中还包含输入类的DOM：会产生错误DOM更新 ==> 界面有问题。
	- 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。
	
3. 开发中如何选择key?:
	1. 最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
	2. 如果确定只是简单的展示数据，用index也是可以的。


## 10. React 脚手架
0. 配置淘宝镜像（要下很多的代码，不配置国内的镜像，下载龟速）
```js
npm config set registry https://registry.npm.taobao.org
yarn config set registry https://registry.npm.taobao.org -g
```
1. 全局安装命令
```js
npm i create-react-app -g //在新版的react中已经不支持全局的命令了
```

2. 创建项目
```js
在想要创建项目的目录下，运行 ： create-react-app `objectName(项目名)` //新版本命令更改为  yarn create react-app  `objectName(项目名)`
```
3. 启动项目
FaceBook 公司写了一套缜密的`webpac`k配置文件，将 `start` 作为了启动项目的key值。所以，我们只需要对应的项目下（注意在该级目录下必须要有`package.json`文件），执行 `yarn start`即可启动项目.

> 我们在配置短命令后，yarn运行时， 只需要 yarn + 对应的断命令key即可；而用npm运行时，需要用 npm run  +对应的短命令key值（start 不用写run）。


4. 项目文件分析
	在创建的初始项目中，会有以下文件：
	![](../img/react脚手架.jpg)	
	其中public和src文件夹内的文件如下：
	-  public ---- 静态资源文件夹
		- favicon.icon ------ 网站页签图标
		- index.html --------  <span style = 'color:red;font-Weight:bold'>主页面---重要</span>
		- logo192.png ------- logo图
		- logo512.png ------- logo图
		- manifest.json ----- 应用加壳的配置文件
		- robots.txt -------- 爬虫协议文件
	- src ---- 源码文件夹
		- App.css -------- App组件的样式
		- App.js ---------  <span style = 'color:red;font-Weight:bold'>App组件---重要</span>
		- App.test.js ---- 用于给App做测试
		- index.css ------ 样式
		- index.js -------  <span style = 'color:red;font-Weight:bold'>入口文件---重要</span>
		- logo.svg ------- logo图
		- reportWebVitals.js
				--- 页面性能分析文件(需要web-vitals库的支持)
		- setupTests.js
				---- 组件单元测试的文件(需要jest-dom库的支持)

## 11.脚手架中配置代理
#### 1. 方法一
1. 在package.json中添加代理配置。
将请求服务器的地址添加到package.json中,作为的proxy的值。
```js
"proxy":"http://localhost:5000"//请求目标地址
```
2. 在代码中，将axios的请求地址，更改为自己打开浏览器的地址。
```js
getStudentInfo = ()=>{
    axios.get('http://localhost:3000/students').then(//我这儿在浏览器中是用http://localhost:3000打开我的页面的，所以就在3000端口给3000端口发请求，肯定不会跨域，然后在由代理的3000端口发给我的请求地址5000端口。
      response =>{console.log('成功了',response.data)},
      err =>{console.log('失败了',err)}
    )
  }
```
3. 优缺点：
- 优点：配置简单，前端请求资源时可以不加任何前缀。
- 缺点：不能配置多个代理。
- 工作方式：上述方式配置代理，当用Ajax请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）

#### 2. 方法二---setuProxy.js配置
使用第二种方法，在前端发送请求时，需要添加对应的前缀。
1. 第一步：创建代理配置文件
   ```js
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
3. 优缺点：
- 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
- 缺点：配置繁琐，前端请求资源时必须加前缀。


## 12. 消息发布与订阅
消息订阅与发布可以实现任意组件间的通信。提供数据的组件，发布消息；接收数据的组件，订阅消息。

state的位置一般放在订阅方。

消息的订阅和发布可以使用`pubsub-js`库。

1. 发布---`PubSub.publish('name', data)`
- 第一个参数是发布的消息的名字
- 第二个参数是发布的消息所要传递的数据
```js
 PubSub.publish('peiqi', { isFirst: false, isLoading: true, users: [], errMsg: '' })
```

2. 订阅---`PubSub.PubSub.subscribe('name',callback)`
消息的订阅一般都是写在componentDidMount这个钩子中的。订阅消息的方法会返回一个唯一id（和定时器同理），我们通常用this.id来接这个返回值，并将其储存到实例对象身上，方便取消订阅时候使用。
- 第一个参数是订阅的消息的名字
- 第二个参数是对发布消息所传递过来的数据的处理回调函数
	- 回调方式有两个参数
		- 第一个参数是订阅消息的名字，我们一般用_代替；
		- 第二个是传递过来的数据。
```js
componentDidMount(){
        this.id =   PubSub.subscribe('peiqi',(_,data)=>{
             this.setState(data)
          })
      }
```

3. 取消订阅---`PubSub.unsubscribe(id)`
为了节约内存，组件的声明周期结束的时候，需要取消消息订阅。取消订阅一般写在componentWillUnmount这个钩子中
取消订阅的方法，接受一个参数--要取消的订阅的id
```js
 componentWillUnmount(){
        PubSub.unsubscribe(this.id)
    }
```

## 13.路由
### 1.SPA的理解
- 单页Web应用（single page web application，SPA）。
- 整个应用只有一个完整的页面。
- 点击页面中的链接不会刷新页面，只会做页面的局部更新。
- 数据都需要通过ajax请求获取, 并在前端异步展现。
### 2.路由的理解
- 一个路由就是一个映射关系(key:value)
- key为路径, value可能是function或component
### 3.路由分类
1.	后端路由：
- 理解： value是function, 用来处理客户端提交的请求。
- 注册路由： router.get(path, function(req, res))
- 工作过程：当服务器接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据
2.	前端路由：
- 浏览器端路由，value是component，用于展示页面内容。
- 注册路由: <Route path="/test" component={Test}>
- 工作过程：当浏览器的path变为/test时, 当前路由组件就会变为Test组件
### 4. 原始的路由库
```js
<button onclick="push('/test1')">路径改为/test1</button><br><br>
<button onclick="replace('/test2')">路径改为/test2</button><br><br>

// 引入用于操作浏览器历史记录的库---history
<script type="text/javascript" src="https://cdn.bootcss.com/history/4.7.2/history.js"></script>
<script type="text/javascript">
// 创建一个历史对象
	let history = History.createBrowserHistory()
// 压栈，实现历史页面的记录，网址变化
	function push(url){
		history.push(url)
	}
// 替换，也会导致浏览器的地址变化，但是不会产生浏览记录，不可以回退。
	function replace(url){
		history.replace(url)
	}
// 监听浏览器的历史记录的变化
	history.listen((location)=>{
		console.log(location)
	})
</script>
```
### 5. react-router-dom
在react中，有一个专门的库用于实现路由的管理
#### 1. 引入
```js
import {Link,BrowserRouter,Route} from 'react-router-dom'
```
#### 2. API介绍
##### 1. Link--编写路由链接
	- react中，使用Link组件标签，实现地址的切换,其中的to属性是目标地址的key.该标签需要被一个BrowserRouter标签包裹着，但是一般将BrowserRouter标签直接包裹整个App组件。(Link最终也被翻译成了a标签)
```js
	<Link className="list-group-item" to="/about" >About</Link>
```
##### 2. NavLink
	可以添加自带属性`activeClassName`。当该标签被使用时（激活时）,添加该属性值对应的类名。当该属性的值为active时，`activeClassName = 'active'`可以忽略
```js
	<NavLink className="list-group-item" activeClassName = 'peiqi' to="/about" >About</NavLink>
	// 下边这行代码自带 activeClassName = 'active'
	<NavLink className="list-group-item" to="/about" >About<NavLink>
```
##### 3. Route--注册路由
	- 注册路由，当路径是xxx的时候，渲染xxx组件。同样的，该标签需要被一个BrowserRouter标签包裹着，但是一般将BrowserRouter标签直接包裹整个App组件。
```js
<Route path ="/about" component = {About}/> ;//当路径为/about时，渲染About组件。
```
##### 4. 路由组件接收到的props
路由组件的props是固定的，主要有如下内容
	history:
		- go: ƒ go(n)  ======既可以前进，也可以后退
		- goBack: ƒ goBack()=======后退函数
		- goForward: ƒ goForward()=====前进函数
		- push: ƒ push(path, state)=====产生记录，可以后退
		- replace: ƒ replace(path, state)====不产生记录，不可以后退
	location:
		- pathname: "/about"=====记录路径
		- search: ""====search传递参数
		- state: null====state传参
	match:
		- params: {}=====接收路由传递的params参数

##### 5. Switch
在注册路由时，默认的react-router-dom会一直查询所有的路由规则，即使已经匹配了，也会一直查询，直到所有路由规则都匹配一遍。但是用Switch包裹后，就变成了一对一的模式，匹配上了后边的就不在查询了，提升运行速度。
```js
<Switch>
	<Route path="/about" component={About}/>
	<Route path="/home" component={Home}/>
	<Route path="/about" component={Test}/>
</Switch>
```
##### 6. 模糊/精准匹配
	react-router-dom的路由匹配默认是模糊匹配：在编写路由链接时的路径单词个数可以多于注册路由时需要的个数（但是顺序和单词得匹配）
```js
	// 可以使用exact={true}开启精准匹配 ，可以简写成exact
	<Route path = "/about" exact={true} component = {About} />
```
原则：
一般用默认匹配，只有当模糊匹配影响功能时，才使用精准匹配。
##### 7. 多级路由的使用
	1.编写路由链接、注册路由时要带有上一级路由的路径
	2.不要开启精准匹配
##### 8. 向路由组件传递参数
1. params参数

在编写路由链接时传递参数(携带参数)：
```js
<Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}/${msgObj.content}`}>{msgObj.title}</Link>
```
注册路由时声明参数
```js
<Route path="/home/message/detail/:id/:title/:content" component={Detail}/>
```
使用参数
```js
const {id,title,content} = this.props.match.params //接收路由的params参数
```
2. search参数

在编写路由链接时传递参数(携带参数)：传递search参数时，将参数写在？后面，形如key=value & key=value这种形式
```js
<Link to={`/home/message/detail?id=${msgObj.id}&title=${msgObj.title}&content=${msgObj.content}`}>{msgObj.title}</Link>
```
注册路由时无需声明参数
```js
<Route path="/home/message/detail" component={Detail}/>
```
使用参数，需要使用querystring库来解析传递的参数（框架自带了该库）
```js
const {search} = this.props.location
const {id,title,content} = qs.parse(search.slice(1))
```

3. state参数

在编写路由链接时传递参数(携带参数)：
```js
<Link 
	to={{
		pathname:'/home/message/detail',
		state:{
			id:msgObj.id,
			title:msgObj.title,
			content:msgObj.content,
		} 
	}}
>
	{msgObj.title}
</Link>
```
注册路由时声明参数
```js
<Route path="/home/message/detail" component={Detail}/>
```
使用参数
```js
const {id,title,content} = this.props.location.state
```
##### 9.路由跳转的两种模式
	- push
	产生历史记录，可以回退
	- replace
	不产生历史记录，不可以回退。用`replace ={true}`开启。
	```js
	<NavLink className="list-group-item" replace={true} to="/home/message">Message</NavLink>
	```

##### 10. 编程式路由
```js
	show = (msgObj,isPush)=>{
		return ()=>{
            //传递的第二个参数是state，用location.state来接
			isPush ? this.props.history.push('/home/message/detail',msgObj) : 
			this.props.history.replace('/home/message/detail',msgObj)
		}
	}

	houtui = ()=>{
        //回退
		this.props.history.goBack()
	}

	qianjin = ()=>{
	//前进
		this.props.history.goForward()
	}

	demo = ()=>{
		//正数前进，负数后退
		this.props.history.go(-2)
	}
```
##### 11. withRouter
用于加工一般组件，经过withRouter加工后，一般组件也有了路由组件的props中的history、location、match属性。
```js
import {withRouter} from 'react-router-dom'
export default withRouter(Header) //本来Heander是一个一般组件，没有history等属性的，经过withRouter的加工后，Header还是一般组件，但是有了history等属性，也可以进行函数式路由编程了。
```
##### 12. Redirect
用于设定默认选中的路由
```js
<Redirect to = "/about"/>//默认选中about路由
```
##### 13.HashRouter
作用与browserRouter类似，但是HashRouter不能传递state参数,在端口号后边会有一个#号,兼容性相对好些

## 14.redux
### 1. redux是什么
redux是一个专门用于做状态管理的JS库（不是react插件库），它可以在react、vue、angalar中使用。但基本与react配合使用。redux的作用：集中式管理react应用中多个组件共享的状态。（与setState不同，redux只管更新状态，不负责重新调用render）
[中文学习网站](http://www.redux.org.cn/)
### 2. redux使用场景
1.	某个组件的状态，需要让其他组件可以随时拿到（共享）。
2.	一个组件需要改变另一个组件的状态（通信）。
3.	总体原则：能不用就不用, 如果不用比较吃力才考虑使用。
### 3. rudex原理图及文件分析
![](../img/redux原理图.png)
redux工作原理
1. 将需要共享的state，传递给ActionCreators，然后ActionCreators生成一个action对象。该对象中有两个属性：type存在事件的类型（一般为事件名称的字符串），data存储着事件需要操作的数据（任意类型，对象、数组、Number等）
2. ActionCreators创建完成action后，将action对象传递给Store，然后Store将上一次的状态previousState和action一起传递给Reducers。
	- 需要注意的是，当Store第一次给Reducers传递数据时，是没有previouseStatedede的，这个时候previouseState的值是undefined，并且第一次传递的action中是没有data数据的，type类型也是一串自动生成的特殊字符串（这个传递称为初始化）
3. Reducers根据传递进来的action中的type和data,对previousState进行操作形成新的State，然后在传递给Store
	- 同样的，由于第一次传递过来的数据是残缺不全的，previouseState是undefined,所以在Reducers中一般会判断这个值，如果为undefined表示是初始化，直接返回最初的state。
4. Store拿到新的State后，再传递给组件。


在运用redux时，通常需要在src中新建一个redux文件夹，该文件夹包括的文件如下：
![](../img/redux文件夹图.jpg)
- store.js
store是redux的核心对象，一个redux项目只有一个store.js文件。该文件主要用于生成store对象。一般情况下，store是只支持一般action（对象类型）的传递的，为了使store对象能够调用异步action，我们需要使用一个中间件库`redux-thunk`（需要安装）。在该文件中，主要有如下代码
```js
import {createStore,applyMiddleware,combineReducers} from 'redux'//引入createStore，用于创建store对象。为了使store支持异步action，需要引入applyMiddleware

import countReducer from './count_reducer'//引入count_reducer

//为了支持开发工具，需要引入该方法
import {composeWithDevTools} from 'redux-devtools-extension'

import thunk from 'redux-thunk'//为了使store能够处理异步action，需要引入thunk。

//合并组件
const allReducers = combineReducers({
    sum:countReducer,
    persons:personReducer
})

//创建一个store对象,注意：创建store时就要指定好哪个reducer为store服务
//例子：餐厅老板在开业之前就找好了后厨团队
// 为了使store能够支持异步action，需要给creatStore传递第二个参数
const store = createStore(allReducers,composeWithDevTools(applyMiddleware(thunk))

//导出store
export default store
```
- xxx_reducer.js
xxx_reducer是用于创建为xxx组件服务的reducer的。每一个组件都相应的会有一个reducer（要使用/共享state的话），在该文件中主要有如下代码：
```js
/* 
	定义一个countReducer,专门用于操作Count组件相关的状态 
	会收到，之前的状态(preState) 、动作对象(action)
	根据action中的type和data决定如何加工数据
*/
import {INCREMENT,DECREMENT} from './constant'
const initState = 0 //初始化状态

export default function countReducer(preState=initState,action){
	//获取type和data
	const {type,data} = action
	//根据type决定如何操作数据
	switch (type) {
		//如果是加
		case INCREMENT: 
			return preState + data
		//如果是减
		case DECREMENT:
			return preState - data
		//如果是初始化
		default:
			return preState
	}
}
```
- xxx_action.js
xxx_action.js是用于定义创建action函数的。每一个action函数都返回一个action对象，每个action对象都有一个type和一个data属性。
```js
import {INCREMENT,DECREMENT} from './constant'

//专门用于创建“加”的action
export const createIncrementAction = value => ({type:INCREMENT,data:value})

//专门用于创建“减”的action
export const createDecrementAction = value => ({type:DECREMENT,data:value})
```
像上边那类返回一个action对象的我们称为同步action；下边我们在介绍一种异步的action,又叫做函数类型的action。
异步action的返回值不再是一个对象，而是一个函数，在这个函数中通常执行一些异步的任务。（异步action也是因此而来）
```js
export const createWaitIncrementAction = value => {
    // redux的作者在设计时就想好了，在处理函数类型的action时，可以传递一个参数，该参数用于分发要调用的一般action。
	// 在本案列中dispatch只是一个我们传递的一个实参，具体的名字可以随便写a、demo、test1等都可以，只不过一般习惯写为dispatch而已。
    return (dispatch) => {
        setTimeout(() => {
            dispatch(createIncrementAction(value*1))
        }, 500)
    }
}
```
- constant.js
该文件是用于定义一些编写代码时常用的单词，目的是避免某些粗心大意的同事写错单词
```js
export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
```
### 3. react-redux
#### 3.1 react-redux原理图及基本概念
Facebook公司为了使广大程序员能够在react中更好的使用redux，又推出了一个类似react补丁的库react-redux（需要安装）。在使用该库时，必须明确如下几点：
1. 所有的UI组件都应该被一个容器组件包裹,他们是父子关系。
2. 容器组件是真正和redux打交道的,里面可以随意的使用redux的api。
3. UI组件中不能使用任何redux的api
4. 容器组件会传通过props的方式给UI组件传递redux中所保存的状态和用于操作状态的方法。
![](../img/react-redux模型图.png)
#### 3.2 容器组件的创建
容器组件是不能由我们自己创建的。而是通过特定的方法生成的。
```js

/*
1. 该组件是专门用于和redux打交道的容器组件。
2. 容器组件是连接UI组件和redux的一个纽带
*/

// 引入UI组件
import CountUI from '../../components/Count'

// 引入connect方法，连接UI和redux
import { connect } from 'react-redux'
// 引入对应的action，用于映射给props，传递给UI组件操作对应的状态
import {createIncrementAction,
createDecrementAction,
createWaitIncrementAction
} from '../../redux/Count_action'

/*
mapStateToProps函数会被react-redux调用
mapStateToProps函数的职责是向UI组件传递redux中所保存的数据（状态）
mapStateToProps函数返回值必须是一个object类型的对象
    - 返回的这个对象的key就作为传给UI组件props的key
    - 返回的这个对象的value就作为传给UI组件props的value
*/
// 一般写法
function mapStateToProps(state) {
    return { sum: state }
}

/*
mapDispatchToProps函数会被react-redux调用
mapDispatchToProps函数的职责是向UI组件传递操作redux中状态的方法
mapDispatchToProps函数返回值必须是一个object类型的对象
    - 返回的这个对象的key就作为传给UI组件props的key
    - 返回的这个对象的value就作为传给UI组件props的value
*/
// 一般写法
function mapDispatchToProps(dispatch) {
    return {
        jia: (value) => { dispatch(createIncrementAction(value))},
        jian: (value) => { dispatch(createDecrementAction(value))},
        wait: (value) => { dispatch(createWaitIncrementAction(value))},

    }
}
// 利用connect生成Count的容器组件
// 容器组件中的store是靠props传进去的，而不是在容器组件中直接引入
// connect的第一个括号中第一个参数一定要是函数,该函数用于给UI组件传递state，第二个参数可以是函数，也可以是对象，用于给UI组件传递操作state的方法。

// connect的一般写法
const CountContainer = connect(mapStateToProps, mapDispatchToProps)(CountUI)
// 暴露容器组件
export default CountContainer

------------------------------------------------------------------------

// connect的精简写法
/*
箭头函数返回一个对象时，要在外边加一个括号（）
connect的第一个括号中第一个参数一定要是函数，第二个参数可以是函数，也可以是对象，是对象的话，会自动分发dispath
*/ 
export default connect(state=> ({ sum: state }),{
    jia: createIncrementAction,
    jian: createDecrementAction,
    wait: createWaitIncrementAction,
})(CountUI)
```
在容器组件创建成功后，在渲染时就不用渲染UI组件了，而是渲染容器组件（由于是父子关系，UI组件也会被自动渲染），并通过props的方式，将redux核心对面store传递给容器组件。
```js
import React, { Component } from 'react'
import Count from './container/Count'
import store from './redux/store'

export default class App extends Component {
  render() {
    return (
      <div>
        <Count store={store}/>
      </div>
    )
  }
}
```
当容器组件创建成功后，对应的UI组件中，可以直接通过this.props.xxx获取容器组件传递过来的保存在redux中的state和方法。
```js
<h2>当前的求和为：{this.props.sum}</h2>

add = () => {
    const { value } = this.selectedNum
    this.props.jia(value * 1)
}
```
#### 3.3 Provider的使用
Provider可以在底层给所有有需要的组件都加上store，并且也在底层监听了redux中的state变化
```js
ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root'));
```
#### 3.4 UI组件和容器组件的整合
在3.2种创建的容器组件时单独的一个文件，UI组件又是另一个单独的组件。其实这两个组件是可以整合在一起的（将UI组件粘贴到容器组件中，最后只暴露一个容器组件）。
```js
import React, { Component } from 'react'

// 引入connect方法，连接UI和redux
import { connect } from 'react-redux'
import {
    createIncrementAction,
    createDecrementAction,
    createWaitIncrementAction
} from '../../redux/Count_action'

// 定义Count的UI组件
class Count extends Component {
    state = {
        wind: "微风"
    }

    render() {
        console.log(this.props)
        return (
            <div>
                <h2>当前的求和为：{this.props.sum}</h2>
                <h2>今天的天气是：{this.state.wind}</h2>
                <select ref={c => this.selectedNum = c}>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                </select>&nbsp;
                <button onClick={this.add}>+</button>&nbsp;
                <button onClick={this.decrement}>-</button>&nbsp;
                <button onClick={this.incrementIfOdd}>当前和为基数才加</button>&nbsp;
                <button onClick={this.incrementWait}>等500ms才+</button>
            </div>
        )
    }

    add = () => {
        const { value } = this.selectedNum
        this.props.jia(value * 1)
    }

    decrement = () => {
        const { value } = this.selectedNum
        this.props.jian(value * 1)
    }

    incrementIfOdd = () => {
        const { value } = this.selectedNum
        if (!(this.props.sum % 2)) return alert('偶数不能加！')
        this.props.jia(value * 1)
    }


    incrementWait = () => {
        const { value } = this.selectedNum
        this.props.wait(value * 1)
    }
}

// connect的第一个括号中第一个参数一定要是函数，第二个参数可以是函数，也可以是对象，是对象的话，会自动分发dispath
export default connect(state => ({ sum: state }), {
    jia: createIncrementAction,
    jian: createDecrementAction,
    wait: createWaitIncrementAction,
})(Count)


//如果使用的合并的组件生成的store，这儿的state就是一个对象，我们可以利用结构赋值的形式获取对应的值
export default connect(({sum,person}) => ({ sum,person}),
    {
        increment, decrement
    }
)(Count)
```

### 4. 多组件合并版本
```js
//引入createStore，用于创建store对象;引入combineReducers用于合并多个reducer
import {createStore,applyMiddleware,combineReducers} from 'redux'

//引入count_reducer
import sum from './reducers/count'

//引入person_reducer
import persons from './reducers/person'

//引入redux-thunk专门，用于与处理函数类型的action（异步action）
import thunk from 'redux-thunk'

//引入composeWithDevTools,用于支持redux开发者工具
import {composeWithDevTools} from 'redux-devtools-extension'

//汇总所有reducer为一个总的reducer
//特别注意：combineReducers调用时传入的对象就是redux中的总状态对象
// 注意，这儿写的是对象的简写形式
const allReducer = combineReducers({
	sum,
	persons
})

//创建一个store对象,注意：创建store时就要指定好哪个reducer为store服务
const store = createStore(allReducer,composeWithDevTools(applyMiddleware(thunk)))

//导出store
export default store
```
```js
import React, { Component } from 'react'
import {connect} from 'react-redux'
import {addPerson} from '../../redux/actions/person'

class Person extends Component {
	render() {
		return (
			<div>
				<h1>我是Person组件</h1>
				<h4>上方的Count组件求和为{this.props.sum}</h4>
				<input type="text" ref={c => this.name = c} placeholder="输入名字"/>
				<input type="text" ref={c => this.age = c} placeholder="输入年龄"/>
				<button onClick={this.addPerson}>添加一个人</button>
				<ul>
					{
						this.props.persons.map((personObj)=>{
							return <li key={personObj.id}>{personObj.name}-{personObj.age}</li>
						})
					}
				</ul>
			</div>
		)
	}
	addPerson = ()=>{
		//获取用户输入
		const {name,age} = this
		// console.log(name.value,age.value)
		this.props.addPerson({id:Date.now(),name:name.value,age:age.value})
	}
}

export default connect(
	//映射状态
	state => ({
		persons:state.persons,
		sum:state.sum
	}),
	//映射操作状态的方法
	{addPerson}
)(Person)

```


### 5. 常用API
#### 1. store.getState()
用于获取新的状态
#### 2. store.subscribe()
用于监听redux中的数据变化。该方法接收一个回调函数作为参数。该方法最常用的方法有如下两种：
1. 在对应的子组件使用
在子组件中监听redux中的数据变化，并配合刷新页面带来的一个问题就是。每一个组件都要写一边，复用性低，代码冗余。
```js
// 组件一加载完毕，就开始监听redux中的数据是否发生变化
componentDidMount(){
//检测redux中的数据的改变
	store.subscribe(()=>{
	// 如果数据发生改变，就调用下边两种中的任意一种，都可以重新加载页面。
		/ this.forceUpdate() //强制更新
	// this.setState({}) //“欺骗react，达到调用render的目的”
})} 
```
2. 在项目入口文件index.js中使用
在项目入口文件中使用，虽然只用写一遍，但是一般redux中的数据发生改变，就会影响整个项目中的所有组件，会有一点点的效率问题。
```js
ReactDOM.render(<App/>,document.getElementById('root'));
//检测redux中的数据的改变,如果改变，渲染整个App组件。
store.subscribe(()=>{
	ReactDOM.render(<App/>,document.getElementById('root'));
})
```
#### 3. store.dispath()
该方法用于分发action。

### 15. Hooks
Hook是React 16.8.0版本增加的新特性/新语法,可以让你在函数组件中使用 state以及其他的React特性，下边我们来主要介绍一下最常见的3个hook:
- State Hook: React.useState()
- Effect Hook: React.useEffect()
- Ref Hook: React.useRef()

#### 1. State Hook---React.useState()
React.useState()传递的参数为需要初始化的数据，返回的是一个包含两个元素的数组,第一个元素就是state数据，第二个元素是用于修改state的方法。（一般用解构赋值的方式获取这两个值，且可以自由命名）
```js
export default function Hook() {
    const [sum, setSum] = React.useState(0)
   
    function increment() {
        setSum(sum + 1)
    }

	function autoIncrement() {
        setInterval(() => {
			/*
			修改state的方法可以接收函数作为参数
			a是形参，真实调用的时候指的是sum
			*/ 
            setSum(a => a + 1)
        }, 300);
    }

    return (
        <Fragment>
		// 初始化的值为0
            <h1>当前的和为：{sum}</h1>
            <button onClick={increment}>点击+1</button>
			<button onClick={autoIncrement}>点我自动持续加</button>
        </Fragment>
    )
}
```
#### 2. Effect Hook---React.useEffect()
React.useEffect()接收两个参数，第一个参数是一个函数，是componentDidMount()和componentDidUpdate()的集合体，第二个参数是一个数组，用于控制第一个参数到底是模仿哪个生命周期： 
- 如果不写，就是两者结合体
- 如果写一个空数组，相当于componentDidMount()
- 如果传入state中的某个数据，就相当于componentDidUpdate()(监控传入的数据)
第一个参数还可以返回一个函数，被返回的函数模拟的是componentWillUnmount() 
```js
    React.useEffect(() => {
        const timer = setInterval(() => {
            setName(()=> nameArry[parseInt(Math.random() * 5)])
        }, 500);
        return () => clearInterval(timer)
    }, [name])
```
#### 2. Ref Hook---React.useRef()
Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据,功能与React.createRef()一样
```js
const userInput = React.useRef()
function show(){
	console.log(userInput.current.value)
}
<input type="text" ref={userInput}/>\
```
函数式组件除了不可以使用字符串类型的ref外，其余几种都可以使用。

### 16. Context
一种组件间通信方式, 常用于祖组件与后代组件间通信

```js
// 引入createContext
import React, { Component, createContext } from 'react'
// 调用createContext，创建一个数据指挥官 一个应用只能有一个指挥官
const carContext = createContext()
// 从数据指挥官身上获取能传递数据的Provider，和可以直接接收数据的Consumer
const { Provider, Consumer } = carContext

// 被Provider标签包裹的组件及其后代组件都可以收到value中传递的数据 注意：传递数据时必须用value属性
<Provider value={this.state.car}>
    <B />
</Provider>


// 第一种接收数据的方式:必须要有关键字 static 和contextType属性
static contextType = carContext
<h1 >我是C（儿子）组件，我收到了爷爷给我的车：{this.context}</h1>

// 第二种接收数据的方式：函数组件没有this，所以可以用这种方式，但是这种方式也适用于类组件 
<Consumer>
// 必须写函数， data等于this.context 
    {data => data}
</Consumer>
```

### 17. 组件通信方式总结

#### 组件间的关系：
- 父子组件
- 兄弟组件（非嵌套组件）
- 祖孙组件（跨级组件）
- 其他

#### 几种通信方式：
1.props
2.消息订阅-发布：pubs-sub等
3.集中式管理：redux等
4.conText:生产者-消费者模式

#### 比较好的搭配方式：
父子组件：props
兄弟组件：消息订阅-发布、集中式管理
祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发少，封装插件用的多)
其他：消息订阅-发布、集中式管理


### 18. 其他
1. 在不适用webpack，单纯的使用script标签引入相对应的模块时，react的核心库一定要最先引入！
2. 在script中用jsx语法创建虚拟DOM时，要在script标签中写入`type=text/babel`，否则浏览器会将jsx的语法默认当作js语法，而导致错误。
3. 区分js语句和js表达式的区别
表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方(用一个变量来接，能接到值的都算表达式)常见表达式类型：
- 单独的变量：a
- 一些内置的函数：alert()
- 各种运算表达式：a+b
- 函数调用：fn(2)
- 定义函数：function test(){}
- 
- 
- 常见语句如下：
- if(){}
- for(){}
- switch()case()
4. 在react中，将组件分为两类
	- 一般组件
		在使用时，是<About/>使用的。About就是一般组件。一般组件一般放在components文件中
	- 路由组件
		在使用时，没有写<About/>，写的是 `component ={About}`,About就是路由组件；路由组件一般放在pages文件夹中。
5. Fragment
react要求在组件返回结构时必须有一个根标签包裹，用一般的div包裹时，会在页面的结构中多出一个没有实际意义的div标签，当我们的一个页面中有多个组件时，就会出现许多无用的div，Fragment可以完美的解决这个问题。用Fragment包裹要返回的结构，在页面中不会出现多余的div。<span style="color:red">Fragment标签能且只能有一个key属性，用于diffing算法的比对（如果用不到可以不写）</span>。
虽然用空标签`<></>`包裹也能不出现多余的div，但是空标签不能够写属性，在遍历返回的时候就不能使用该方法了。
```js
import { Fragment } from 'react'
export default class App extends Component {
  render() {
    return (
      <Fragment>
        <Hooks/>
      </Fragment>
    )
  }
}
```
