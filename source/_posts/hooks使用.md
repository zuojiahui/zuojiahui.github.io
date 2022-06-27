---
title: hooks使用
categories: React
tags:
  - hooks
abbrlink: 4234
top: 2
---
## 前言
hooks是 16.8 版本之后的新特性，React 团队希望，组件不要变成复杂的容器，最好只是数据流的管道。开发者根据需要，组合管道即可。 组件的最佳写法应该是函数，而不是类。
React 早就支持函数组件，
但是，这种写法有重大限制，必须是纯函数，不能包含状态，也不支持生命周期方法，因此无法取代类。
React Hooks 的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件。
Hooks可以让无状态组件实现有状态组件的部分功能，比如设置state，使用钩子函数
## useState - 状态 
```
import React, { Fragment, useState } from 'react';
const HooksCompont = () => {
  const [count, setCount] = useState(0);
  return (
    <Fragment>
      <h3>hello hooks!! {count}</h3>
      <button onClick={() => setCount(count + 1)}>加</button>
    </Fragment>
  );
};
export default HooksCompont;
```
## useEffect - 副作用
```
import React, { Fragment, useState, useEffect } from 'react';
const HooksCompont = () => {
  const [count, setCount] = useState(0);
  const [disable, setDisable] = useState(true);
  // useEffect 相当于 conmponentDidMout,conmponentDidUpdate和 conmponentWillUnmount
  useEffect(() => {
    console.log(count, '页面挂载');
    if (count === 2) {
      console.log(count, '变化');
    }
    return () => {
      // 这里相当于conmponentWillUnmount (页面离开时)
      // 但是如果 第二参数值 发生改变时，触发副作用函数，并且执行里面逻辑
      console.log('执行离开');
    };
    // 当 useEffect 未添加第二个参数时 ：页面每次重新渲染(任意点击按钮),都要执行一遍这些副作用函数，显然是不经济的。
    // 怎么跳过一些不必要的计算呢？我们需要给 useEffect 传第二个参数即可。
    // 用第二个参数来告诉 react只有当  这个参数的值 发生改变时，才执行我们传的副作用函数（第一个参数）。
  }, [count]);

  useEffect(() => {
    return () => {
      // 这里相当于conmponentWillUnmount (页面离开时)
      console.log('离开');
    };
  }, []);
  return (
    <Fragment>
      <h3>hello hooks!! {count}</h3>
      <button onClick={() => setCount(count + 1)}>加</button>
      <button onClick={() => setDisable(!disable)}>{disable ? '启用' : '禁用'}</button>
    </Fragment>
  );
};
export default HooksCompont;
```
## userReducer - 复杂状态处理
```
import React, { Fragment, useReducer } from 'react';
const ReducerDemo = () => {
  const initialState = { count: 0, name: '鸣人' };
  const reducer = (state, action) => {
    const { type, payload = 1 } = action;
    switch (type) {
      case 'increment':
        return { ...state, count: state.count + payload };
      case 'decrement':
        return { ...state, count: state.count - payload };
      case 'rename':
        return { ...state, name: payload };
      default:
        throw new Error();
    }
  };
  // 第一位参数是函数, 第二参数为初始值
  // useReducer((state, action)=>{}, 0)
  // const [state, dispatch] = useReducer(reducer, initialArg);
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <Fragment>
      <div>
        useReducer {state.count} ---- {state.name}
      </div>
      <button onClick={() => dispatch({ type: 'increment' })}>加</button>
      <button onClick={() => dispatch({ type: 'decrement', payload: 2 })}>减</button>
      <button onClick={() => dispatch({ type: 'rename', payload: '佐助' })}>changeName</button>
    </Fragment>
  );
};
export default ReducerDemo;
```
## useContext - 深层值传递
```
import React, { Fragment, useState, createContext, useContext } from 'react';

// 创建 Context
const theme = createContext('#fff');

const Demo = () => {
  return <Child />;
};

const Child = () => {
  const father = useContext(theme);
  const style = {
    backgroundColor: father.bgColor,
  };
  return (
    // 消费者
    <Fragment>
      {/* 第一种写法 */}
      <div style={style}>{father.content}</div>
      {/* 第二种 使用 Consumer 属性 */}
      <theme.Consumer>{(value) => <div>{value.bgColor}</div>}</theme.Consumer>
    </Fragment>
  );
};

const HooksContext = () => {
  // 深层值传递相当于 生产者 和 消费者模式
  const [bgColor, setBgColor] = useState('#fff');
  return (
    <Fragment>
      <input
        type="color"
        onChange={(e) => {
          setBgColor(e.target.value);
        }}
      />
      {/* 生产者 */}
      <theme.Provider value={{ bgColor, content: 'Child' }}>
        <Demo />
      </theme.Provider>
    </Fragment>
  );
};
export default HooksContext;

```
## useRef - 引用
```
import React, { Fragment, useEffect, useRef, forwardRef } from 'react';

const Input = forwardRef((prpos, ref) => {
  // 将ref父类的ref作为参数传入函数式组件中，本身props只带有children这个参数，这样可以让子类转发父类的ref,
  // 当父类把ref挂在到子组件上时，子组件外部通过forwrardRef包裹，可以直接将父组件创建的ref挂在到子组件的某个dom元素
  return <input {...prpos} ref={ref} />;
});

const RefDemo = () => {
  // 用于获取元素的原生DOM或者获取自定义组件所暴露出来的ref方法(父组件可以通过ref获取子组件，并调用相对应子组件中的方法)
  const inputRef = useRef();
  useEffect(() => {
    inputRef.current.focus();
    inputRef.current.value = 'hello react';
  }, []);
  return (
    <Fragment>
      <Input placeholder="请输入" ref={inputRef} />
    </Fragment>
  );
};
export default RefDemo;

```
## useMemo
```
import React, { Fragment, useState, useMemo } from 'react';

const Child = ({ name, children }) => {
  const change = () => {
    // 要求点击 鸣人 才执行此语句    现点击任何按钮都会执行 导致子组件重新渲染
    // 原因 : 父组件任何一个状态发生变化，子组件的代码块都会重新执行一遍
    window.console.log('鸣人分身');
    return name;
  };
  //  useMemo 是在DOM更新前触发的
  const actionChange = useMemo(()=>change(),[name]);
  return (
    <Fragment>
      <div>{actionChange}</div>
      <div>{children}</div>
    </Fragment>
  );
};
const Demo = () => {
  const [mingren, setMingren] = useState('鸣人');
  const [zuozhu, setZuozhu] = useState('佐助');
  return (
    <Fragment>
      <button
        onClick={() => {
          setMingren(mingren + 1);
        }}
      >
        鸣人
      </button>
      <button
        onClick={() => {
          setZuozhu(zuozhu + 2);
        }}
      >
        佐助
      </button>
      <Child name={mingren}>{zuozhu}</Child>
    </Fragment>
  );
};
export default Demo;

```
## 轮播图案例
```
//轮播图数据源
import React from 'react'
import Swiper from '../../components/swiper'
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            images: [
                { src: require("../../assets/images/banner1.jpg"), url:"https://baidu.com"},
                { src: require("../../assets/images/banner2.jpg"), url:"https://baidu.com"},
                { src: require("../../assets/images/banner3.jpg"), url:"https://baidu.com"},
                { src: require("../../assets/images/banner3.jpg"), url:"https://baidu.com"},
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
//轮播图UI
import React from 'react'
import Hoc from './hoc'
import "./style.css"
export default Hoc((props) => {
    return (
        <div className="banner">
            <div className="my-swiper-main" onMouseOver={props.stop} onMouseOut={props.autoPlay}>
                {
                    props.data.length > 0 && props.data.map((item, index) => {
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
                        props.data.length > 0 && props.data.map((item, index) => {
                            return (
                                <div className={item.active ? "dot active" : "dot"} key={index} onClick={() => { props.change(index) }}></div>
                            )
                        })
                    }
                </div>
            </div>
        </div>
    )
})
```
```
//轮播图逻辑
import React, { useState, useEffect, useRef, useCallback } from 'react';
export default function Hoc(WithCompont) {
    return function HocCompont(props) {
        let [data, setData] = useState([]);
        let [isInit, setIsInit] = useState(true)
        let [iIndex, setIIndex] = useState(0)
        // 创建一个表示，通用容器，专门解决 setInterval
        let timer = useRef(null);

        function change(index) {       //点击切换图片
            setIIndex(index)
            if (data.length > 0) {
                for (let i = 0; i < data.length; i++) {
                    if (data[i].active) {
                        data[i].active = false;
                        break;
                    }
                }
            }
            data[index].active = true;
            setData(data)
        }

        const autoPlay = useCallback(() => {      //自动播放
            clearInterval(timer.current)
            timer.current = setInterval(() => {
                let tmpIndex = iIndex;
                if (data.length > 0 && data) {
                    for (let i = 0; i < data.length; i++) {
                        if (data[i].active) {
                            data[i].active = false;
                            break;
                        }
                    }
                    if (tmpIndex >= data.length - 1) {
                        tmpIndex = 0;
                    } else {
                        tmpIndex++
                    }
                    data[tmpIndex].active = true;
                    setIIndex(tmpIndex)
                    setData(data)
                }

            }, (3000))
        }, [data, iIndex])  //  useState 中用到的值需要放在 第二个参数中
        
        function stop() {     //鼠标经过清除定时器
            clearInterval(timer.current)   
        }
        useEffect(() => {
            if (props.data && props.data.length > 0 && isInit) {
                setIsInit(false);
                for (let i = 0; i < props.data.length; i++) {
                    if (i === 0) {
                        props.data[i].active = true;
                    } else {
                        props.data[i].active = false
                    }
                }
                setData(props.data); //将默认的空数组等于 props.data
            }
            autoPlay();
            return () => {    //页面离开清除定时器 (页面离开时执行)
                clearInterval(timer.current)
            }
        }, [props.data, isInit, autoPlay]);
        let newsPros = {
            change: change,
            data: data,
            stop: stop,
            autoPlay: autoPlay,
        }
        return (
            // <WithCompont {...props} data={data} change={change} stop={stop}></WithCompont>
            <WithCompont {...props} {...newsPros}></WithCompont>

        )
    }
}
```