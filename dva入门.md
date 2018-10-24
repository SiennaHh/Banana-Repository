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

