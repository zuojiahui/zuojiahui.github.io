---
title: React组件的生命周期的使用
categories: React
tags:
  - React组件生命周期
abbrlink: 45464
date: 2020-10-26 12:35:03
---
### 组件的生命周期钩子函数
“ 钩子 ”就是在某个阶段给你一个做某些处理的机会。生命周期钩子函数就是在组件预备、创建、使用和销毁的过程中的函数监听。就和我们人一样，从出生到死亡的过程  
**react的生命周期可以分成三个部分：**  
**1. 挂载**
**2. 渲染&&更新**
**3. 卸载**
### 1. 挂载
当组件实例被创建并插入 `DOM` 中时，其生命周期调用顺序如下：
##### constructor( )
在 **React 组件挂载之前，**会调用此钩子函数。  

**此钩子函数仅用于一下两种情况:**
1. 通过给`this.state`赋值对象来初始化内部`state`
2. 为事件处理函数绑定实例  
**注意:** 此钩子函数中不应调用`setState( )`方法。如果你的组件需要使用内部`state`请直接在构造函数中为`this.state` 赋值初始`state`

##### static getDerivedStateFromProps( )
##### render( )
react最重要的步骤，创建虚拟dom，进行diff算法，更新dom树都在此进行。此时就不能更改state了。  
这里是合成虚拟DOM，可以理解为，在这里实际上都还没有真实的dom。
##### componentDidMount( )
此钩子函数会渲染真实的`DOM`浏览器，在这里才可以得到`DOM`。  
**此钩子函数一般用于:**
依赖于 `DOM` 节点的初始化应该放在这里,`ajax`请求一般都在这里进行。
### 2. 渲染&&更新
##### static getDerivedStateFromProps( )
##### shouldComponentUpdate( ) 
##### render( ) 
##### getSnapshotBeforeUpdate( )  
##### componentDidUpdate( )
### 3. 卸载
##### componentWillUnmount( )  
此钩子函数会在组件卸载及销毁之前调用。
**此钩子函数一般用于:**
**例 :** 清除`timer`，取消网络请求或解绑某些事件
**注意:** 此钩子函数中不应调用 `setState( )`因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

