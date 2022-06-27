---
title: React入门基础
categories: React
abbrlink: 45464
date: 2020-10-26 12:35:03
top: 1
---

### 概念

它是一个将数据渲染为 HTML 视图 的 js 库

#### 原生 js 痛点

用 dom 的 API 去操作 dom，繁琐且效率低  
用 js 直接操作 dom，浏览器会进行大量的回流和重绘  
原生 js 没有组件化的编程方案，代码复用性低,对样式和结构也没办法拆解

#### React 特点

采用组件化模式，声明式编码，提高开发效率和组件复用性  
在 React Native 中可以用 react 预发进行安卓、ios 移动端开发  
使用虚拟 dom 和有些的 diffing 算法，尽量减少与真实 dom 的交互，提高性能

### 模块与组件、模块化与组件化

#### 模块

1. 一般就是指 js 文件
2. 为什么要拆成模块： 随着业务逻辑的增加，代码越来越复杂
3. 作用： 复用 js，简化 js 的编写，提高 js 的工作效率

#### 组件

1. 用来实现局部功能效果的代码和资源的集合
2. 为什么使用组件： 因为一个界面的功能很复杂，需要拆分
3. 作用： 复用编码，简化项目编码，提高运行效率

#### 模块化

当应用的 js 都是已模块的方式来编写的时候，那么这个应用就是模块化的应用

#### 组件化

当应用是已多组件的方式实现的，那么这个应用就是组件化的应用

### 组件的使用

React 通过组件的思想，将界面拆分成一个个可复用的模块，每一个模块就是一个 React 组件。一个 React 应用由若干组件组合而成，一个复杂组件也可以由若干简单组件组合而成

#### 创建组件方式

组件是由元素构成的。元素数据结构是普通对象，而组件数据结构是类或纯函数  
定义组件的两个要求:

1. 组件名称必须以大写字母开头
2. 组件的返回值只能有一个根元素

#### 函数组件

1. 就是一个函数
2. 没有生命周期函数
3. 不能使用 state,只会接收一个 props 形参,并且直接使用 props 参数不需要 this
4. 无状态组件就是一个简单的视图函数，没有业务逻辑更纯粹的展示 UI

**代码示例:**

```
function Welcome() {
  console.log(this)    // undefined
  // bable 中开启严格模式
  return (
    <div>无状态组件 ( 适用于 简单 组件的定义 无state  状态 ）)</div>
  )
}
```

#### 类式组件

1. 是一个 class 类，继承了类的组件是类式组件,继承自 React.Component 类
2. 类式组件必须拥有继承 extends React.Component 和 render 属性
3. 可以使用 state 和 props,并且都是使用 this 来调用 this.state 或 this.props
4. 拥有生命周期函数
5. 类式组件可以使用生命周期可以在里面写业务逻辑，可以在里面做任何事情

**代码示例:**

```
class Welcome extends React.Component {
  render() {
    console.log(this)    // Welcome组件的实例对象
    return (
      <div>类式组件 ( 适用于 复杂 组件的定义 有state（ 状态 ）)</div>
    )
  }
}
```

### 事件处理

1. 通过 onXxxx 属性指定事件处理函数（小驼峰形式）
2. 通过 event.target 可以得到发生事件的 Dom 元素
3. 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串

### 组件的三大属性

#### state

1. state 是组件实例对象最重要的属性，必须是对象的形式
2. 组件被称为状态机，通过更改 state 的值来达到更新页面显示（重新渲染组件）
3. 组件 render 中的 this 指的是组件实例对象
4. 状态数据不能直接赋值，需要用 setState（）
5. 组件自定义方法中的 this 为 undefined, 怎么解决？
   将自定义函数改为表达式+箭头函数的形式（推荐）  
   在构造器中用 bind（）强制绑定 this

```
import React, { Fragment } from "react";
class Welcome extends React.Component {
  constructor(props) {
    // constructor 调用次数 1   页面初始化
    super(props);
    this.state = {
      isHot: false,
    };
  }
  change() {
    // 状态的 (state) 不可直接修改，必须痛过 this.setState 方法
    this.setState({ isHot: !this.state.isHot });
  }
  render() {
    // render调用次数  1 + n    1: 页面初始化执行   n: 状态更新次数
    const {
      state: { isHot },
      change,
    } = this;
    return (
      <Fragment>
        <div onClick={change.bind(this)}>
          今天的天气{isHot ? "凉爽" : "炎热"}
        </div>
      </Fragment>
    );
  }
}
export default Welcome;
```

