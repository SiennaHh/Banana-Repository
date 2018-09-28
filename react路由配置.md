# react简单路由配置 含登录页面路由

## 总路由Router.js
引入App.js，登录，注册页面路由写在本文件，页面路由文件pageRouter.js通过组件方式引入,登录注册页面没有Nav
```js
const Root = () => (
    <div>
        <Switch>
            <Route
                path="/"
                render={props => (
                    <App>
                        <Switch>
                            <Route path="/" exact component={Login}/>
                            <Route path="/Login"  component={Login}/>
                            <Route path="/register"  component={RegistrationForm}/>
                            <Route path="/"  component={pageRouter}/>
                            <Route render={() => <Redirect to="/"/>} />
                        </Switch>
                    </App>
                )}
            />
        </Switch>
    </div>
)
export default Root;
```
## 页面路由pageRouter.js
需要Nav，Nav放在这里。
```js
const pageRouter = () => (
    <div>
        <div className="app-nav">
            <Nav/>
        </div>
        <div className="main-content">
            <Switch>
                <Route path="/" exact component={ProductList}/>
                <Route path="/ProductList"  component={ProductList}/>
                <Route path="/Shopping"  component={Shopping}/>
                <Route path="/Detail"  component={Detail}/>
                <Route path="/AddProduct"  component={AddProduct}/>
                <Route path="/EditProduct/:id"  component={EditProduct}/>
                <Route render={() => <Redirect to="/"/>} />
            </Switch>
        </div>
    </div>
)
export default pageRouter;
```
## App.js
```js
class App extends Component {
  render() {
    console.log(this.props.children)
    return (
      <div className="App">
        <div className="app-head">
          <Header/>  //头部组件
            <div className="middle">
              <div className="main">
                  {this.props.children}  //Router.js 
              </div>
            </div>
          <Footer/>  //底部组件
        </div>
      </div>
    );
  }
}
export default App;
```
## 最外层 index.js 
```js
import Root from './router/Routers';  //总路由文件
import registerServiceWorker from './registerServiceWorker';
const mountNode = document.getElementById('root');
ReactDOM.render(
    <BrowserRouter>
        <Root />
    </BrowserRouter>,
    mountNode
);
registerServiceWorker();
```


