---
title: React路由的基本使用
categories: React
tags:
  - react-router
abbrlink: 42024
date: 2020-11-01 17:15:33
---
### 前言
使用React构建的单页面应用，要想实现页面间的跳转，首先想到的就是使用路由。在最新React中，使用的是**react-router-dom**
### 安装路由
```
npm install --save-dev react-router-dom
```
### 路由的基础配置
**第一步:**  
将 `App.js` 更改成 `router.js`, 内容如下 :
```
/*router.js 页面里的代码
HashRouter: 有#号
BrowserRouter: 没有#号
Route: 设置路由与组件关联
Switch: 只要匹配到一个地址不往下匹配，相当于for循环里面的break
Link: 跳转页面，相当于vue里面的router-link
exact: 完全匹配路由
Redirect: 路由重定向
*/
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
//如若更换成 hash 路由模式需要将  BrowserRouter 更换为 HashRouter
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Route></Route>
            </Switch>            
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
**第二步:**
将入口文件`index.js`内容更改如下:
```
import React from 'react';
import ReactDOM from 'react-dom';
import RouterComponent from './router';
import * as serviceWorker from './serviceWorker';
function App() {
  return (
    <React.Fragment>
      <RouterComponent />
    </React.Fragment>
  )
}
ReactDOM.render(<App />, document.getElementById('root'));
// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```
**完成以上操作基础配置完成**
### 路由的使用
**在 pages 文件夹下新建 3 页面 :**
![](https://gitee.com/zuo_jiahui/blogimage/raw/master/img/a28b18e5b30b624193e834105772ac0.png)
```
// Index文件夹下的index.js
import React from 'react';
import { Link } from 'react-router-dom';
class IndexPage extends React.Component {
    render() {
        return (
            <React.Fragment>
                <div>首页</div>
                <ul>
                    <li><Link to="/news">前往新闻页面</Link></li>
                </ul>
            </React.Fragment>
        )
    }
}
export default IndexPage
```
```
// New文件夹下index.js
import React from 'react';
import { Link } from 'react-router-dom';
class News extends React.Component {
    render() {
        return (
            <React.Fragment>
                <div>我是新闻页面</div>
                <ul>
                    <li><Link to="/news/details">新闻详情1</Link></li>
                </ul>
            </React.Fragment>
        )
    }
}
export default News;
```
```
// New文件夹下details.js
import React from 'react';
class NewsDetails extends React.Component {
    render() {
        return (          
            <React.Fragment>
                 <div>新闻详情1页面</div>
            </React.Fragment>
        )
    }
}
export default NewsDetails;
```
**`router` 配置文件 `router.js`代码如下:**
```
import React from 'react';
import {BrowserRouter as Router, Route,Switch} from 'react-router-dom';
import IndexPage from "./pages/Index";
import NewsPage from './pages/News';
import NewsDetails from './pages/News/details'
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Route path="/" exact component={IndexPage}></Route>
              <Route path="/news" exact component={NewsPage}></Route>
              <Route path="/news/details" exact component={NewsDetails}></Route>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
**此时已经可以完成简单的路由跳转:**
![](https://gitee.com/zuo_jiahui/blogimage/raw/master/img/5ab8e180349328960ed54a43ff67140.png)
### 路由传参
#### 方法一 : params
**通过`params`接受到传递过来的参数**
**路由表中 :**
```
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import IndexPage from "./pages/Index";
import NewsPage from './pages/News';
import NewsDetails from './pages/News/details'
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Route path="/" exact component={IndexPage}></Route>
              <Route path="/news" exact component={NewsPage}></Route>
              <!-- 这里 id title 为需要接收的参数 -->
              <Route path="/news/details/:id/:title" exact component={NewsDetails}></Route>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
**Link处 :**
```
import React from 'react';
import { Link } from 'react-router-dom';
class News extends React.Component {
    render() {
        return (
            <React.Fragment>
                <div>我是新闻页面</div>
                <ul>
                     <!-- 1 新闻详情1为传递的参数 -->
                    <li><Link to="/news/details/1/新闻详情1">新闻详情1</Link></li>
                </ul>
            </React.Fragment>
        )
    }
}
export default News;
```
**sort页面 :**
```
import React from 'react';
class NewsDetails extends React.Component {
    constructor(props) {
        super(props);
        this.state = {};
        console.log("id:" + props.match.params.id,)
    }

    //使用 this.props.match.params 就可以获取到传递过来的参数（ id title ）

    componentDidMount() {  // 组件生命钩子函数，组件已经加载完成
        console.log("title:" + this.props.match.params.title)
    }
    render() {
        return (
            <div>新闻详情1页面</div>
        )
    }
}
export default NewsDetails;
```
#### 方法二 : query
**通过`query`接受到传递过来的参数**
**前提：**不能刷新页面,必须由其他页面跳过来，参数才会被传递过来
**注：**不需要配置路由表。路由表中的内容照常
**路由表中 :**
```
import React from 'react';
import {BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import IndexPage from "./pages/Index";
import NewsPage from './pages/News';
import NewsDetails from './pages/News/details'
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Route path="/" exact component={IndexPage}></Route>
              <Route path="/news" exact component={NewsPage}></Route>
              {/* 不需要配置路由表,路由表中的内容照常 */}
              <Route path="/news/details" exact component={NewsDetails}></Route>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
**Link处 :**
```
import React from 'react';
class News extends React.Component {
    render() {
        return (
            <React.Fragment>
                <div>我是新闻页面</div>
                <ul>
                    <li onClick={() => {
                        this.props.history.push({
                            pathname: "/news/details",
                            search:"?id=1&title=新闻详情1",
                            query: {
                                id: 1,
                                title: "新闻详情1"
                            }
                        })
                    }}>新闻详情1</li>
                </ul>
            </React.Fragment>
        )
    }
}
export default News;
```
**sort页面 :**
```
import React from 'react';
class NewsDetails extends React.Component {
    constructor(props) {
        super(props);
        this.state = {};
    }
    componentDidMount() {  // 组件生命钩子函数，组件已经加载完成
        // 使用 this.props.location.query  就可以获取到传递过来的参数（ id title ）
        console.log(this.props.location.query.id )
        console.log(this.props.location.query.title )
    }
    render() {
        return (
        <div>新闻详情1页面,id:{this.props.location.query.id }</div>
        )
    }
}
export default NewsDetails;
```
#### 方法三 : 自定义函数
**通过自定义函数接受到传递过来的参数**
**在 utils 文件夹下建立一个 js 文件，内容如下 :**
```
export function localParam (search, hash) {
    search = search || window.location.search;
    hash = hash || window.location.hash;
    var fn = function(str, reg) {
        if (str) {
            var data = {};
            str.replace(reg, function($0, $1, $2, $3) {
                data[$1] = $3;
            });
            return data;
        }
    };
    return {
            search : fn(search, new RegExp("([^?=&]+)(=([^&]*))?", "g")) || {},
            hash : fn(hash, new RegExp("([^#=&]+)(=([^&]*))?", "g")) || {}
        };
    }

```
**路由表中 :**
```
import React from 'react';
import {BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import IndexPage from "./pages/Index";
import NewsPage from './pages/News';
import NewsDetails from './pages/News/details'
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Route path="/" exact component={IndexPage}></Route>
              <Route path="/news" exact component={NewsPage}></Route>
              {/* 不需要配置路由表,路由表中的内容照常 */}
              <Route path="/news/details" exact component={NewsDetails}></Route>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
**Link处 :**
```
import React from 'react';
class News extends React.Component {
    render() {
        return (
            <React.Fragment>
                <div>我是新闻页面</div>
                <ul>
                    <li onClick={() => {
                        this.props.history.push("/news/details?id=1&title=新闻详情1")
                    }}>新闻详情1</li>
                </ul>
            </React.Fragment>
        )
    }
}
export default News;
```
**sort页面 :**
```
import React from 'react';
// 引入自定义方法
import { localParam } from '../../utils'
class NewsDetails extends React.Component {
    constructor(props) {
        super(props);
        this.state = {};
    }
    componentDidMount() {  // 组件生命钩子函数，组件已经加载完成
        console.log("id ：" + localParam(this.props.location.search).search.id);
        // 如是中文，则会出现乱码
        console.log("title ：" + localParam(this.props.location.search).search.title) 
        // 如果想转回中文使用 decodeURIComponent 解码
        console.log("title" + decodeURIComponent(localParam(this.props.location.search).search.title));
    }
    render() {
        return (
            <React.Fragment>
                <div>新闻详情1页面</div>
                <ul>
                    <li>{localParam(this.props.location.search).search.id}</li>
                    <li>{decodeURIComponent(localParam(this.props.location.search).search.title)}</li>
                </ul>
                {/* （-1）返回上一级路由 */}
                <button type={"button"} onClick={() => { this.props.history.go(-1) }}>返回</button>
            </React.Fragment>

        )
    }
}
export default NewsDetails;
```
### 路由懒加载
#### 方法一 : 自定义组件
**制作异步函数组件**
**建立AsynComponent.js文件，内容如下：**
```
import React, { Component } from "react";
export default function asyncComponent(importComponent) {
    class AsyncComponent extends Component {
        constructor(props) {
            super(props);

            this.state = {
                component: null
            };
        }
        async componentDidMount() {
            const { default: component } = await importComponent();
            this.setState({
                component: component
            });
        }
        render() {
            const C = this.state.component;
            return C ? <C {...this.props} /> : null;
        }
    }
    return AsyncComponent;
}
```
**路由表中 :**
```
import React from 'react';
import {BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import asyncComponent from './assets/components/async/asyncComponent';  // 引入懒加载组件
// 激活chunkFilename,默认不激活(路由懒加载)
const  IndexPage = asyncComponent(() =>import("./pages/Index"));
const  NewsPage = asyncComponent(() =>import("./pages/News"));
const  NewsDetails = asyncComponent(() =>import("./pages/News/details"));
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Route path="/" exact component={IndexPage}></Route>
              <Route path="/news" exact component={NewsPage}></Route>
              <Route path="/news/details" exact component={NewsDetails}></Route>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
#### 方法二 : lazy和suspense
**路由表中 :**
```
import React, { lazy, Suspense } from 'react';      //导入 lazy, Suspense 
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
const IndexPage = lazy(() => import("./pages/Index"));
const NewsPage = lazy(() => import("./pages/News"));
const NewsDetails = lazy(() => import("./pages/News/details"));
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              {/* 用 Suspense 包裹 Route */}
              <Suspense fallback={<React.Fragment />}>
                <Route path="/" exact component={IndexPage}></Route>
                <Route path="/news" exact component={NewsPage}></Route>
                <Route path="/news/details" exact component={NewsDetails}></Route>
              </Suspense>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
### 路由嵌套
#### 路由嵌套(主子路由)
**路由表中 :**
```
import React, { lazy, Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
const IndexPage = lazy(() => import("./pages/Index"));
const GoodssPage = lazy(() => import("./pages/Home"));
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Suspense fallback={<React.Fragment />}>
                <Route path="/" exact component={IndexPage}></Route>
                {/* 这里不需要添加 exact */}
                <Route path="/goods" component={GoodssPage}></Route>
              </Suspense>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
**sort页面 :**
```
import React, { lazy, Suspense } from 'react';
import { Route, Switch, Redirect } from 'react-router-dom';
const GoodsItem = lazy(() => import("./item"))
const GoodsDetails = lazy(() => import("./details"))
const GoodsEvaluation = lazy(() => import("./evaluation"))
class Index extends React.Component {
    goPage(url) {
        // 这里使用 replace 方法，不采用 push
        this.props.history.replace(url);
    }
    render() {
        return (
            <React.Fragment>
                <ul>
                    <li onClick={this.goPage.bind(this, '/goods/item')}>商品</li>
                    <li onClick={this.goPage.bind(this, '/goods/details')}>详情</li>
                    <li onClick={this.goPage.bind(this, '/goods/evaluation')}>评价</li>
                </ul>
                <button type="button" onClick={this.props.history.go.bind(this, -1)}>返回</button>
                <div>
                    <Switch>
                        <Suspense fallback={<React.Fragment />}>
                            {/* 这里不需要添加 exact */}
                            <Route path="/goods/item" component={GoodsItem}></Route>
                            <Route path="/goods/details" component={GoodsDetails}></Route>
                            <Route path="/goods/evaluation" component={GoodsEvaluation}></Route>
                            {/* 路由重定向 : 需要要放在最下面 ，先有 path*/}
                            <Redirect to="/goods/item"></Redirect>
                        </Suspense>
                    </Switch>
                </div>
            </React.Fragment>
        )
    }
}
export default Index;
```
#### 子组件路由跳转
**withRouter实现子组件路由跳转**
**sort页面 :**
```
import React, { lazy, Suspense } from 'react';
import { Route, Switch, Redirect } from 'react-router-dom';
import GoodsNav from "../../assets/components/GoodsNav"      //引入 GoodsNav 组件
const GoodsItem = lazy(() => import("./item"))
const GoodsDetails = lazy(() => import("./details"))
const GoodsEvaluation = lazy(() => import("./evaluation"))
class Index extends React.Component {
    render() {
        return (
            <React.Fragment>
                {/* GoodsNav子组件 */}
                <GoodsNav></GoodsNav>
                <button type="button" onClick={this.props.history.go.bind(this, -1)}>返回</button>
                <div>
                    <Switch>
                        <Suspense fallback={<React.Fragment />}>
                            {/* 这里不需要添加 exact */}
                            <Route path="/goods/item" component={GoodsItem}></Route>
                            <Route path="/goods/details" component={GoodsDetails}></Route>
                            <Route path="/goods/evaluation" component={GoodsEvaluation}></Route>
                            {/* 路由重定向 : 需要要放在最下面 ，先有 path*/}
                            <Redirect to="/goods/item"></Redirect>
                        </Suspense>
                    </Switch>
                </div>
            </React.Fragment>
        )
    }
}
export default Index;
```


**新建一个GoodsNav组件 :**
```
import React from 'react';
import { withRouter } from 'react-router-dom'    //引入 withRouter
function GoodsNav(props) {
    const goPage = (url) => { 
        console.log(this,props,url)   //这里没有 this : undefined
        props.history.replace(url)
    };
    return (
        <ul>
            <li onClick={goPage.bind(null, '/goods/item')}>商品</li>
            <li onClick={goPage.bind(null, '/goods/details')}>详情</li>
            <li onClick={goPage.bind(null, '/goods/evaluation')}>评价</li>
        </ul>
    )
}
export default withRouter(GoodsNav);
```
### 路由认证
**制作会员认证路由 :**
```
//在routes/private.js里面的代码
import React from 'react';
import {Route,Redirect} from 'react-router-dom';
export function AuthRoute({ component:Component, ...rest }) {
    return (
        <Route {...rest} render={props =>
                Boolean(localStorage['isLogin']) ? (
                    <Component {...props} />
                ) : (
                    <Redirect
                        to={{
                            pathname: "/login",
                            state: { from: props.location }
                        }}
                    />
                )
            }
        />
    );
}
```
**建立首页 :**
```
import React from 'react';
import {Link} from 'react-router-dom';
class IndexPage extends React.Component{
    render() {
        return(
            <React.Fragment>
                <div>首页</div>
                <ul>
                    <li><Link to="/login">会员登录</Link></li>
                    <li><Link to="/user">会员中心</Link></li>
                </ul>

            </React.Fragment>
            
        )
    }
}
export default IndexPage
```
**建立会员登录 :**
```
import React from 'react';
class Login extends React.Component {
    constructor() {
        super();
        this.state={
            username: "",
            password: "",
        }
    }
    doLogin() {
        if(this.state.username.match(/^\s*$/)) {
            alert("请输入用户名");
            return;
        }
        if(this.state.password.match(/^\s*$/)) {
            alert("请输入用密码");
            return;
        }
        // 登录成功将username进行存储，并返回上级
        localStorage['username'] = this.state.username;
        localStorage['isLogin'] = true;
        this.props.history.go(-1);
    }
    render() {
        return (
            <React.Fragment>
                     <div>
                         用户名 : <input type="text" placeholder="请输入用户名" onClick={(e)=>{
                             this.setState({username : e.target.value})
                         }}/>
                         <br/>
                         密码 : <input type="text" placeholder="请输入密码" onClick={(e)=>{
                             this.setState({password: e.target.value})
                         }}/>
                     </div>
                     <button type="button" onClick={this.doLogin.bind(this)}>登录</button>
            </React.Fragment>
       
        )
    }
}
export default Login;
```
**建立会员中心页面 :**
```
import React from 'react';
class User extends React.Component {
    outLogin() {
        // 退出清空数据
        localStorage.clear();
        this.props.history.replace("/login")
    }
    render() {
        return(
            <React.Fragment>
            <div>欢迎{localStorage['username']} 回来！</div>
            <button type="button" onClick={this.outLogin.bind(this)}>安全退出</button>
     
            </React.Fragment>
   )
    }
}
export default User;
```
**路由表中 :**
```
import React, { lazy, Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
// 引入路由认证 js 
import { AuthRoute } from "./utils/router/private"
const IndexPage = lazy(() => import("./pages/Index"));
const LoginPage = lazy(() => import("./pages/login"));
const UserPage = lazy(() => import("./pages/user"));
class RouterComponent extends React.Component {
  render() {
    return (
      <React.Fragment>
        <Router>
          <React.Fragment>
            <Switch>
              <Suspense fallback={<React.Fragment />}>
                <Route path="/" exact component={IndexPage}></Route>
                <Route path="/login" exact component={LoginPage}></Route>
                {/* 将 Route 更换成 AuthRoute*/}
                <AuthRoute path="/user" exact component={UserPage}></AuthRoute>
              </Suspense>
            </Switch>
          </React.Fragment>
        </Router>
      </React.Fragment>
    )
  }
}
export default RouterComponent;
```