精简版：

```
import React, { Fragment } from "react";
class Welcome extends React.Component {
  state = {
    isHot: false,
    num: 0,
  };

  change = () => {
    //结果还是之前的，而不是+1之后的
    // this.setState是异步的  在你调用了this.setState后在他的下面输出他的结果还是没变的状态
    this.setState({ isHot: !this.state.isHot, num: this.state.num + 1 });
    console.log(this.state.num, "执行1");
  };

  addNum = () => {
    this.setState((data) => {
      // this.setState的第一个参数可以是一个对象，也可以是一个函数返回一个对象，函数的参数是上一次的state
      console.log(data);
      return { num: data.num + 1 };
    });
  };

  reduce = () => {
    //this.setState的第二个参数是它的回调函数，在前面重新给state赋值后执行
    this.setState({ num: this.state.num - 1 }, () => {
      console.log(this.state.num);
    });
  };

  render() {
    console.log(this.state.num, "执行2");
    const {
      state: { isHot },
      change,
      addNum,
      reduce,
    } = this;
    return (
      <Fragment>
        <div onClick={change}>今天的天气{isHot ? "凉爽" : "炎热"}</div>
        <button onClick={addNum}>加➕</button>
        <button onClick={reduce}>减➖</button>
      </Fragment>
    );
  }
}
export default Welcome;
```

#### props

props 就是在调用组件的时候在组件中添加属性传到组件内部去使用

```
import React, { Fragment } from "react";
import PropTypes from "prop-types";
class Person extends React.Component {
  // 类型检测
  static propTypes = {
    // array : 数组类型
    // bool : 布尔类型
    // func : 函数
    // number : 数字
    // object : 对象
    // string : 字符串
    // symbol : ES6新增的symbol类型

    name: PropTypes.string.isRequired, // 必须传递字符串类型
    sex: PropTypes.string, // 字符串类型
    age: PropTypes.number, // 字符串类型
  };

  // 默认值
  static defaultProps = {
    sex: "女",
    age: 18,
  };

  render() {
    // props 是只读的，不可修改
    const { name, age, sex } = this.props;
    return (
      <Fragment>
        <div>姓名: {name}</div>
        <div>年龄: {age}</div>
        <div>性别: {sex}</div>
      </Fragment>
    );
  }
}
class Welcome extends React.Component {
  render() {
    const a = { name: "猪猪" };
    const b = { name: "粥粥", age: 23, sex: "男" };
    return (
      <Fragment>
        <Person {...a} />
        <Person {...b} />
      </Fragment>
    );
  }
}
export default Welcome;

```

#### refs

refs 是组件实例对象中的属性，它专门用来收集那些打了 ref 标签的 dom 元 素

```
import React, { Fragment, createRef } from "react";
class Welcome extends React.Component {
  myRef = createRef();

  showData = () => {
    // 不推荐使用 字符串 形的 ref, (效率不高)
    const { input1 } = this.refs;
    alert(input1.value);
  };

  showData2 = () => {
    // 回调形式的 ref
    // 如果 ref 回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数 null，然后第二次会传入参数 DOM 元素
    const { input2 } = this;
    alert(input2.value);
  };

  showData3 = () => {
    // React.createRef   （推荐使用）
    // 调用后可以返回一个容器，该容器可以存储被 ref 所表标识的节点， 该容器是 “专人专用” 的
    const { value } = this.myRef.current;
    alert(value);
  };

  render() {
    const { showData, showData2, myRef, showData3 } = this;
    return (
      <Fragment>
        <input ref="input1" placeholder="点击按钮提示" /> &nbsp;
        <button onClick={showData}>点击提示</button>&nbsp;
        <input
          ref={(current) => (this.input2 = current)}
          onBlur={showData2}
          placeholder="失去焦点提示"
        />
        &nbsp;
        <input ref={myRef} onBlur={showData3} placeholder="失去焦点提示" />
      </Fragment>
    );
  }
}
export default Welcome;

```

