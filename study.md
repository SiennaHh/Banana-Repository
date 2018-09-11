# react学习记录
## react组件化
react的一个组件很明显的由dom视图和state数据组成，两个部分泾渭分明。state是数据中心，它的状态决定着视图的状态。
它只负责ui的渲染。与其他框架监听数据动态改变dom不同，react采用setState来控制视图的更新。setState会自动调用render函数，触发视图的重新渲染，如果仅仅只是state数据的变化而没有调用setState，并不会触发更新。 组件就是拥有独立功能的视图模块，许多小的组件组成一个大的组件，整个页面就是由一个个组件组合而成。它的好处是利于重复利用和维护。

更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法,只会更新必要的部分
 ***



# 组件
组件的返回值只能有一个根元素。这也是我们要用一个<div>来包裹所有<Welcome />元素的原因。
无论是使用函数或是类来声明一个组件，它决不能修改它自己的props


## 将函数转换为类
通过5个步骤将函数组件 Clock 转换为类

1.创建一个名称扩展为 React.Component 的ES6 类

2.创建一个叫做render()的空方法

3.将函数体移动到 render() 方法中

4.在 render() 方法中，使用 this.props 替换 props

5.删除剩余的空函数声明

## setState()
1.不要直接更新状态 应当使用this.setState({comment: 'Hello'});

2.构造函数是唯一能够初始化 this.state 的地方

3.状态更新可能是异步的，this.props 和 this.state 可能是异步更新的
setState() 来接受一个函数而不是一个对象
```js
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

## 数据自顶向下流动 (单向数据流)
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

## 向事件处理程序传递参数
```
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
隐示的传递
# 条件渲染
```
1.&&
2.condition ? true : false
```
如果条件变得过于复杂，就要考虑提取组件
## 阻止组件渲染
让 render 方法返回 null 而不是它的渲染结果即可实现。
```js
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }
```
组件的 render 方法返回 null 并不会影响该组件生命周期方法的回调。例如，componentWillUpdate 和 componentDidUpdate 依然可以被调用

# 列表 & Keys
渲染多个组件 map()方法遍历numbers数组
## 基础列表组件
当你创建一个元素时，必须包括一个特殊的 key 属性
```
const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
  ```
  ## Keys
  Keys可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化，通常使用来自数据的id作为元素的key，也可以使用index
  ## 用keystiquzujian
  1.如果你提取出一个ListItem组件，你应该把key保存在数组中的这个<ListItem />元素上，而不是放在ListItem组件中的<li>元素上
  2.元素的key在他的兄弟元素之间应该唯一
  
  ## 在jsx中嵌入map()
  声明了一个单独的listItems变量并将其包含在JSX中
  ```
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />

  );
  ```
  在map()中这样使用：
  ```
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />

      )}
    </ul>
  );
  ```
  # 表单
  ## 受控组件
  在React中，可变的状态通常保存在组件的状态属性中，并且只能用 setState() 方法进行更新
  ```
  <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
  </label>
  ```
  注意this.state.value是在构造函数中初始化，这样文本区域就能获取到其中的文本。
  ## select
  1.在React中，并不使用之前的selected属性，而在根select标签上用value属性来表示选中项
  
  2. ` <input type="text"> <textarea>` 和 `<select>` 都十分类似 - 他们都通过传入一个value属性来实现对组件的控制
  
  
  *** 
   
   # react生命周期
  组件的生命周期可分成三个状态：

  * Mounting：已插入真实 DOM
  * Updating：正在被重新渲染
  * Unmounting：已移出真实 DOM
  
   ## 生命周期的方法有：
  1.componentWillMount 在渲染前调用,在客户端也在服务端。

  2.componentDidMount : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他    JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)。

  3.componentWillReceiveProps 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。

  4.shouldComponentUpdate 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
  可以在你确认不需要更新组件时使用。

  5.componentWillUpdate在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。

  6.componentDidUpdate 在组件完成更新后立即调用。在初始化时不会被调用。

  7.componentWillUnmount在组件从 DOM 中移除的时候立刻被调用。   
  
  # 状态提升
  状态分享是通过将state数据提升至离需要这些数据的组件最近的父组件来完成的。这就是所谓的状态提升。
  
  props是只读的， prop 从父组件传递下来，子组件是没有控制权的，这个问题通常是通过让组件“受控”来解决。当我们想要响应数据改变时，使用父组件提供的   `this.props.onTemperatureChange()` 而不是`this.setState()` 方法。
  
  







