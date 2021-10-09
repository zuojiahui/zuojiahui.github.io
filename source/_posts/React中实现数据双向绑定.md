---
title: React实现数据双向绑定
categories: React
tags:
  - 双向绑定
abbrlink: 51352
---
## 前言
双向数据绑定，带来双向数据流。数据（state）和视图（View）之间的双向绑定。
说到底就是 （value 的单向绑定 + onChange 事件侦听）的一个语法糖
**特点:** 无论数据改变，或是用户操作，都能带来互相的变动，自动更新。适用于项目细节
## 案例
```
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            amount: 0
        }
    }
    changeAmout(e) {
        this.setState({ amount: e.target.value })
    }
    render() {
        return (
            <div>
                数量 ：<input type="text" value={this.state.amount} onChange={(event)=>{
                    this.changeAmout(event)
                }}/>
                <br/>
                数量值 : {this.state.amount}
            </div>
        )
    }
}
```

