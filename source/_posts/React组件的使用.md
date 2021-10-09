---
title: React组件的使用
categories: React
tags:
  - React创建组件
  - React组件传值
abbrlink: 45355
date: 2020-10-25 12:35:03
---
## 组件的意义
React通过组件的思想，将界面拆分成一个个`可复用`的模块，每一个模块就是一个React 组件。一个React 应用由若干组件组合而成，一个复杂组件也可以由若干简单组件组合而成。
## 创建组件方式
组件是由元素构成的。元素数据结构是普通对象，而组件数据结构是类或纯函数。
**定义组件的两个要求:**
  1. 组件名称必须以大写字母开头
  2. 组件的返回值只能有一个根元素

### 1. 无状态组件
1. 就是一个函数
2. 没有生命周期函数
3. 不能使用state,只会接收一个props形参,并且直接使用props参数不需要this
4. 无状态组件就是一个简单的视图函数，没有业务逻辑更纯粹的展示UI
**代码示例:**

```
function Welcome() {
  return (
    <div>无状态组件</div>
  )
}
```
### 2. 有状态组件
1. 是一个class类，继承了类的组件是有状态组件,继承自Component类
2. 可以使用state和props,并且都是使用this来调用this.state或this.props
3. 拥有生命周期函数
4. 有状态组件可以使用生命周期可以在里面写业务逻辑，可以在里面做任何事情

**代码示例:**
```
class Welcome extends React.Component {
  constructor() {
    super();
    this.state = {
      name: "有状态组件"
    }
  };
  componentWillMount() { // 生命钩子函数，此钩子函数可以在这里做一些业务初始化操作，或者设置组件状态。
    this.setState({ name: "你好,我是有状态组件" })
  }
  render() {
    return (
      <div>{this.state.name}</div>
      // 此时页面上的展示的是 : 你好,我是有状态组件
    )
  }
}
```
### 3. 高阶组件
**高阶组件类似高阶函数**  
1. **高阶函数**   
高阶函数就是一个函数，但 `参数` 和 `返回值` 为 `函数` 
```
function hoc() {
    return function (args) {
        console.log(args)
    }
}
hoc()("我是高级函数")
```
2. **高阶组件**  
高阶组件也是一个函数，但 `参数` 和 `返回值`为 `组件`
 **其实就是定义一个函数，里面返回一个有状态组件，就是高阶组件**
高阶组件就像我们吃火锅的锅底，可以在里面加羊肉、牛肉、蔬菜等。锅底相当于业务逻辑，食物相当于UI展示，可以使我们的业务逻辑层和UI层分离，代码更加清晰，更适合多人开发和维护

**高阶组件分为两种**
1. **属性代理方式 :**  
属性代理是最常见的高阶组件的使用方式。他通过一些操作，将包裹的组件的 `props` 和新生成的 `props`一起传递给此组件，这称之为属性代理  
  **特点:** 返回一个全新的组件，不可以获取输入组件的state、生命周期、方法。

```
import React from 'react';
// 属性代理
function Hoc(WithComponent, title) { //WithComponent：接收传过来的组件，title：接收传来的参数
  return class HocComponent extends React.Component {  //return一个有状态组件  继承于React.Component
    render() {
      return (
        <React.Fragment>
          {/* name：传递的参数 */}
          <WithComponent name="我是属性代理高阶组件"></WithComponent>
          <div>{title}</div>
        </React.Fragment>
      )
    }
  }
}
class App extends React.Component {
  constructor() {
    super();
    this.state = {
    }
  };
  render() {
    return (
      // 使用 props 接收传递过来的参数
      <div>{this.props.name}</div>
    )
  }
}
export default Hoc(App, "我是属性代理高阶组件参数"); //App:传递的组件
```
2. **反向继承方式 :**
反向继承返回的React组件继承了被传入的组件，它能够访问的区域、权限更多，相比属性代理方式，它更像打入组织的内部，对其进行修改  
  **特点:** 返回输入组件的子组件，可以获取输入组件的state、生命周期、方法。

