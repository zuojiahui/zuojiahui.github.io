---
title: call apply bind三者之间的区别及作用
categories: JavaScript
tags:
  - call
  - apply
  - bind
abbrlink: 18266
---
### 前言
说到 call apply bind 这三者那就老生常谈，因为这三者之间属于高频面试题，每一次面试中基本都会有面试官问call、apply、bind的区别以及实现原理
### 理解
call、apply、bind三者都是用来调用函数并且改变函数内部的this指向
### 三者之间的区别
#### call 与 apply
其实 call 与 apply 几乎没有差别，如果有，也就传参方式不同  
`call` : 需要把实参列表按照形参列表的个数传进去
`apply` : 需要传递一个 arguments
```
var name = "鸣人",
     age = 18;
var obj1 = {
  name: "雏田",
  age: 17,
};
var obj = {
  name: "佐助",
   age: 20,
   say: function () {
   console.log("姓名" + this.name + "年龄" + this.age);
   },
};
obj.say(); // 姓名佐助年龄20  这里 obj 调用 say 方法，所以 this 指向 obj
//1.非严格模式下，如果参数不传,或者第一个传递的是null/undefined,this是window
//2.严格模式下：不传，this是undefined
obj.say.call();       // 姓名鸣人年龄18  此时 this 指向 window
obj.say.call(obj1);   // 姓名雏田年龄17  此时 this 指向 obj1
obj.say.apply();      // 姓名鸣人年龄18  此时 this 指向 window
obj.say.apply(obj1);  // 姓名雏田年龄17  此时 this 指向 obj1


//  在来看看传参方式
var newobj = {
  name: "佐助",
   age: 20,
 myfun: function (fm, t) {
   console.log(this.name + "年龄" + this.age, "来自" + fm + "去往" + t);
 },
};
var newobj1 = {
  name: "小樱",
   age: 18,
};
newobj.myfun("上海","北京")                    // 佐助年龄20 来自上海去往北京
newobj.myfun.call(newobj1,"成都","北京")       // 小樱年龄18 来自成都去往北京
newobj.myfun.apply(newobj1,["东京","上海"])    // 小樱年龄18 来自东京去往上海
```
#### call 与 bind
`call`: 不需要调用，自执行
`bind`: 返回的是一个新的函数，你必须调用它才会被执行
```
var newobj = {
  name: "佐助",
   age: 20,
 myfun: function (fm, t) {
   console.log(this.name + "年龄" + this.age, "来自" + fm + "去往" + t);
 },
};
var newobj1 = {
  name: "小樱",
   age: 18,
};
newobj.myfun.call(newobj1,'巴黎', '东京')      // 小樱年龄18 来自巴黎去往东京
newobj.myfun.bind(newobj1,'上海', '成都')()    // 小樱年龄18 来自上海去往成都
newobj.myfun.bind(newobj1,['上海', '成都'])()  // 小樱年龄18 来自上海,成都去往undefined
```
### 栗子
```
function fn(a, b) {
	this.c = 3
	console.log(a, b, this)
}

fn(1, 2) //  打印1, 2, Window

console.log(c) //  打印3

const obj = {d: 4}

fn.call(obj, 1, 2) //  打印1, 2, {d: 4}

fn.apply(obj, [1, 2]) //  打印1, 2, {d: 4}

fn.call(null, 1, 2) //  打印1, 2, window

fn.call(undefined, 1, 2) //  打印1, 2, window

fn.bind(obj)(1, 2) //  打印1,2，{d: 4}

fn.bind(obj, 5)(1, 2) //  打印5, 1, {d: 4}

fn.bind(obj, 5, 3)(1, 2) //  打印5, 3, {d: 4} 

```
### 源码
```
Function.prototype.call = function (obj, ...args) {
    // 错误写法，有同学问fn调用call，那么这里的this是指向fn，直接调用函数并传入参数不就可以了吗？
    // this(...args)
    
    // 上面这种写法，并不能改变fn函数内部的this指向，所以不符合的我们的需求。我们应该是让obj去调用fn函数，才能让fn指向obj
    
    // 1. 处理obj是undefined或者null的情况
    if (obj === undefinded || obj === null) {
        obj = window
    }
        
    // 2. 给obj添加一个方法tmpFn，等于fn函数
    obj.tmpFn = this
    
    // 3. 调用obj的tmpFn的方法，并保存执行结果，此时fn函数中this指向obj
    const result = obj.tmpFn(...args)
    
    // 4. 删除obj上的tmpFn
    delete obj.tmpFn
    
    // 5. 返回方法的返回值
    return result
}

Function.prototype.apply = function (obj, args) {
    // 1. 处理obj是undefined或者null的情况
    if (obj === undefinded || obj === null) {
        obj = window
    }
    // 2. 给obj添加一个方法tmpFn，等于fn函数
    obj.tmpFn = this
    
    // 3. 调用obj的tmpFn的方法
    const result = obj.tmpFn(args)
    
    // 4. 删除obj上的tmpFn
    delete obj.tmpFn
    
    // 5. 返回方法的返回值
    return result
}

Function.prototype.bind = function (obj, ...args) {
    // 1. 返回一个新函数，这里采用es6 箭头函数写法，不懂的同学自行学习
    return (...args2) => {
		// 2. 调用原来函数，改变this指向obj，参数列表由args和args2依次组成。
        return this.call(obj, ...args, ...args2) // 这里this.call的this指向的是fn函数，也就是调用bind的函数。
    }
}

```
