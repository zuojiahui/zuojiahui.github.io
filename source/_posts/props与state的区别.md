---
title: React中props与state的区别
categories: React
tags:
  - props
  - state
abbrlink: 45355
date: 2020-10-27 11:35:03
---
## props 
主要是用于组件之间传递参数，获取组件的属性值。 组件之间数据单向流动 ，从父组件流向子组件。属性值是无法修改的，它是只读的。
## state
React组件的核心，是数据的来源，主要用于组件更新控制。如果想重新渲染或更新组件，只需要修改`state`即可，然后根据具体修改的`state`,重新渲染用户界面（无需操作DOM对象），可以通过`this.state`来访问它们
## setState
在React中，如果想更新`state`里面的值，必须使用`this.setState( )`方法,从而更新组件的状态，这是一个异步方法，而且有第二个参数，是一个回调函数

**代码示例:**
```
import React from 'react';
// 父组件
class Father extends React.Component {
  constructor() {
    super();
    this.state = {
      // 初始化数据
      name: "鸣人"
    }
  };
  componentDidMount() {
    //修改 name 值
    this.setState({ name: "佐助" }, () => { //第二个参数：回调函数
      //输出 佐助
      console.log(this.state.name)
    })
    // 输出 鸣人    name值已经修改，但输出的是鸣人，原因是setState是一个异步方法  
    console.log(this.state.name)
  }
  render() {
    const { name } = this.state
    return (
      <React.Fragment>
        {/* 传递 name 参数 */}
        <Son myName={name}></Son>
      </React.Fragment>
    )
  }
}
// 子组件
class Son extends React.Component {
  constructor() {
    super();
    this.state = {}
  }
  render() {
    const { myName } = this.props
    return (
      <React.Fragment>
        {/* 通过 this.props获取属性值 */}
        <h4>我的名字: {myName}</h4>
      </React.Fragment>
    )

  }
}
export default Father;

```