```
import React from 'react';
// 反向继承
function Hoc(WithComponent, title) { //WithComponent：接收传过来的组件，title：接收传来的参数
  return class HocComponent extends WithComponent {  //return一个有状态组件  继承传来过来的组件
    render() {
      return (
        <React.Fragment>
          <div>{title}</div>
          {/* 可以访问传递过来组件 state 中的数据 */}
          {this.state.users}
          {/* name：传递的参数 */}
          <WithComponent name="我是反向继承高阶组件"></WithComponent>
        </React.Fragment>
      )
    }
  }
}
class App extends React.Component {
  constructor() {
    super();
    this.state = {
      users: "鸣人"
    }
  };
  render() {
    return (
      // 使用 props 接收传递过来的参数
      <div>{this.props.name}</div>
    )
  }
}
export default Hoc(App, "我是反向继承高阶组件参数"); //App:传递的组件 
```
## 组件之间的传值
### 1. 父向子传值
**无状态组件直接使用props参数不需要this**
**有状态组件必须使用this.props**
```
import React from 'react';
import PropTypes from 'prop-types'     //检测数据类型
// 父组件
class Father extends React.Component {
  constructor() {
    super();
    this.state = {
      name: "小灰灰"
    }
  };
  render() {
    const { name } = this.state
    return (
      <React.Fragment>
        {/* title    isShow    myName父组件要传递给子组件的属性 */}
        <Child title="父组件给子组件传值" isShow={true} myName={name} />
      </React.Fragment>
    )
  }
}
// 子组件
class Child extends React.Component {
  constructor() {
    super();
    this.state = {
    }
  };
  render() {
    const { myName } = this.props
    return (
      <React.Fragment>
        {/* 父组件向子组件传递属性，子组件使用 props 属性接收 */}
        {this.props.title}
        <div style={this.props.isShow ? { display: 'block' } : { display: 'none' }}>我是一个盒子</div>
        <h4>我的名字: {myName}</h4>
      </React.Fragment>
    )
  }
}
//检测属性值的类型
Child.propTypes = {
  title: PropTypes.string.isRequired,  //isRequired：检查是否为必填项
  isShow: PropTypes.bool,
  myName: PropTypes.string,
  // 
}
//默认传递属性值
Child.defaultProps = {
  title: "默认传递"  //如果父组件没有传递title属性，默认传递此属性值
}
export default Father;

```
### 2. 子向父传值
**如果子组件对父组件进行传值，则需要通过事件触发，子组件调用在父组件中的方法，并以传递参数的形式来将子组件中的state传递给父组件**
**父组件:**
```
import React from 'react'
import Son from './son';
class App extends React.Component{
    constructor() {
        super();
        this.state={
        msg:""
        }
    }
    getChildren(val) {
        this.setState({msg:val})

    }
    render() {
        return(
            <div>
                <Son sendParent={this.getChildren.bind(this)} />
                {this.state.msg}
            </div>
        )
    }
}
export default App;
```
**子组件:**
```
import React from 'react';
class Son extends React.Component {
    constructor() {
        super();
        this.state = {
            msg: "我是子组件的值"
        }
    }
    render() {
        return (
            <div>
              <button type="button" onClick={this.props.sendParent.bind(this,this.state.msg)}>请点击我</button>
            </div>
        )
    }
}
export default Son
```
## 组件之间传递方法
### 父向子传递方法
**父组件:**
```
import React from 'react';
import Son from "./son"
class Father extends React.Component {
    constructor() {
        super();
        this.state = {
            msg: "我是父组件中的方法"
        }
    }
    parent() {
        alert(this.state.msg)
    }
    render() {
        return(
            <div>
                <Son parentMethod={this.parent.bind(this)} />
            </div>
        )
    }
}
export default Father
```
**子组件:**
```
import React from 'react';
class Son extends React.Component {
    constructor() {
        super();
        this.state={}
    }
    render() {
        return(
            <div>
                <button onClick={this.props.parentMethod.bind(this)}>点我</button>
            </div>
        )
    }
}
export default Son;
```
### 子向父传递方法
## 案例
### 1. 登录案例
```
import React from "react"
import Hoc from "./proxy"

const Login = Hoc((props) => {     //将此无状态组件传递到./proxy
    console.log({ ...props.username })
    return (
        <React.Fragment>
            {/* 传递过来的参数 */}
            <div>{props.title} {props.id}</div>
            用index户名: <input type="text" placeholder="请输入用户名" {...props.username} /> {props.username.value}
            {props.nulls.isNullusername ? "请输入用户名" : ""}
            <br />
            密码: <input type="password" placeholder="请输入密码" {...props.password} /> {props.password.value}
            {props.nulls.isNullpassword ? "请输入用密码" : ""}
            <br />
            <button type="button" onClick={props.submit.bind(this, () => {
                alert('提交数据')
            })}>登录</button>
        </React.Fragment>

    )
})
class App extends React.Component {
    constructor() {
        super();
        this.state = {

        }
    }
    render() {
        return (
            <React.Fragment>
                <Login title="会员登录" id="1"></Login>
            </React.Fragment>
        )
    }
}
export default App;
```
```
//proxy文件
// 属性代理
import React from "react"
export default function Hoc(WithComponent) {              //接收组件
    return class HocComponent extends React.Component {   //继承于React.Component
        constructor() {
            super();
            this.state = {
                username: "",
                password: "",
                isNullusername: false,
                isNullpassword: false
            }
        }
        setUsername(e) {
            this.setState({ username: e.target.value })
        }
        setPassword(e) {
            this.setState({ password: e.target.value })
        }
        submitData(callback) {     //提交数据
            if(this.state.username.match(/`\s*$/)){
                this.setState({isNullusername:true})
                return;
            }
            if(this.state.username.match(/`\s*$/)){
                this.setState({isNullpassword:true})
                return;
            }
            if(typeof callback==='function') {
                callback();
            }
        }
        render() {
            let newProps = {
                username: { // 用户名
                    onChange: this.setUsername.bind(this),
                    value: this.state.username
                },
                password: { //用户密码
                    onChange: this.setPassword.bind(this),
                    value: this.state.password
                },
                nulls:{  //  显示条件
                    isNullusername: this.state.isNullusername,
                    isNullpassword: this.state.isNullpassword,
                }
            }
            
            return (
                <React.Fragment>
                    {/* <WithComponent title={this.props.title} {...this.props} setUsername={this.setUsername.bind(this)} username={this.state.username}></WithComponent> */}
                    {/*WithComponent 传递过来得组件。  传入的参数过多时，采用扩展运算符无限获取参数 */}
                    <WithComponent  {...this.props} {...newProps} submit={this.submitData.bind(this)}></WithComponent>
                </React.Fragment>
            )
        }
    }
}
```
### 2. 轮播图案例
**父组件 :**
```
//轮播图数据源
import React from 'react'
import Swiper from '../../components/swiper'
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            images: [
                { src: require("../../assets/images/banner1.jpg"), url:"https//baidu.com"},
                { src: require("../../assets/images/banner2.jpg"), url:"https//baidu.com"},
                { src: require("../../assets/images/banner3.jpg"), url:"https//baidu.com"},
                { src: require("../../assets/images/banner3.jpg"), url:"https//baidu.com"},
            ]
        }
    }
    render() {
        return (
            <React.Fragment>
                <Swiper data={this.state.images} />
            </React.Fragment>
        )
    }
}
export default App;
```
```
//轮播图逻辑
import React from 'react';
import Proptypes from 'prop-types';
export default function Hoc(WithCompont) {
    return class HocCompont extends React.Component {
        static propTypes = {  //检测传递过来的数据类型
            data: Proptypes.array.isRequired   //isRequired为必填项
        }
        static defaultProps = { //默认值
            data: []
        }
        constructor(props) {
            super(props);
            this.state = {
                data: []
            }
            this.aData = []
            this.isInit = true
            this.timer = null
            this.index = 0
        }
        change(index) {   //点击切换图片
            this.isInit = false
            this.index = index
            if (this.aData.length > 0) {
                for (let i = 0; i < this.aData.length; i++) {
                    if (this.aData[i].active) {
                        this.aData[i].active = false;
                        break;
                    }
                }
            }
            this.aData[index].active = true;
            this.setState({ data: this.aData })

        }
        autoPlay() {//自动播放
            this.timer = setInterval(() => {
                if (this.aData.length > 0) {
                    this.isInit = false;
                    for (let i = 0; i < this.aData.length; i++) {
                        if (this.aData[i].active) {
                            this.aData[i].active = false;
                            break;
                        }
                    }
                    if (this.index >= this.aData.length - 1) {
                        this.index = 0;
                    } else {
                        this.index++
                        console.log(this.index)
                    }
                    this.aData[this.index].active = true;
                    this.setState({ data: this.aData })
                }

            }, (3000))
        }
        stop() { //鼠标经过清除定时器
            clearInterval(this.timer)
        }
        componentDidMount() {
            this.setState({ data: this.props.data })
            this.autoPlay();  //开启定时器
        }
        componentWillUnmount() {
            clearInterval(this.timer)  //清除定时器

        }
        render() {
            console.log(this.props)   //父组件传递过来的参数
            this.aData = this.props.data;
            if (this.aData && this.aData.length > 0 && this.isInit) {
                for (let i = 0; i < this.aData.length; i++) {
                    if (i == 0) {
                        this.aData[i].active = true;
                    } else {
                        this.aData[i].active = false
                    }
                }
            }
            return (
                //传递参数以及方法
                <WithCompont {...this.props} change={this.change.bind(this)} stop={this.stop.bind(this)} autoPlay={this.autoPlay.bind(this)}></WithCompont>
            )
        }
    }
}
```
```
//轮播图视图层
import React from 'react'
import Hoc from './hoc'
import "./style.css"
// export default function hoc(){
export default Hoc((props) => {
    console.log(props)
    let aData = props.data;
    return (
        <div className="banner">
            <div className="my-swiper-main" onMouseOver={props.stop.bind(this)} onMouseOut={props.autoPlay.bind(this)}>
                {
                    aData.length > 0 && aData.map((item, index) => {
                        return (
                            <div className={item.active ? "my-slide show" : "my-slide"} key={index}>
                                <a href={item.url} target="_blank" rel="noopener noreferrer">
                                    <img src={item.src} alt="" />
                                </a>
                            </div>
                        )
                    })
                }
                <div className="pagination">
                    {
                        aData.length > 0 && aData.map((item, index) => {
                            return (
                                <div className={item.active ? "dot active" : "dot"} key={index} onClick={props.change.bind(this, index)}></div>
                            )
                        })
                    }
                </div>
            </div>
        </div>
    )
})
```



