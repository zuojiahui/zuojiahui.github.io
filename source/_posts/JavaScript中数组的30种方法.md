---
title: JavaScript中数组的使用及方法
categories: JavaScript
tags:
  - 数组
abbrlink: 3498
---
## 前言
<font face="微软雅黑" size=5>**什么是数组**</font> 
数组对象是使用单独的变量名来存储一系列的值。简单意思就是普通变量一次只能储存一个值，数组可以储存多个值
如果你有一组数据（例如：车名字），存在单独变量如下所示:
```
var car1="奔驰";
var car2="奥迪";
var car3="宝马";
```
## 创建数组
<font face="微软雅黑" size=5>**创建一个数组有3种方式**</font> 
**方式一 : 常规方式**
```
var myCars = new Array();
myCars[0]="奔驰"
myCars[1]="奥迪"
myCars[2]="宝马"
```
**方式二 : 简洁方式**
```
var myCars=new Array("奔驰","奥迪","宝马");
```
**方式三 : 字面 (常用)** 
```
var myCars=["奔驰","奥迪","宝马"];
```

## 数组的属性
### constructor
**返回创建数组对象原型的函数**
返回myCars数组对象原型创建的函数：
```
var myCars = ["奔驰", "奥迪", "宝马"]
myCars.constructor
```
**结果输出:**
>function Array() { [native code] }

**在JavaScript中，constructor属性返回对象的构造函数。** 
**返回值是函数的引用，不是函数名:**
>JavaScript 数组 constructor 属性返回 function Array() { [native code] }
>JavaScript 数字 constructor 属性返回 function Number() { [native code] }
>JavaScript 字符串 constructor 属性返回 function String() { [native code] }
### lenght
**设置或返回数组元素的个数。**
```
var myCars = ["奔驰", "奥迪", "宝马"]
myCars.length
```
**结果输出:**
>3

**设置数组的数目：array.length=number**
```
var myCars = ["奔驰", "奥迪", "宝马"]
myCars.length=10
console.log(myCars.length) 
console.log(myCars) 
console.log(myCars[5])
```
**结果输出:**
>10
>["奔驰", "奥迪", "宝马", empty × 7] &emsp; &emsp; &emsp; 后面为空值
>undefined
### prototype
**允许你向数组对象添加属性或方法**
**添加一个新的数组的方法，将数组值转为大写：**
```
var fruits=["Banana","Orange","Apple","Mango"]
Array.prototype.myUcase = function () {
    for (i = 0; i < this.length; i++) {
        this[i] = this[i].toUpperCase();      //toUpperCase() 方法用于把字符串转换为大写
    }
}
fruits.myUcase();
```
**结果输出:**
>["BANANA", "ORANGE", "APPLE", "MANGO"]
## Array 对象方法
**数组总共有30种方法**
### 不改变原数组
#### toString()
**把数组转换为字符串**
```
var arr = [1, 2, 3, 4]
var sum = arr.toString()
console.log(sum)
```
**输出结果 :**
>1,2,3,4,     字符串类型
#### concat()
**连接两个或更多的数组，并返回结果**
```
var fruits1 = ["Banana", "Orange",]
var fruits2 = ["Apple","Mango",]
var fruits3 = ["watermelon","pear",]
var fruits = fruits1.concat(fruits2,fruits3)
```
**fruits 输出结果:**
> ["Banana", "Orange", "Apple", "Mango", "watermelon", "pear"]
#### entries()
**返回数组的可迭代对象**
**简单说就是 Object.entries() 可以把一个对象的键值以数组的形式遍历出来，迭代对象中数组的索引值作为 key，数组元素作为 value。结果和 for…in 一致，但不会遍历原型属性**    
**1. 传入对象**
```
var obj = {name: '雏田',age: 18}
var x = Object.entries(obj)
```
**x 输出结果:**
>[['name', '雏田'], ['age', 18]]

**2. 传入数组**
```
var arr =  ["鸣人", "雏田", "佐助"]; 
var x = Object.entries(arr)
```
**x 输出结果:**
> [['0', "鸣人"], ['1', '雏田'], ['2', '佐助']]

**3. 传入数组中包含对象**
```
var arr =  [{name:"鸣人"}, "雏田", "佐助"]; 
var x = Object.entries(arr)
```
**x 输出结果:**
> [['0', { name: "鸣人" }], ['1', '雏田'], ['2', '佐助']]

**4. 传入字符串**
```
var string =  "123" 
var x = Object.entries(string)
```
**x 输出结果:**
>[["0", "1"], ["1", "2"], ["2", "3"]]

