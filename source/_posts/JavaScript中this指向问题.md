---
title: JavaScript中this指向问题
categories: JavaScript
tags:
  - this
abbrlink: 3329
---
### 前言
面向对象语言中 this 表示当前对象的一个引用,但在 JavaScript 中 this 不是固定不变的，它会随着执行环境的改变而改变。
### 1. 全局作用域或者普通函数中 this 指向全局对象 window
```
//直接打印
var a = 10;
console.log(this.a)
```
>10

```
//function声明函数
var a = 10;
function bar() {
  var a = 20
  console.log(this.a);
}
bar();
```
>10

```
//function声明函数赋给变量
var a = 10;
var bar = function () {
  var a = 20;
  console.log(this.a);
};
bar();
```
>10

```
//立即执行函数
var a = 10;
(function () {
  var a = 20;
  console.log(this.a);
})();
```
>10

```
//定时器方法
var a = 10;
setInterval(function () {
    var a = 20
  console.log(this.a);
}, 2000);
```
>10
### 2. 方法调用中谁调用 this 指向谁
```
//对象方法调用
var a = 10;
var person = {
  a: 20,
  run: function () {
    console.log(this.a);
  },
};
person.run();
```
>20

```
<!-- 事件绑定 -->
<button type="button">点我</button>
<script>
  var btn = document.querySelector("button");
  btn.onclick = function () {
    console.log(this);
  };
</script>
```
> < button button button type="button">点我 < /button>

```
<!-- 事件监听 -->
<button type="button">点我</button>
<script>
  var btn = document.querySelector("button");
  btn.addEventListener("click", function () {
    console.log(this);
  });
</script>
```
> < button button button type="button">点我 < /button>
### 3. 在构造函数或者构造函数原型对象中 this 指向构造函数的实例
```
//不使用new指向window
function Person(name) {
  console.log(this)     // window
  this.name = name;
}
Person('inwe')
//使用new
function Person(name) {
  this.name = name
  console.log(this)     //people
  self = this
}
var people = new Person('iwen')
console.log(self === people)       //true
//这里new改变了this指向，将this由window指向Person的实例对象people
```
>window
>people
>true

### 4. 箭头函数中指向外层作用域的 this
```
var obj = {
  foo() {
    console.log(this);
  },
  bar: () => {
    console.log(this);
  }
}
obj.foo()    // {foo: ƒ, bar: ƒ}
obj.bar()    // window
```
>{foo: ƒ, bar: ƒ}
>window