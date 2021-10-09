---
title: JS中的浅拷贝与深拷贝
categories: JavaScript
tags:
  - 浅拷贝
  - 深拷贝
abbrlink: 35313
---
### 前言
浅拷贝是拷贝一层，深层次的对象级别的就拷贝引用；  
深拷贝是拷贝多层，每一级别的数据都会拷贝出来；
**总结来看 :**   
浅拷贝的时候如果数据是基本数据类型，那么就如同直接赋值那种，会拷贝其本身，如果除了基本数据类型之外还有一层对象，那么对于浅拷贝而言就只能拷贝其引用，对象的改变会反应到拷贝对象上；但是深拷贝就会拷贝多层，即使是嵌套了对象，也会都拷贝出来。
### 浅拷贝
#### 实现方式一
```
var a = {x : 1}
var b = a
console.log(b)    // {x:1}
b.x = 2
console.log(a)   // {x:2}
console.log(b)   // {x:2}
```
#### 实现方式二
```
var obj = {
    name: "abc",
    num: [1,2,3,4,5],
  }
var obj1 = {}
for (var prop in obj) {       //用遍历的方式将 obj 的数据拷贝给 obj1
    obj1[prop] = obj[prop]
}
obj1.num[4] = 8
console.log(obj.num[4])    //此时可以看见 obj 的数据也进行了改变    8
```
**上述案例可以看出:** 浅拷贝是一个传址,也就是把 a 的值赋给 b 的时候同时也把 a 的址赋给了 b,当 a || b 的值改变的时候， a || b 的值也同时会改变, 实现浅拷贝的方法很多，这里不多讲，在来看看重点深拷贝
### 深拷贝
#### 实现方式一
**使用递归复制所有层级属性**
```
function deepClone(obj) {
  let objClone = Array.isArray(obj) ? [] : {};
  if (obj && typeof obj === "object") {
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        // 判断obj子元素是否为对象，如果是，递归复制
        if (obj[key] && typeof obj[key] === "object") {
          objClone[key] = deepClone(obj[key]);
        } else {
          objClone[key] = obj[key];
        }
      }
    }
  }
  return objClone;
}
let a = [1, 2, 3, 4, [5, 6]];
let b = deepClone(a); 
a[4][0] = 0;
console.log(a);  // [1, 2, 3, 4, [0, 6]]
console.log(b);  // [1, 2, 3, 4, [5, 6]]
```
#### 实现方式二
**通过 JSON 对象实现深拷贝**
```
function deepClone (obj) {
  let _obj = JSON.stringify(obj)
  let objClone = JSON.parse(_obj)
  return objClone
}
let a = [0,1,[2,3],4]
let b = deepClone(a)
a[0] = 1  
a[2][0] = 1
console.log(a)  // [1,1,[1,3],4]
console.log(b)  // [0,1,[2,3],4]
```
#### 实现方式三
**使用 JQ 的 extend 方法**
```
$.extend([deep ], target, object1 [, objectN ])
```
deep表示是否深拷贝，为true为深拷贝；为false，为浅拷贝。

target Object类型 目标对象，其他对象的成员属性将被附加到该对象上。

object1  objectN可选。 Object类型 第一个以及第N个被合并的对象。
```
let a = [0,1,[2,3],4]
let b = $.extend(true, [], a)
a[0] = 1
a[2][0] = 1
console.log(a)  // [1,1,[1,3],4]
console.log(b)  // [0,1,[2,3],4]
```
**以上几种方法可完全实现深度拷贝，下面还有几种是不完全实现深度拷贝**
### 浅深拷贝
**当对象中只有一级属性，没有二级属性的时候，此方法为深拷贝，但是对象中有对象的时候，此方法，在二级属性以后就是浅拷贝。**
#### 方式一
**使用扩展运算符实现深拷贝**
```
var obj = { a: 1,b: {c:2}}
var newObj = { ...obj }
obj.a = 2
obj.b.c = 0
console.log(obj);     // { a: 2,b: {c:0}}
console.log(newObj)   // { a: 1,b: {c:0}}
```
#### 方式二
**数组中 slice 方法**
```
let a = [0,1,[2,3],4]
let b = a.slice()
a[0] = 1
a[2][0] = 1
console.log(a)  // [1,1,[1,3],4]
console.log(b)  // [0,1,[1,3],4]
```
**同理数组中的 concat 方法也会实现上述情况**
#### 方式三
**通过使用 Object.assign() 实现**
```
const objA = { a: 1, b: { c: 2 } };
const objB = { name: "xixi" };
const obj = Object.assign({},objA, objB);
objA.a = 2;
objA.b.c = 3;
objB.name = "haha";
console.log(objA);  //  { a: 2, b: { c: 3 } };
console.log(objB);  //   { name: "haha" };
console.log(obj);   // { a: 1, b: { c: 3 }, name : "xixi" };
```

