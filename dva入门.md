# 1. dva简介
基于 redux, react-router, redux-saga 的轻量级封装框架
# 2.dva的5个API
## 1. app = dva(opts)  创建应用，返回dva实例。
在opts可以配置所有的hooks
```js
const app = dva({
     history,
     initialState,
     onError,
     onAction,
     onStateChange,
     onReducer,
     onEffect,
     onHmr,
     extraReducers,
     extraEnhancers,
});
```
如果要配置 history 为 browserHistory，可以这样：
```js
import createHistory from 'history/createBrowserHistory';
const app = dva({
  history: createHistory(),
});
```
* initialState：指定初始数据，优先级高于 model 中的 state，默认是 {}，但是基本上都在modal里面设置相应的state。
## 2. app.use(Hooks)：配置 hooks 或者注册插件。
这里最常见的就是dva-loading插件的配置，
```js
import createLoading from 'dva-loading';
...
app.use(createLoading(opts));
```
## 3. app.model(ModelObject)：这个是你数据逻辑处理，数据流动的地方。
eg:
```js
app.model(require('./models/example'));
app.model(require('./models/todoList'))
```
## 4. app.router(Function)：注册路由表，我们做路由跳转的地方。
eg: 
```js
import { Router, Route } from 'dva/router';

app.router(({ history }) => {
  return (
    <Router history={history}>
      <Route path="/" component={App} />
    <Router>
  );
});
```
## 5. app.start([HTMLElement], opts)
启动项目
# 3. Dva九个概念
## 1.State
初始值，我们在 dva() 初始化的时候和在 modal 里面的 state 对其两处进行定义，其中 modal 中的优先级低于传给 dva() 的 opts.initialState
```js
// dva()初始化
const app = dva({
  initialState: { count: 1 },
});

// modal()定义事件
app.model({
  namespace: 'count',
  state: 0,
});
```
## 2. Action：表示操作事件，可以是同步，也可以是异步
action 的格式如下，它需要有一个 type ，表示这个 action 要触发什么操作；payload 则表示这个 action 将要传递的数据
```js
{
  type: String,
  payload: data,
}
```
我们通过 dispatch 方法来发送一个 action

Action
Action 表示操作事件，可以是同步，也可以是异步
```js
 {
  type: String,
  payload: data
}
```
格式
```js
dispatch(Action);
dispatch({ type: 'todos/add', payload: 'Learn Dva' });
```

其实我们可以构建一个Action 创建函数，如下
```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```
//我们直接dispatch(addTodo()),就发送了一个action。
dispatch(addTodo())
## 3. Model
model 是 dva 中最重要的概念，Model 非 MVC 中的 M，而是领域模型，用于把数据相关的逻辑聚合到一起，几乎所有的数据，逻辑都在这边进行处理分发

* state
这里的 state 跟我们刚刚讲的 state 的概念是一样的，只不过她的优先级比初始化的低，但是基本上项目中的 state 都是在这里定义的。

* namespace
model 的命名空间，同时也是他在全局 state 上的属性，只能用字符串，我们发送在发送 action 到相应的 reducer 时，就会需要用到 namespace 。

* Reducer
以key/value 格式定义 reducer，用于处理同步操作，唯一可以修改 state 的地方。由 action 触发。其实一个纯函数。
```js
reducers: {
    save(state, action) {
      return { ...state, ...action.payload };
    },
  },
```
* Effect
用于处理异步操作和业务逻辑，不直接修改 state，简单的来说，就是获取从服务端获取数据，并且发起一个 action 交给 reducer 的地方。

其中它用到了redux-saga，里面有几个常用的函数。
```js
*add(action, { call, put }) {
    yield call(delay, 1000);
    yield put({ type: 'minus' });
},
```
在项目中最主要的会用到的是 put 与 call。
## 4. Router
```js
export default function({ history }){
  return(
    <Router history={history}>
      <Route path="/" component={App} />
    </Router>
  );
}
```
# 4.整体架构
* 首先我们根据 url 访问相关的 Route-Component，在组件中我们通过 dispatch 发送 action 到 model 里面的 effect 或者直接 Reducer

* 当我们将action发送给Effect，基本上是取服务器上面请求数据的，服务器返回数据之后，effect 会发送相应的 action 给 reducer，由唯一能改变state 的 reducer 改变 state ，然后通过connect重新渲染组件。

* 当我们将action发送给reducer，那直接由 reducer 改变 state，然后通过 connect 重新渲染组件。

* 这样我们就能走完一个流程了。