**5. 传入数字，浮点数**
```
var number =  123 
var number1 =  123.3 
var x = Object.entries(number)
var y = Object.entries(number1)
```
**x y输出结果:**
>都为 [ ]
#### every()
**检测数值元素的每个元素是都否都符合条件**
```
var number = [2, 34, 45, 1]
var x = number.every((i) =>
    i > 30
)
```
**x 输出结果:**
> false
#### some()
**检测数组元素中是否有元素符合指定条件**
**如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测**
**如果没有满足条件的元素，则返回false**
```
var number = [2, 34, 45, 1]
var x = number.some((i) =>
    i > 30
)
console.log(x)
```
**x 输出结果**
> true
#### filter()
**检测数值元素，并返回符合条件所有元素的数组**
```
var number = [2, 34, 45, 1]
var x = number.filter((i) =>
    i > 30
)
```
**x 输出结果:**
>[34, 45]
#### find()
**返回符合传入测试（函数）条件的数组元素**
**获取数组中年龄大于 30 的第一个元素**
```
var number = [2, 34, 45, 1]
var x = number.find((i) =>
    i > 30
)
```
**x 输出结果:**
>34
#### findIndex()
**返回符合传入测试（函数）条件的数组元素索引**
获取数组中年龄大于等于 30 的第一个元素索引位置
```
var number = [2, 34, 45, 1]
var x = number.findIndex((i) =>
    i > 30
)
```
**x 输出结果:**
>1
#### forEach()
**遍历数组中的每一位**
```
var arr = ["a", "b", "c", "d", "e", "f"];
var a = "";
var b = 0;
arr.forEach((value, index, array) => {
  console.log(value)
  console.log(index)
  console.log(array)
  console.log(array[index])
  a += (array[index])
  b += index
    })
console.log(a)
console.log(b)
```
**输出结果:**
>a b c d e f
>0 1 2 3 4 5
>["a", "b", "c", "d", "e", "f"] * 6
>a b c d e f
>abcdef
>15

<font size=5>forEach() 的 continue 与 break</font>

**forEach() 本身是不支持的 `continue` 与 `break` 语句的，我们可以通过 `some` 和 `every` 来实现** 
**continue 实现** 
**使用 return 语句实现 continue 关键字的效果：**
```
var arr = [1, 2, 3, 4, 5];
arr.forEach(function (item) {
    console.log(item)
    if (item === 3) {
        return;       //3的元素跳过
    }
    console.log(item);
});
```
**输出结果:**
> 1 2 3 4 5
> 1 2 4 5
```
var arr = [1, 2, 3, 4, 5];
arr.some(function (item) {
    if (item === 2) {
        return;     // 不能为 return false
    }
    console.log(item);
});
```
**输出结果:**
>1 3 4 5

