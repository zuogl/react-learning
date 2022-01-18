# React学习笔记

react是单向数据流

创建一个react需要考虑的几点：

1. 考虑静态UI，
2. 考虑组件的状态组成
   1. 需不需要内部维护state
   2. 需不需要外部传参
3. 考虑组件的交互方式



受控组件和非受控组件的概念

组件创建的原则：单一职责原则

1. 每个组件只干一件事情，

2. 如果组件变得复杂，那么应该拆分成小组件。

数据状态管理：DRY原则

1. 能计算得到的状态就不要单独存储
2. 组件尽量无状态，所需数据通过props获取

JSX：在JavaScript代码中直接写HTML标记
什么是表达式：是会返回一个值的js语法

约定：
React认为小写的tag是原生的DOM节点，如div,
大写字母开头为自定义组件
JSX标记可以直接使用属性语法，例如<menu.item>

生命周期
1. constructor
	1. 用于初始化内部状态，很少使用
	2. 唯一可以直接修改stae的地方
	
2. getDerivedStateFromProps
	1. 但state需要从props初始化时使用
	2. 尽量不要使用，维护两者状态一致性会增加复杂度
	3. 每次render都会调用
	4. 典型场景：表单控件获取默认值 
	
3. 	componentDidMount
	1. UI渲染完成后调用
	2. 只执行一次
	3. 典型场景：获取外部资源

4. componentWillUnmount

 	1. 组件移除时被调用
	2. 典型场景：释放 资源 	
	
5. 	getSnapshotBeforeUpdate
	1. 在页面render之前调用，state已更新
	2. 典型场景：获取render之前的DOM状态
	
6. componetDidUpdate
	1. 每次UI更新都被调用
	2. 典型场景：页面需要根据props变化重新获取数据
	
8. shouldComponentUpdate

   1. 决定Virtual DOM是否需要重绘
   2. 一般可以由PureComponent自动实现
   3. 典型场景：性能优化

   

Virtual DOM 



contextApi

​    解决组件间的通信问题。

	1. provider
 	2. consumer(需要作为prvider的子组件/元素)

​    React.createContext()



使用脚手架工具创建React应用



Create React App 

Codesandbox

Rekit
