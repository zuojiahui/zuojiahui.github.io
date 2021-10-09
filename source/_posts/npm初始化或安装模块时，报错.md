---
title: npm初始化或安装模块报错
categories: npm
tags:
  - npm
abbrlink: 7920
---
### 解决方案
报错信息如图:
<div>    
<img src="https://gitee.com/zuo_jiahui/blogimage/raw/master/img/微信截图_20210718201153.png" width = 100% height = 100% />
</div> 

网上有说是版本不一致的原因，那么检测 npm 的版本

```
npm -v
更新npm版本命令:
npm install npm -g 要记住全局更新
cnpm install npm -g 淘宝镜像会比较快
```
安装完之后查看npm -v，还是之前的版本，说明本地已经是最新版本了。不是版本原因
清除npm的缓存 
```
npm cache clean --force
```
然后再运行我们需要安装模块的命令
有时是网络问题，依赖包加载不完整，删掉node_modules文件后，重新执行 `npm install`即可