**break 实现** 
```
var arr = [1, 2, 3, 4, 5];
arr.every(function (item) {
        console.log(item);
        return item !== 3;
        console.log("我被终止了")
});
```
**输出结果:**
>1 2 3
>不执行
#### from()
**`语法 : Array.from(object, mapFunction, thisValue)`**
`object : 必需，要转换为数组的对象`
`mapFunction : 可选，数组中每个元素要调用的函数`
`thisValue : 可选，映射函数(mapFunction)中的 this 对象`
**通过给定的对象中创建一个数组**
```
var arr = Array.from("RUNOOB")
console.log(arr)
```
**输出结果:**
>["R", "U", "N", "O", "O", "B"]
**下面的实例返回集合中包含的对象数组**
```
var setObj = new Set(["a", "b", "c"]);
var objArr = Array.from(setObj);
console.log(objArr[1] == "b")
```
**输出结果:**
> true
**下面的实例演示如何使用箭头语法和映射函数更改元素的值**
```
var arr = Array.from([1, 2, 3], x => x * 10);
console.log(arr[0]) 
console.log(arr[1]) 
console.log(arr[2]) 
```
**输出结果:**
>10
>20
>30
#### includes()
**判断一个数组是否包含一个指定的值**
```
var fruits = ["Banana", "Orange","Apple","Mango","pear"]
fruits.includes("Banana") 
fruits.includes("哈粒嘎")
```
**输出结果:**
>true
>false
#### isArray() 
**判断对象是否为数组：**
```
var obj = {id:"Banana"}
var fruits = ["Banana", "Orange","Apple","Mango","pear"]
console.log(Array.isArray(fruits))
console.log(Array.isArray(obj)) 
```
**输出结果:**
>true
>false
#### join()
**将数组转换为字符串拼接数组**
**里面的参数必须时字符串类型 ----->返回字符串类型**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var fruits1 = fruits.join();
var fruits2 = fruits.join("~");
console.log(fruits1)
console.log(fruits2)
```
**输出结果:**
>Banana,Orange,Apple,Mango
>Banana~Orange~Apple~Mango
#### keys()
**返回一个数组索引的迭代器**
```
var arr = ["a", "b","c"];   //传入对象
var obj = {a:123,b:345}     //传入数组
var str = 'ab1234';         //传入字符串
console.log(Object.keys(obj))
console.log(Object.keys(arr))
console.log(Object.keys(str))
```
**输出结果:**
>["a", "b"]
>["0", "1", "2"]
>["0", "1", "2", "3", "4", "5"]
#### indexOf()
**搜索数组中的元素，并返回它所在的位置**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var fruits1=["Banana","Orange","Apple","Mango","Banana","Orange","Apple"];
var a = fruits.indexOf("Apple");
var b = fruits1.indexOf("Apple");
console.log(a)
console.log(b)
```
**输出 a b 结果:**
>2 
>2
#### lastIndexOf()
**搜索数组中的元素，并返回它最后出现的位置**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var fruits1=["Banana","Orange","Apple","Mango","Banana","Orange","Apple"];
var a = fruits.lastIndexOf("Apple");
var b = fruits1.lastIndexOf("Apple");
console.log(a)
console.log(b)
```
**输出 a b 结果:**
>2
>6
#### map()
**对数组的每个元素调用定义的回调函数并返回包含结果的数组** 
**`语法 : array.map(function(currentValue,index,arr), thisValue)`**
`currentValue : 必须。当前元素的值`
`index : 可选。当前元素的索引值`
`arr : 可选。当前元素属于的数组对象`
`thisValue : 可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。如果省略了 thisValue，或者传入 null、undefined，那么回调函数的 this 为全局对象。`
```
var arr = [1, 3, 4, 6];
//Es5写法
let num = arr.map(function (value, index, array) {
    console.log(value)
    console.log(index)
    console.log(array)
    return value * 3;
})
console.log(num)
//ES6箭头函数写法
let newNum = arr.map((value, index, array) => value * 3)
```
**输出结果:**
>1 3 4 6
>0 1 2 3
>[1, 3, 4, 6]*4
>[3, 9, 12, 18]
#### slice()
**选取数组的一部分，并返回一个新数组**
**`array.slice(start, end)`**
`start(从该位开始截取) : 可选。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，slice(-2) 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）`
`end(截取该位结束) : 可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 slice(-2,-1) 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）`
**1个参数时：从该位截取，一直截取到最后**
```
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(2);
var citrus1 = fruits.slice(1,3)
console.log(fruits)
console.log(citrus)
console.log(citrus1)
```
**输出结果:**
>["Banana", "Orange", "Lemon", "Apple", "Mango"]
>["Lemon", "Apple", "Mango"]
> ["Orange", "Lemon"]
#### reduce()
**将数组元素计算为一个值（从左到右）**
**`语法: array.reduce(function(total, currentValue, currentIndex, arr), initialValue)`**
`total : 必需。初始值, 或者计算结束后的返回值`
`currentValue : 必需。当前元素`
`currentIndex : 可选。当前元素的索引`
`arr : 可选。当前元素所属的数组对象`
`initialValue : 可选。传递给函数的初始值`
```
var arr = [1, 3, 5, 7, 9];
var x = arr.reduce(function (x, y) {
    return x + y;
});
var y = arr.reduce(function (x, y) {
    return x * 10 + y;
});
console.log(arr)
console.log(x)
console.log(y)
```
**执行结果**
>[1, 3, 5, 7, 9]
>25
>13579
#### reduceRight()
**将数组元素计算为一个值（从右到左）**
```
var arr = [1, 3, 5, 7, 9];
var x = arr.reduceRight(function (x, y) {
    return x + y;
});
var y = arr.reduceRight(function (x, y) {
    return x * 10 + y;
}); 
console.log(arr)
console.log(x)
console.log(y)
```
**执行结果 :**
>[1, 3, 5, 7, 9]
>25
>97531
#### valueOf()
**返回数组对象的原始值**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var x=fruits.valueOf();
console.log(x)
```
**输出结果:**
>["Banana", "Orange", "Apple", "Mango"]     fruits.valueOf()与 fruits返回值一样
### 改变原数组  
#### copyWithin()
**`语法: array.copyWithin(target, start, end)`**
`target : 必需。复制到指定目标索引位置`
`start : 可选。元素复制的起始位置`
`end : 可选。停止复制的索引位置 (默认为 array.length)。如果为负值，表示倒数`
**从数组的指定位置拷贝元素到数组的另一个指定位置中**
**复制数组的前面两个元素到后面两个元素上**
```
var fruits = ["Banana", "Orange","Apple","Mango","pear"]
fruits.copyWithin(2, 0)
```
**fruits 输出结果:**
>["Banana", "Orange", "Banana", "Orange", "Apple"]

