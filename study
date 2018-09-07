更新元素渲染
更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法,只会更新必要的部分

组件
组件的返回值只能有一个根元素。这也是我们要用一个<div>来包裹所有<Welcome />元素的原因。
无论是使用函数或是类来声明一个组件，它决不能修改它自己的props


将函数转换为类
你可以通过5个步骤将函数组件 Clock 转换为类

1.创建一个名称扩展为 React.Component 的ES6 类

2.创建一个叫做render()的空方法

3.将函数体移动到 render() 方法中

4.在 render() 方法中，使用 this.props 替换 props

5.删除剩余的空函数声明

setState()
1.不要直接更新状态 应当使用this.setState({comment: 'Hello'});

2.构造函数是唯一能够初始化 this.state 的地方

3.状态更新可能是异步的，this.props 和 this.state 可能是异步更新的
setState() 来接受一个函数而不是一个对象
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));


数据自顶向下流动
