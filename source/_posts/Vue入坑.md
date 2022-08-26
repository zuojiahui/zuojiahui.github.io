---
title: Vue入坑
categories: Vue
tags:
  - vue
abbrlink: 16678
---

### Vue.js 是什么

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架

### Vue 的特点

1. 采用组件化模式，提高代码复用率、且让代码更好维护。
2. 声明式编码，让编码人员无需直接操作 DOM，提高开发效率。
3. 使用虛拟 DOM + Diff 算法，尽量复用 DOM 节点。

### Vue 导入

- 注意: 容器和实例是一对一关系

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <title>创建一个Vue对象</title>
</head>

<body>
  <div id="app">
    <h1>用户名是: {{name}}</h1>
  </div>
  <script type="text/javascript">
    //创建一个Vue对象
    new Vue({
      //指定,该对象代表<div id="app">,也就是说,这个div中的所有内容,都被当前的app对象管理
      el: "#app",
      //定义vue中的数据
      data: {
        name: "张三"
      }
    });
  </script>
</body>

</html>
```

### Vue 基本语法

#### 插值表达式

插值表达式用户把 vue 中所定义的数据, 显示在页面上. 插值表达式允许用户输入"JS 表达式"
**注意区分：js 表达式 和 js 代码（语句**
表达式： a, a+b, fun() ... (一个表达式会产生一个值，可以放在任何一个需要值的地方)
语句： if(){ } for(){ }
**Vue 模板语法包括两大类：**

1. 插值语法：

- 功能：用于解析标签体内容
- 写法：`{{ xxx }}` xxx 是 js 表达式，且可以直接读取到 data 中的所有区域

2. 指令语法：

- 功能：用于解析标签（包括：标签属性、标签体内容、绑定事件…）
- 举例：`<a v-bind:href="xxx">或简写为<a :href="xxx">`，xxx 同样要写 js 表达式，且可以直接读取到 data 中的所有属性
  备注：Vue 中有很多的指令，且形式都是 v-???，此处我们只是拿 v-bind 举个例子

```
<html lang="en">

<head>
  <meta charset="UTF-8">
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <title>创建一个Vue对象</title>
</head>

<body>
  <div id="app">
    <h1>用户名是: {{name}}</h1>
    <h1>时间: {{Date.now()}}</h1>
    <a v-bind:href="url">点我去博客</a>
    <a :href="url">点我去博客</a>
  </div>
  <script type="text/javascript">
    new Vue({
      el: "#app",
      data: {
        name: "你好👋",
        url: "https://zuojiahui.fun/"
      }
    });
  </script>
</body>

</html>
```

<!-- ### 初始化脚手架

> 如果你已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，你需要先通过 npm uninstall vue-cli -g 或 yarn global remove vue-cli 卸载它。

> Vue CLI 4.x 需要 Node.js v8.9 或更高版本 (推荐 v10 以上)。你可以使用 n，nvm 或 nvm-windows 在同一台电脑中管理多个 Node 版本。

#### 安装脚手架

```
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

安装之后，你就可以在命令行中访问 vue 命令。你可以通过简单运行 vue，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。

你还可以用这个命令来检查其版本是否正确：

```
vue --version
```

#### 创建项目

运行以下命令来创建一个新项目：

```
vue create hello-world
```

#### 启动项目

```
npm run serve -->
<!-- ``` -->
