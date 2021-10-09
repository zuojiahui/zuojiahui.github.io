---
title: TypeScript从入门到入坑
categories: TypeScript
tags:
  - TypeScript
abbrlink: 17690
---
### 安装
在使用 npm 命令之前电脑必须安装 nodejs
```
npm install -g typescript
或者
cpm install -g typescript
或者
yarn global add typescript
验证是否安装成功
tsc -v
```
### TypeScript 中的数据类型
```
// 布尔类型  boolean
const show: boolean = true

// 数字类型  number
const num: number = 123.4

// 字符串类型  string
const str: string = "你好!"

// 数组类型  array   ts中定义数组有两种方式
// 1. 第一种定义数组方式
const arr: number[] = [1, 2, 3, 4, 5, 6]
const arr1: string[] = ["js", "css"]
// 2. 第一种定义数组方式 (泛型)
const arr2: Array<number> = [1, 2, 3, 4, 5, 6]
const arr3: Array<string> = ["js", "css"]
// 2. 第三种定义数组方式
const arr4: any[] = [11, "22", true]

// 元组类型  tuple  (属于数组的一种)
const arr5: [boolean, string, number] = [true, "你好", 123]

/*
枚举类型 enum

enum 枚举名 {
    标识符[=整型常数]
    ...
    标识符[=整型常数]
}

pay_status   0 未支付  1 支付  2 交易成功
flag 1 true  2 false
*/
enum Flag { true = 1, false = 2 }
const f: Flag = Flag.true
console.log(f)  // 1
enum Num { num0, num1, num2 }   // 为赋值即为索引值
const n: Num = Num.num0
console.log(n)  // 0

// 任意类型 any    
const anyType: any = true
const anyType1: any = 123
const anyType3: any = "你好"

// null 和 undefined  其他(never类型) 数据类型的子类型
let numUndefined: number | undefined;
console.log(numUndefined)  // undefined
// 一个元素可能是 number 可能是 null 可能是 undefined
let anyTypes: number | null | undefined;

// void 类型 ：表示没有任何类型，一般用于定义方法没有返回值
/*
es5的定义方法
function run() {
    console.log("run")
}
run()
*/
function run(): void {
    console.log("run")
}
run()
// 如果有返回值
function run1(): number {
    return 123
}
run()

// never类型：其他类型（包括 null 和 undefined）的子类型，代表从来不会出现值
// 声明的 never 的变量只能被 never类型赋值
let a: never = (() => {
    throw new Error("错误")
})()
```
### TypeScript 中的函数