### 高阶函数和函数柯里化

高阶函数：如果一个函数符合下面的 2 个规范的任何一个，那么改函数就是高阶函数

1. 若 A 函数，接受的参数是一个函数，那么 A 就是高阶函数
2. 若 A 函数， 调用的返回值依然是一个函数，那么 A 就是高阶函数  
   常见的高阶函数： Promise map setTimeOut

函数柯里化：通过函数的回调继续返回函数的方式， 实现多次接受函数最后统一处理的函数编码形式

```
import React, { Fragment } from "react";
class Welcome extends React.Component {
  state = {
    name: "",
    age: "",
  };
  save = (type) => {
    return (e) => this.setState({ [type]: e.target.value });
  };
  render() {
    return (
      <Fragment>
        <input onChange={this.save("name")} /> &nbsp;
        <input onChange={this.save("age")} />
      </Fragment>
    );
  }
}
export default Welcome;

```

### 组件的生命周期

**旧的生命周期 和 新的生命周期区别**
**旧的生命周期**即将废弃 3 个勾子函数:  
componentWillMount  
componentWillReceiveProps  
componentWillUpdate
**新的生命周期**新增 2 个勾子函数
getDerivedStateFromProps
getSnapshotBeforeUpdate

#### 旧版本生命周期

<div  align="center">    
<img src="https://gitee.com/zuo_jiahui/blogimage/raw/b4f9f5540825aef435c0f17c818738cfcb22a8cb/img/20211209095419.png" width = 100% height = 100 />
</div>

**生命周期的三个阶段**

1. 初始化阶段：由 ReactDom.render () 触发 - - - 初次渲染  
   constructor ()  
   componentWillMount ()  
   render ()  
   **componentDidMount () 常用**
2. 更新阶段：由组件内部的 this.setState () 或 render () 触发
   componentWillReceiveProps ()
   shouldComponentUpdate ()
   componentWillUpdate ()
   **render () 必用**
   componentDidUpdate ()
3. 卸载阶段： 由 ReactDom.unmountComponentAtNode () 触发
   **componentWillUnmount () 常用**

```
import React, { Fragment } from "react";
import ReactDOM from "react-dom";
class Demo extends React.Component {
  componentWillReceiveProps(props) {
    console.log(props);
  }
  render() {
    return <div>{this.props.conut}</div>;
  }
}
class Welcome extends React.Component {
  // 挂载 constructor  componentWillMount  render  componentDidMount
  // 构造器
  constructor(prpos) {
    console.log("第 1 执行");
    super(prpos);
    this.state = {
      conut: 0,
      num: 1,
    };
    this.num = 1;
  }
  // 组件挂将要挂在
  componentWillMount() {
    console.log("第 2 执行");
  }
  // 组件挂在完毕
  componentDidMount() {
    console.log("第 4 执行");
  }

  // 组件更新 componentWillReceiveProps shouldComponentUpdate render componentWillUpdate componentDidUpdate
  // 控制组件是否被更新
  shouldComponentUpdate() {
    console.log("更新 1 执行");
    const { conut } = this.state;
    if (conut > 3) {
      return false;
    }
    return true;
  }
  // 组件将要更新
  componentWillUpdate() {
    console.log("更新 2 执行");
  }
  // 组件更新完毕
  componentDidUpdate() {
    console.log("更新 4 执行");
  }
  // 组件将要卸载执行
  componentWillUnmount() {
    console.log("卸载 1 执行");
  }
  // 初始渲染 状态更新 执行
  render() {
    console.log("第 3 执行", "更新 3 执行");
    const {
      state: { conut },
    } = this;

    return (
      <Fragment>
        <h2>当前求和为：{conut}</h2>
        <button onClick={() => this.setState({ conut: conut + 1 })}>
          点我 + 1
        </button>
        <button
          onClick={() => {
            // 强制更新
            this.state.conut += 1;
            this.forceUpdate();
          }}
        >
          强制更新
        </button>
        <Demo conut={conut} />
        <button
          onClick={() => {
            ReactDOM.unmountComponentAtNode(document.getElementById("root"));
          }}
        >
          卸载组件
        </button>
      </Fragment>
    );
  }
}
export default Welcome;

```

