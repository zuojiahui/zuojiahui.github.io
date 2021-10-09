---
title: React中使用axios发送请求
categories: React
tags:
  - axios
abbrlink: 50332
---
### 前言
axios 是一个基于Promise 用于浏览器和 nodejs 的 HTTP client
#### 特征
* 从浏览器创建 XMLHttpRequests
* 从node.js 发出 http 请求
* 支持 Promise API
* 拦截请求(request) 和响应(response)
* 转换请求和响应数据
* 终止请求
* 自动转换 JSON 数据
* Client 端支持防范 XSRF

## 安装
```
npm install axios --save
```
## 基本使用
### GET请求
```
import React from 'react';
import axios from 'axios'     // 第一步引入 axios
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            navs: [],
        }
    }
    componentDidMount() {    // 第二步在此生命周期函数中发送请求
        // axios后面直接跟 url 地址 ， 在使用 .then方法
        axios.get("http://vueshop.glbuys.com/api/home/index/slide?token=1ec949a15fb709370f").then((res) => {
            console.log(res)
            if (res.status === 200) {
                this.setState({ navs: res.data.data })
            } else {
                alert("请求失败")
            }
        })
    }
    render() {
        return (
            <React.Fragment>
                <ul>
                    {this.state.navs.map((item, index) => {    // 第三步渲染数据
                        return (
                            <li key={index}>{item.title}</li>
                        )
                    })}
                </ul>
            </React.Fragment>
        )
    }
}
export default App;
```
### POST请求
```
import React from 'react';
import axios from 'axios'     // 第一步引入 axios
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            phone: "",
            password: "",
        }
    }
    componentDidMount() {        // 第二步在此生命周期函数中发送请求
        // axios后面直接跟 url 地址 ， 在使用 .then方法
        axios.post("http://vueshop.glbuys.com/api/home/index/slide?token=1ec949a15fb709370f", {
            // 第三步传递需要的数据      clellphone,    password这是需要传递的数据
            clellphone: this.state.phone,
            password: this.state.password
        }).then((res) => {
            if (res.status === 200) {
               console.log("请求成功",res)
            } else {
                alert("请求失败")
            }
        })
    }
    render() {
        return (
            <React.Fragment>
            </React.Fragment>
        )
    }
}
export default App;
```
### 上传本地文件
```
import React from 'react';
import axios from 'axios'     // 第一步引入 axios
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            num: 0,
        }
    }
    upHead(e) {
        let headFile = e.target.files[0];
        let data = new FormData();
        data.append("headfile", headFile);
        var config = {
            onUploadProgress: (progressEvent) => {
                // 进度条
                var percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total);
                this.setState({ num: percentCompleted }, () => {
                    if (this.state.num === 100) {
                        this.setState({ num: 0 })
                    }
                })
            }
        };
        axios.post("http://vueshop.glbuys.com/api/user/myinfo/formdatahead?token=1ec949a15fb709370f",
            data, config).then(res => {
                console.log(res);
                if (res.data.code === 200) {
                    this.setState({ showHead: "http://vueshop.glbuys.com/userfiles/head/" + res.data.data.msbox })
                }
            })
    }
    render() {
        return (
            <React.Fragment>
                上传头像 : <input type="file" onChange={this.upHead.bind(this)} /> <br />
                 头像预览 : {this.state.showHead !== "" ? <img src={this.state.showHead} alt="" style={{ width: 200, height: 200 }} /> : ""}<br />
                 进度条预览 : <div style={{ width: 300, height: 25, border: 1, }}>
                    <div style={{ height: 100 + '%', width: this.state.num + "%", background: "green" }}></div>
                </div>
            </React.Fragment>
        )
    }
}
export default App;
```
## 封装请求
建立 reques.js文件，内容如下:
```
import axios from 'axios'
export function request(url, method = "get", data = {}, config = {}) {
    return axiosRequest(url, method, data, config)
}
function axiosRequest(url, method, data, config) {
    if (method.toLocaleLowerCase() === "post") {
        let params = new URLSearchParams();       
        if (data instanceof Object) {              //如果后端格式为 raw    从此开始注释
            for (let key in data) {
                params.append(key, data[key]);
            }
            data = params;                        // 此处注释结束
        }                       
    } else if (method.toLocaleLowerCase() === "file") {
        method = "post";
        let params = new FormData();
        if (data instanceof Object) {
            for (let key in data) {
                params.append(key, data[key]);
            }
            data = params;
        }
    }
    let axiosConfig = {
        url: url,
        method: method.toLocaleLowerCase(),
        data: data
    };
    if (config instanceof Object) {
        for (let key in config) {
            axiosConfig[key] = config[key];
        }
    }
    return axios(axiosConfig).then(res => res.data);
}
```
### GET请求
```
import React from 'react';
import { request } from '../../utils/reques';    // 第一步引入方法
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            navs: [],
        }
    }
    componentDidMount() {    // 第二步在此生命周期函数中发送请求
        // 调用此方法，第一位参数为请求地址，第二位参数为请求方式
        request("http://vueshop.glbuys.com/api/home/index/slide?token=1ec949a15fb709370f","get").then(res => {
            if (res.code === 200) {
                this.setState({ navs: res.data })
            } else {
                alert("请求失败")
            }
        })
    }
    render() {
        return (
            <React.Fragment>
                <ul>
                    {this.state.navs.map((item, index) => {    // 第三步渲染数据
                        return (
                            <li key={index}>{item.title}</li>
                        )
                    })}
                </ul>
            </React.Fragment>
        )
    }
}
export default App;
```
### POST请求
```
import React from 'react';
import { request } from '../../utils/reques';       // 第一步引入方法
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            phone: "",
            password: "",
        }
    }
    componentDidMount() {        // 第二步在此生命周期函数中发送请求
        // 调用此方法，第一位参数为请求地址，第二位参数为请求方式，第三位参数为需要传递的数据
        request("http://vueshop.glbuys.com/api/home/index/slide?token=1ec949a15fb709370f", "post", {
            clellphone: this.state.phone,
            password: this.state.password
        }.then((res) => {
            if (res.code === 200) {
                this.props.dispatch(actions.user.login({ username: this.state.username, isLogin: true }))
            } else {
                alert("请求失败")
            }
        }))
    }
    render() {
        return (
            <React.Fragment>
            </React.Fragment>
        )
    }
}
export default App;
```
### 上传本地文件
```
import React from 'react';
import { request } from '../../utils/reques';    // 第一步引入方法
class App extends React.Component {
    constructor() {
        super();
        this.state = {
            num: 0,
        }
    }
    upHead(e) {
        let headFile = e.target.files[0];
        var config = {
            onUploadProgress: (progressEvent) => {
                // 进度条
                var percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total);
                this.setState({ num: percentCompleted }, () => {
                    if (this.state.num === 100) {
                        this.setState({ num: 0 })
                    }
                })
            }
        };
         // 调用此方法，第一位参数为请求地址，第二位参数为请求方式,这里 file 方法中改为为了 post
        request("http://vueshop.glbuys.com/api/user/myinfo/formdatahead?token=1ec949a15fb709370f", "file", {
            headfile: headFile
        }, config).then(res => {
            if (res.code === 200) {
                this.setState({ showHead: "http://vueshop.glbuys.com/userfiles/head/" + res.data.msbox })
            } else {
                alert("请求失败")
            }
        })
    }
    render() {
        return (
            <React.Fragment>
                上传头像 : <input type="file" onChange={this.upHead.bind(this)} /> <br />
                 头像预览 : {this.state.showHead !== "" ? <img src={this.state.showHead} alt="" style={{ width: 200, height: 200 }} /> : ""}<br />
                 进度条预览 : <div style={{ width: 300, height: 25, border: 1, }}>
                    <div style={{ height: 100 + '%', width: this.state.num + "%", background: "green" }}></div>
                </div>
            </React.Fragment>
        )
    }
}
export default App;
```