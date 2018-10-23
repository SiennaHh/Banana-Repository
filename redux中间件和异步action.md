## 1. 异步action
每次action被触发（dispatch），reducer就会同步地对store进行更新，在实际开发项目的时候，有很多需求都是需要通过接口等形式获取异步数据后再进行更新操作的.
## 2. 中间件 
redux 提供了一个中间件（插件）机制，可以使用第三方提供的中间件或自己编写一个中间件来对Redux的功能进行增强.
那么，在哪个环节添加功能呢？
```
（1）Reducer：纯函数，只承担计算 State 的功能，不合适承担其他功能，也承担不了，因为理论上，纯函数不能进行读写操作。

（2）View：与 State 一一对应，可以看作 State 的视觉层，也不合适承担其他功能。

（3）Action：存放数据的对象，即消息的载体，只能被别人操作，自己不能进行任何操作。
```
只有发送 Action 的这个步骤，即store.dispatch()方法，可以添加功能。

中间件就是一个函数，对store.dispatch方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能。
## 3. redux-thunk 
比如可以使用`redux-logger`这个中间件来记录action以及每次action前后的state、使用`redux-undo`来取消/重做action、使用`redux-persist-store`来对store进行持久化等,
要实现异步的action，最常用也是官方文档中使用的redux-thunk这个中间件来实现。
* 用法：
1. 安装，npm i redux-thunk -S
2. 使用applyMiddleware挂载中间件
   在创建store的时候，我们将ReduxThunk使用Redux.applyMiddleware方法进行包装后传给Redux.createStore的第二个方法
   ```js
   import {createStore, combineReducers, applyMiddleware} from 'redux';
   import * as home from './home/reducer';
   import * as production from './production/reducer';
   import thunk from 'redux-thunk';
   let store = createStore(
      combineReducers({...home, ...production}),
      applyMiddleware(thunk)
    );
   ```
   
   applyMiddlewares()是 Redux 的原生方法，作用是将所有中间件组成一个数组，依次执行。
* 注意：
   1.createStore方法可以接受整个应用的初始状态作为参数，那样的话，applyMiddleware就是第三个参数了。
   
```js
   const store = createStore(
      reducer,
      initial_state,
      applyMiddleware(logger)
    );
```
  2.中间件的次序有讲究。
```js
    const store = createStore(
      reducer,
      applyMiddleware(thunk, promise, logger)
    );
 ```
  

   
   ## redux-thunk解析
  1. 原来的store.dispatch方法只能接收一个普通的action对象作为参数，当我们加入了ReduxThunk这个中间件之后，store.dispatch还可以接收一个方法作为参数，
  这个方法会接收到两个参数，第一个是dispatch，等同于store.dispatch，第二个是getState，等同于store.getState，
  ```js
  store.dispatch((dispatch, getState) => dispatch({type: 'INCREASE'}));
  ```
  redux-thunk的简单实现：
  ```js
  function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```
createThunkMiddleware方法里的  "柯里化"
```js
({ dispatch, getState }) => next => action => {
  ...
};
```
其实是
```js
function ({dispatch, getState}) {
  return function (next) {
    return function (action) {
      ...
    };
  };
}
```
这其实就是一个Redux中间件的基本写法。

参数next是一个方法，这个方法的作用是通知下一个Redux中间件对这次的action进行处理： next(action)，如果一个中间件中没有执行next(action)，则action会停止向后续的中间件传递，并阻止reducer的执行（store将不会因为本次的action而更新）。

参数action就不用多说了，就是当前被触发的action。

在ReduxThunk这个中间件中，做的处理很简单：判断当前的action是否为一个方法，如果是，就执行action这个方法，并将store.dispatch与store.getState方法作为参数传递给action方法；如果不是，则执行next(action)将控制权转移给下一个中间件（如果有）。

所以当我们给store.dispatch方法传入一个方法的时候，ReduxThunk就会去执行这个方法，以达到自由控制action触发流程的一个目的。



--------------------- 
参考文章:
原文：https://blog.csdn.net/awaw00/article/details/55668558 

    
