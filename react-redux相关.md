### 1.redux和react是怎么配合的?
react-redux提供了connect和Provider两个好基友，它们一个将组件与redux关联起来，一个将store传给组件。
组件通过dispatch发出action，store根据action的type属性调用对应的reducer并传入state和这个action，reducer
对state进行处理并返回一个新的state放入store，connect监听到store发生变化，调用setState更新组件，此时组件的props也就跟着变化。

### 2.react-redux保存登录信息

