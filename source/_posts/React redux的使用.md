---
title: React redux的使用
categories: React
tags:
  - redux
abbrlink: 57455
date: 2020-11-08 21:35:03
---
## 前言
Redux 是 JavaScript 状态容器，提供可预测化得状态管理，可以跨组件、跨页面推送数据。应用场景如 : 购物车、会员登录等功能模块。Redux 由 Flux 演变而来  
**存储流程:**
Redux 的基本思想是整个应用的 state 保持在一个单一的 store 中，store 是简单的 JavaScript 对象，而改变应用的 start 的唯一方式是在应用中触发 actions，然后为这些 actions 编写 reducers来修改 state。整个 state 转化是在 reducers 中完成  

**流程可分为4大部:**
**1. 选购商品**
**2. 商品装车**
**3. 存入仓库**
**4. 从仓库获取商品**
## 安装 redux
```
npm install redux --save
npm install react-redux --save
```
<font size=5>计数器案例:</font>

在 src 新建一个 store 文件, 再次文件下分别建立 actions reducers 文件
子组件 :
```
import React from 'react';
class Couter extends React.Component {
    render() {
        return(
            <React.Fragment>
                <div>
                    子组件计数器 : 0
                </div>
            </React.Fragment>
        )
    }
}
export default Couter;
```
## 选购商品
```
import React from 'react'
// 关联仓库，将组件进行包裹起来，此时该组件就有 redux 的属性
import {connect} from 'react-redux'
import Conter from "../../components/counter"
import actions from '../../store/actions';
class IndexPage extends React.Component {
    constructor() {
        super();
        this.num = 0 
    }
    inCount() {     //  点击 ++
        // 1. 选购商品 this.props.dispatch
        //  使用 dispatch 属性触发 actions，  其中 type 为必填项,其他的属性可以自行添加 
        //  this.props.dispatch({type: "INC", data: {count: ++this.num}})   //正常写法,推荐使用下面采取模块化
        // 调用 actions.counter.incCount 方法 并传递数据
        this.props.dispatch(actions.counter.incCount({count: ++this.num}))
    }
    deCount() {      //点击 -- 
        this.props.dispatch(actions.counter.decCount({count: --this.num}))
    }
    render() {
        return (
            <React.Fragment>
                {/* 子组件 */}
                <Conter />
                <div> 
                    计数器 : <button type="button" onClick={this.deCount.bind(this)}>-</button> 0 <button onClick={this.inCount.bind(this)}>+</button>
                </div>
            </React.Fragment>
        )
    }
}
export default connect()(IndexPage);
```
**actions 文件下建立 index.js  和 counter.js**
**counter.js文件**
```
export function incCount(data) {
    return {
        type: "INC",
        data   // data:data
    }
}
export function decCount(data) {
    return {
        type: "DEC",
        data
    }
}
```
**index.js文件**
```
// import {decCount, decCount} from './couter' 导入 decCount decCount方法
import * as counter from './counter'    //导入 counter文件中所有的方法
export default {
    counter,    //counter: counter
}
```
## 商品装车
**reducers 文件下建立 counter.js**
**counter.js文件**
```
//2. 商品装车
let defaultState = { 
  count: 0
}
// state : 数据源       action : 获取 dispatch 的值
function counterReducer(state = defaultState, action) {
  switch (action.type) {
    case "INC":
      return { ...state, ...action.data };              //写法一  常用
    case "DEC":
      return Object.assign({}, state, action.data)      //写法二
    default:
      return state;    //必须的返回 state， 否则会出错
  }
}
export default counterReducer;
```
## 存入仓库
**reducers 文件下建立 index.js**
**index.js文件**
```
// 引入 createStore 仓库，为 store 的创建做准备  combineReducers用于组合关联使用
import { createStore, combineReducers } from 'redux';
import CounterReducer from './counter';
// 3.创建仓库， 将reducers存储到仓库(存放数据)
// let store = createStore(CounterReducer)   传入单个方法
let store = createStore(combineReducers({   //传入多个个方法
    counter: CounterReducer,
  }))
  export default store;
```
**在入口index中添加 store**
```
import React from 'react';
import ReactDOM from 'react-dom';
import RouterComponent from './router';
// 用于读取数据 
import { Provider } from 'react-redux'
import store from './store/reducers'
import * as serviceWorker from './serviceWorker';
function App() {
  return (
    <React.Fragment>
      {/* 使用 Provider, 添加上 store 属性*/}
      <Provider store={store}>
        <RouterComponent />
      </Provider>
    </React.Fragment>
  )
}
ReactDOM.render(<App />, document.getElementById('root'));
// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```
## 从仓库获取商品
```
import React from 'react'
// 关联仓库，将组件进行包裹起来，此时该组件就有 redux 的属性
import {connect} from 'react-redux'
import Conter from "../../components/counter"
import actions from '../../store/actions';
class IndexPage extends React.Component {
    constructor() {
        super();
        this.num = 0 
    }
    inCount() {     //  点击 ++
        this.props.dispatch(actions.counter.incCount({count: ++this.num}))
    }
    deCount() {      //点击 -- 
        this.props.dispatch(actions.counter.decCount({count: --this.num}))
    }
    render() {
        return (
            <React.Fragment>
                {/* 子组件 */}
                <Conter />
                <div> 
                    {/* 4. 从仓库获取商品 */}
                    计数器 : <button type="button" onClick={this.deCount.bind(this)}>-</button>  {this.props.state.counter.count} <button onClick={this.inCount.bind(this)}>+</button>
                </div>
            </React.Fragment>
        )
    }
}
export default connect((state) =>{  //  state 接收第二步中的 state
    return {
        state : state
    }
})(IndexPage);
```
子组件 :
```
import React from 'react';
import {connect} from 'react-redux'
class Couter extends React.Component {
    render() {
        const {count} = this.props
        return(
            <React.Fragment>
                <div>
                    子组件计数器 : {count}
                </div>
            </React.Fragment>
        )
    }
}
//只有和仓关联起来，才可访问 redux 的属性
export default connect((state) =>({
    count:state.counter.count
}))(Couter);
```