**复制数组的前面两个元素到第三和第四个位置上：**
```
var fruits = ["Banana", "Orange", "Apple", "Mango", "Kiwi", "Papaya"];
fruits.copyWithin(2, 0, 2);
```
**fruits 输出结果:**

>["Banana","Orange","Banana","Orange","Kiwi","Papaya"]
#### fill()
**`语法 : array.fill(value, start, end)`**
`value : 必需。填充的值`
`start : 可选。开始填充位置`
`end : 可选。停止填充位置 (默认为 array.length)`
**使用一个固定值来填充数组**
```
var fruits = ["Banana", "Orange", "Apple", "Mango", "pear"];
fruits.fill("cantaloupe");
```
**fruits 输出结果:**
>["cantaloupe", "cantaloupe", "cantaloupe", "cantaloupe", "cantaloupe"]
```
var fruits = ["Banana", "Orange", "Apple", "Mango", "pear"];
fruits.fill("cantaloupe", 2, 4);
```
**fruits 输出结果:**
>["Banana", "Orange", "cantaloupe", "cantaloupe", "pear"]
#### pop()
**删除数组的最后一个元素并返回删除的元素**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.pop())
console.log(fruits)
```
**输出结果:**
>Mango
> ["Banana", "Orange", "Apple"]
#### shift()
**删除数组的第一个元素并返回删除的元素**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift()
console.log(fruits)
```
**输出结果:**
> ["Orange", "Apple", "Mango"]
#### push()
**向数组的末尾添加一个或更多元素，并返回新的长度**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Kiwi")
console.log(fruits)
fruits.push("Lemon","Pineapple")
console.log(fruits)
```
**输出结果:**
>["Banana", "Orange", "Apple", "Mango", "Kiwi"]
>["Banana", "Orange", "Apple", "Mango", "Kiwi", "Lemon", "Pineapple"]
#### unshift()
**向数组的开头添加一个或更多元素，并返回新的长度**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.unshift("Kiwi")
console.log(fruits)
fruits.unshift("Lemon","Pineapple")
console.log(fruits)
```
**输出结果:**
>["Kiwi", "Banana", "Orange", "Apple", "Mango"]
>["Lemon", "Pineapple", "Kiwi", "Banana", "Orange", "Apple", "Mango"]
#### reverse()
**反转数组的元素顺序**
```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.reverse();
console.log(fruits)
```
**输出结果:**
> ["Mango", "Apple", "Orange", "Banana"]
#### sort()
**对数组的元素进行排序**
**排序的方法比较的是 ASCLL，如果正常排序，此时 sort( ) 里面需要写一个匿名函数**
```
arr.sort( function ( a, b ){
    return;
    });
```
**匿名函数注意事项:**
1. 必须写 2 形参
2. 看返回值： 当返回值为`负数`时，那么前面的数放在前面, 当返回值为`正数`时，那么后面的数在前,为 `0 `时,不动

```
var arr = [1, 3, 2, 5, -1];
arr.sort();
console.log(arr)
var arr1 = [1, 3, 5, 4, 10];
arr1.sort(); //这里比较的是ASCLL
console.log(arr1)
arr1.sort(function (a, b) {
    if (a > b) {
        return 1;
    } else {
        return -1;
    }
});
console.log(arr1);
```
**输出结果:**
>[-1, 1, 2, 3, 5]
>[1, 10, 3, 4, 5]
>[1, 3, 4, 5, 10]
#### splice()
**从数组中添加或删除元素**
**`语法: array.splice(index,howmany,item1,.....,itemX)`**
`index : 从第几位开始`
`howmany : 截取多少长度`
`item1,.....,itemX : 在切口处添加新的数据`
```
var arr = [1, 2, 3, 4, 5]
arr.splice(1, 2)
console.log(arr)
var arr1 = [1, 2, 3, 7, 8]
arr1.splice(3, 0, 4, 5, 6)
console.log(arr1)
```
**输出结果 :**
>[1, 4, 5]
>[1, 2, 3, 4, 5, 6, 7, 8]