#### 新版本生命周期

<div  align="center">
<img src="https://gitee.com/zuo_jiahui/blogimage/raw/master/img/20220107215836.png" width = 100% height = 100 />
</div>

**生命周期的三个阶段**

1. 初始化阶段：由 ReactDom.render () 触发 - - - 初次渲染  
   constructor ()  
   getDerivedStateFromProps ()
   render ()  
   **componentDidMount () 常用**
2. 更新阶段：由组件内部的 this.setState () 或 render () 触发
   getDerivedStateFromProps ()
   shouldComponentUpdate ()
   **render () 必用**
   getSnapshotBeforeUpdate ()
   componentDidUpdate ()
3. 卸载阶段： 由 ReactDom.unmountComponentAtNode () 触发
   **componentWillUnmount () 常用**

```
import React, { Fragment } from "react";
import ReactDOM from "react-dom";
class Welcome extends React.Component {
  // 挂载 constructor getDerivedStateFromProps  render  componentDidMount
  // 构造器
  constructor(prpos) {
    console.log("第 1 执行");
    super(prpos);
    this.state = {
      conut: 0,
      num: 1,
    };
    this.num = 1;
  }

static getDerivedStateFromProps(props, state) {
console.log("第 2 执行", "更新 1 执行");
return null;
}
// 组件挂在完毕
componentDidMount() {
console.log("第 4 执行");
}

// 组件更新 getDerivedStateFromProps shouldComponentUpdate render getSnapshotBeforeUpdate componentDidUpdate
// 控制组件是否被更新
shouldComponentUpdate() {
console.log("更新 2 执行");
const { conut } = this.state;
if (conut > 3) {
return false;
}
return true;
}
getSnapshotBeforeUpdate() {
console.log("更新 4 执行");
return null;
}
// 组件更新完毕
componentDidUpdate() {
console.log("更新 5 执行");
}
// 组件卸载 componentWillUnmount
// 组件将要卸载执行
componentWillUnmount() {
console.log("卸载 1 执行");
}
// 初始渲染 状态更新 执行
render() {
console.log("第 3 执行", "更新 3 执行");
const {
state: { conut },
} = this;

    return (
      <Fragment>
        <h2>当前求和为：{conut}</h2>
        <button onClick={() => this.setState({ conut: conut + 1 })}>
          点我 + 1
        </button>
        <button
          onClick={() => {
            // 强制更新
            this.state.conut += 1;
            this.forceUpdate();
          }}
        >
          强制更新
        </button>
        <button
          onClick={() => {
            ReactDOM.unmountComponentAtNode(document.getElementById("root"));
          }}
        >
          卸载组件
        </button>
      </Fragment>
    );

}
}
export default Welcome;

```

### diff 算法

计算出虚拟 DOM 中真正变化的部分，并只针对该部分进行原生 DOM 操作，而非重新渲染整个页面
**react/vue 中的 key 有什么作用：** 当状态中的数据发生改变时，react 会根据【新数据】生成【新虚拟 DOM】，随后 react 会进行【新虚拟 DOM】和【旧虚拟 DOM】的 diff 算法比较，具体的比较规则如下：

1. 若【旧 DOM】中找到了与【新 DOM】相同的 key，则会进一步判断两者的内容是否相同，如果也一样，则直接使用之前的真实 DOM，如果内容不一样，则会生成新的真实 DOM，替换掉原先的真实 DOM
2. 若【旧 DOM】中没找到与【新 DOM】相同的 key，则直接生成新的真实 DOM，然后渲染到页面

**用 index 作为 key 可能引发的问题：**

3. 若对数据进行：逆序添加、逆序删除等破坏顺序的操作时会产生不必要的真实 DOM 更新，造成效率低下
4. 如果结构中还包含输入类的 dom，会产生错误 dom 更新，出现界面异常

**开发中如何选择 key：**  
5. 最好选中标签的唯一标识 id、手机号等  
6. 如果只是简单的展示数据，用 index 也是可以的

### 脚手架

