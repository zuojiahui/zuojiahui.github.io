---
title: Markdown常用语法
categories: Markdown
tags:
  - Markdown
abbrlink: 52877
date: 2020-10-18 12:35:03
---
## 前言
Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。  
Markdown具有一系列衍生版本，用于扩展Markdown的功能，这些功能原初的Markdown尚不具备，它们能让Markdown转换成更多的格式，例如LaTeX，Docbook。Markdown 语言在 2004 由约翰·格鲁伯（John Gruber）创建。
## 用途  
Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客等。  
当前许多网站都使用了Markdown来撰写帮助文档或是用于论坛上发表消息。例如简书、知乎、CSDN、GitHub、OpenStreetMap 、Diaspora等。  
## Markdown基础语法
### 1. 标题
在想要设置为标题的文字前面加#来表示  
一个#是一级标题，两个#是二级标题，以此类推。支持六级标题，标题字号逐级递减降低  
**注意：语法在#后跟个空格再写文字**
**示列:**
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
 ```
**效果如下:**  
<div align=center>
<img src="https://img-blog.csdnimg.cn/20200426182643977.png?" width = 50% height = 50%>
</div>

### 2. 字体
斜体     
要倾斜的文字左右分别用一个 * 号包起来 
加粗  
要加粗的文字左右分别用两个 * 号包起来   
斜体加粗    
要倾斜和加粗的文字左右分别用三个 * 号包起来  
删除线  
要加删除线的文字左右分别用两个~~号包起来  
**示列:**
```
*这是倾斜的文字*
**这是加粗的文字**
***这是斜体加粗的文字***
~~这是加删除线的文字~~
```
**效果如下:**  
*这是倾斜的文字*  
**这是加粗的文字**  
***这是斜体加粗的文字***  
~~这是加删除线的文字~~  
### 3. 分割线
三个 - 或者 *
**示列:**
```
---
***
```
**效果如下:**

---
***
### 4. 引用  
在需要引用的文字前加 >
**示列:**
```
>大家好，你们好
```
**效果如下:**
>大家好，你们好
### 5. 图片 
![图片下方显示名字](“图片url 鼠标放在图片上的显示信息”)
**示列:**
```
![我是皮卡丘](https://img-blog.csdnimg.cn/20200507103002365.png "皮卡皮卡")
```
**效果如下:**
<div align=center>

![我是皮卡丘](https://img-blog.csdnimg.cn/20200507103002365.png "皮卡皮卡")
</div>

### 6. 超链接
```
[网址名](网址) 
```
**示列:**
```
[GitHub](https://github.com/)
[百度](https://www.baidu.com/) 
```
[GitHub](https://github.com/)
[百度](https://www.baidu.com/) 
### 7. 列表
**有序列表:**  
数字加上 . (后面跟上一个空格)
**示列:**
```
1. 有序列表
2. 有序列表
3. 有序列表
```
1. 有序列表
2. 有序列表
3. 有序列表

**无序列表:**  
内容前面加上 *，+，- (后面跟上一个空格)
**示列:**
```
* 无序列表
+ 无序列表
- 无序列表
```
* 无序列表
+ 无序列表
- 无序列表

### 8. 代码插入
单行代码:  
用两个`把代码内容包起来  
'包裹的代码'
**示列:**

`Hello World`

代码块:  
用 两个```把代码块包起来  
**示列:**
```
var num =(function(a, b ,c){
    var d = a + b + c + 1;  
    return d
}(1,2,3))
```
## Markdown进阶
### 1. 文字缩进
在要缩进的文字前使用 `&emsp;`(注意后面要加上空格)  
**示列:**
我是缩进前的文字  
&emsp; &emsp; &emsp; 我是缩进后的文字
### 2. 文字居中
对于标准的markdown文本，是不支持居中对齐的。但是markdown支持html语言，所以可以采用html语法格式来实现
**示列:**
```
<center>我要居中显示</center>
```
<center>我要居中显示</center>

### 3. 字体字号及颜色
同样采用HTML语法实现  
**示列:**
```
<center><font face="微软雅黑" size=8 color=red>我是一行文本</font></center>
```
<center><font face="微软雅黑" size=8 color=red>我是一行文本</font></center>

### 4. 图片的大小和位置  
center 居中 ，center换成 left 和 right 可以实现居左，居右  
width 和 height 调控宽度和高度  
**示列:**

```
<div  align="center">    
<img src="https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3222272914,986927614&fm=26&gp=0.jpg" width = 50% height = 50% />
</div>  
```
<div  align="center">    
<img src="https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3222272914,986927614&fm=26&gp=0.jpg" width = 50% height = 50% />
</div>  

## Markdown更多用法
[Markdown 中文文档](https://markdown-zh.readthedocs.io/)