# 更新元素渲染
更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法,只会更新必要的部分

# 组件
组件的返回值只能有一个根元素。这也是我们要用一个<div>来包裹所有<Welcome />元素的原因。
无论是使用函数或是类来声明一个组件，它决不能修改它自己的props


# 将函数转换为类
通过5个步骤将函数组件 Clock 转换为类

1.创建一个名称扩展为 React.Component 的ES6 类

2.创建一个叫做render()的空方法

3.将函数体移动到 render() 方法中

4.在 render() 方法中，使用 this.props 替换 props

5.删除剩余的空函数声明

# setState()
1.不要直接更新状态 应当使用this.setState({comment: 'Hello'});

2.构造函数是唯一能够初始化 this.state 的地方

3.状态更新可能是异步的，this.props 和 this.state 可能是异步更新的
setState() 来接受一个函数而不是一个对象
```js
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

# 数据自顶向下流动 (单向数据流)
父组件或子组件都不能知道某个组件是有状态还是无状态，并且它们不应该关心某组件是被定义为一个函数还是一个类

# 事件处理
1.React事件绑定属性的命名采用驼峰式写法

2.需要传入一个函数作为事件处理函数
onClick={activateLasers}

3. 用e.preventDefault()阻止默认行为;

4.类的方法默认是不会绑定 this，需要this.handleClick = this.handleClick.bind(this);
不绑定的解决方式：
（1）使用属性初始化器语法
```js
    handleClick = () => {
    console.log('this is:', this);
  }
 ``` 
（2）在回调函数中使用 箭头函数
     onClick={(e) => this.handleClick(e)}
建议在构造函数中绑定或使用属性初始化器语法

# 向事件处理程序传递参数
```js
1.<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>  //arrow functions 
```
显示的传递
```js
preventPop(name, e){    //事件对象e要放在最后
        e.preventDefault();
        alert(name);
    }
  ```
```js  
2.<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>  //Function.prototype.bind  
```
隐士的传递
# 条件渲染
1.&&
2.condition ? true : false
# 阻止组件渲染