#### 安装脚手架

使用 create-react-app（脚手架工具）创建一个初始化项目

1. 下载脚手架工具：npm i -g create-react-app
2. 创建应用项目：create-react-app my-app
3. 运行应用：cd my-app（进入应用文件夹），npm start（启动应用）

#### React 脚手架配置代理

##### 方法一

**在 package.json 中追加如下配置**

```
"proxy":"http://localhost:5000"

```

1、优点：配置简单，前端请求资源可以不加任何前缀  
2、缺点：不能配置多个代理（如果请求的不同服务器就不行）  
3、工作方式：当请求了自身 3000 端口不存在的资源时，那么会转发给 5000 端口（优先会匹配自身的资源，如果自己有就不会请求 5000 端口了）

##### 方法二

**在 src 下创建配置文件：src/setupProxy.js**

```
const proxy = require("http-proxy-middleware");
module.exports = function (app) {
  app.use(
    proxy("/api", {
      target: "http://localhost:4000/",
      changeOrigin: true,
      pathRewrite: { "^/api": "" },
    }),
    proxy("/api2", {
      target: "http://localhost:5000/",
      changeOrigin: true,
      pathRewrite: { "^/api2": "" },
    })
  );
};

```

1、优点：可以配置多个代理，可以灵活控制请求是否走代理
2、缺点：配置繁琐，前端请求资源时必须加前缀

### 消息订阅-发布机制

原先 react 传递数据基本用的是 props，而且只能父组件传给子组件，如果子组件要传数据给父组件，只能先父组件传一个函数给子组件，子组件再调用该方法，把数据作为形参传给父组件，那考虑一个事情，兄弟间组件要如何传递数据呢？这就要引出下面这个消息订阅-发布机制

```
npm install pubsub-js --save
```

使用：

1. 先引入：import PubSub from “pubsub-js”
2. 传递数据方发布：PubSub.publish('消息名',data)
3. 要接收数据方订阅：PubSub.subscribe('消息名'，（data）=>{ console.log(data) })

```
import React, { Fragment } from "react";
import PubSub from "pubsub-js";
class Demo extends React.Component {
  render() {
    return (
      <button
        onClick={() => {
          PubSub.publish("showNum", { num: 1 });
        }}
      >
        点击
      </button>
    );
  }
}
class Content extends React.Component {
  state = {
    num: 0,
  };
  componentDidMount() {
    this.token = PubSub.subscribe("showNum", (_, setObjs) => {
      this.setState(setObjs);
    });
  }
  componentWillUnmount() {
    PubSub.unsubscribe(this.token);
  }
  render() {
    const { num } = this.state;
    return <div>{num}</div>;
  }
}
class App extends React.Component {
  render() {
    return (
      <Fragment>
        <Demo />
        <Content />
      </Fragment>
    );
  }
}
export default App;

```

### redux

#### 学习文档

1. 英文文档: https://redux.js.org/
2. 中文文档: https://redux.org.cn/
3. GitHub 地址: https://github.com/reactis/redux

#### redux 是什么

1. 它是专门做状态管理的 js 库，不是 react 插件库
2. 它可以用在 angular、vue、react 等项目中，但与 react 配合用到最多
3. 作用：集中式管理 react 应用中多个组件共享的状态

#### 什么情况下需要使用它

1. 某个组件的状态需要让其他组件也能拿到
2. 一个组件需要改变另一个组件的状态（通信）
3. 总体原则：能不用就不用，如果不用比较吃力，就可以使用

#### redux 的工作流程

<div  align="center">
<img src="https://gitee.com/zuo_jiahui/blogimage/raw/master/img/20220323221342.png" width = 100% height = 100 />
</div>

#### redux 的三个核心概念

##### action

1. 动作的对象
2. 包含 2 个属性
- 标识属性，值为字符串，唯—，必要属性
- 数据属性，值类型任意，可选属性
3. 栗子：type: { 'ADD', data : { name: "tom", age:18} }

#### reducer
1. 用于初始化状态、加工状态
2. 加工时，根据旧的 state 和 action，产生新的 state 的纯函数

#### store
1. 将 state、 action、 reducer 联系在一起的对象
