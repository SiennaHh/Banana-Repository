### 1.redux和react是怎么配合的?
react-redux提供了connect和Provider两个好基友，它们一个将组件与redux关联起来，一个将store传给组件。
组件通过dispatch发出action，store根据action的type属性调用对应的reducer并传入state和这个action，reducer
对state进行处理并返回一个新的state放入store，connect监听到store发生变化，调用setState更新组件，此时组件的props也就跟着变化。

### 2.react-redux保存登录信息和清除登录信息，并取用
1. Provider组件是让所有的组件可以访问到store。不用手动去传。也不用手动去监听。

connect函数作用是从 Redux state 树中读取部分数据，并通过 props 来把这些数据提供给要渲染的组件。也传递dispatch(action)函数到props。

### 3. 被react-redux的connect包裹的子组件取不到this.props.history这个对象？
*** 解决方法：
```js
import PropTypes from 'prop-types'
```

```js
你的组件.contextTypes = {
  router: PropTypes.object.isRequired
}
```
最后console.log(this.context)
