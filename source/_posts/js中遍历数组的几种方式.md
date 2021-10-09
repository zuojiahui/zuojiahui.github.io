---
title: js中遍历数组的几种方式
categories: JavaScript
tags:
  - 数组
abbrlink: 30917
---
### 前言
JS数组遍历，基本就是for,for in,forEach,for of,map等等一些方法，以下介绍几种本文分析用到的数组遍历方式
### 第一种: 普通for循环
最简单的一种，也是使用频率最高的一种，虽然性能不弱，但仍有优化空间
```js
let arr = ["apple", "banana", "watermelon", "pear"];
for (let i = 0; i < arr.length; i++) {
  console.log(i)         // 0 1 2 3
  console.log(arr[i])    // apple banana watermelon pear
}
```
### 第二种: 优化版for循环
使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显  
这种方法基本上是所有循环遍历方法中性能最高的一种
```
let arr = ["apple", "banana", "watermelon", "pear"];
for(let i = 0, len = arr.length; i < len; i++) {
  console.log(i)         // 0 1 2 3
  console.log(arr[i])    // apple banana watermelon pear
}
```
### 第三种: 弱化版for循环
这种方法其实严格上也属于for循环，只不过是没有使用length判断，而使用变量本身判断
```
let arr = ["apple", "banana", "watermelon", "pear"];
for (let i = 0; arr[i] != null; i++) {
  console.log(i);        // 0 1 2 3
  console.log(arr[i]);   // apple banana watermelon pear
}
```
### 第四种: forEach循环
数组自带的forEach循环，使用频率较高，实际上性能比普通for循环弱
```
let arr = ["apple", "banana", "watermelon", "pear"];
arr.forEach((item, index, arr) => {
  console.log(item);    // apple banana watermelon pear
  console.log(index);   // 0 1 2 3
  console.log(arr);     // 数组本身
});
```
### 第五种: forEach变种
由于forEach是Array型自带的，对于一些非这种类型的，无法直接使用(如NodeList)，所以才有了这个变种，使用这个变种可以让类似的数组拥有forEach功能。  
实际性能要比普通forEach弱
```
let arr = ["apple", "banana", "watermelon", "pear"];
Array.prototype.forEach.call(arr, (item, index) => {
  console.log(item);    // apple banana watermelon pear
  console.log(index);   // 0 1 2 3
});
```
### 第六种: for in循环
这个循环很多人爱用，但实际上，经分析测试，在众多的循环遍历方式中它的效率是最低的
```
let arr = ["apple", "banana", "watermelon", "pear"];
for (let i in arr) {
  console.log(i);       // 0 1 2 3
  console.log(arr[i]);  // apple banana watermelon pear
}
```
### 第七种: map遍历
这种方式也是用的比较广泛的，虽然用起来比较优雅，但实际效率还比不上forEach
```
let arr = ["apple", "banana", "watermelon", "pear"];
arr.map((item, index) => {
  console.log(item);    // apple banana watermelon pear
  console.log(index);   // 0 1 2 3
}); 
```
### 第八种: for of遍历(需要ES6支持)
这种方式是es6里面用到的，性能要好于for in，但仍然比不上普通for循环
```
let arr = ["apple", "banana", "watermelon", "pear"];
for (let value of arr) {
  console.log(value);  // apple banana watermelon pear
}
```
### 各种遍历方式的性能对比
以下截图数据是，在chrome (支持es6)中运行了1000次后得出的结论(每次运行100次,一共10个循环，得到的分析结果) 
<div  align="center">    
<img src="https://gitee.com/zuo_jiahui/blogimage/raw/master/img/6361fb0c77ece9ce732852ee1699d60.png" width = 100% height = 100% />
</div> 
