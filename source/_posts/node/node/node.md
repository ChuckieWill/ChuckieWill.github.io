---
title: node
date: 2020-03-02 10:46:17
tags:
- node
categories:
- [node, node]
---

node:mRw8LZz1ulLPMf5U

小程序：K57S1kGd4CLBz2dw

# node

##  1 node简介

###  1.1 node安装及下载

* [node下载官网](https://nodejs.org/en/) 

  * 若已安装node.js 但是终端node-v 显示不是内部命令  [解决方案](https://blog.csdn.net/qq_37248318/article/details/80839564?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1 )
  * Vscode终端运行node命令，提示“'node' 不是内部或外部命令   [解决方案](https://blog.csdn.net/LINHONG_1994/article/details/103068943)

* 终端检测node下载

  ```
  node -v
  //输出版本号即安装成功
  ```

* 终端检测npm下载

  ```
  npm -v
  //输出版本号即安装成功
  ```

###  2. 2  node技术介绍

* Node.js 
  * Node.js不能重复安装  若计算机上已有老版本 安装新版本之前要把老版本先卸载  或者 用nvm管理两个不同的版本
  * Node.js 的本质是让JS 可以脱离浏览器运行  而非是为了web开发 ，web开发只是其能力的一个方面
  * Node.js的能力与应用
    * **脱离浏览器运行JS**
    * **NodeJS Stream（前端工程化基础）**------开源的、框架级别的产品 
    * **服务端API**
    * **作为中间层 **------------ 中大型项目中才有中间层
  * Node.js 对ES6-10支持情况  
    * 目前Node.js  对ES6-10的一些特性是不支持的：1、import from导入方式不支持  需要用require  2、decorator装饰器 不支持  3、class类的属性不支持  必须用this.x = x  设置属性   （相对其他语言没有属性这一说  必须在构造函数中设置属性）
* npm:Node.js中的一个工具包
* koa：基于Node.js 专业开发web的框架
  * koa特点：洋葱圈模型  精简   使用时需要二次开发，否则会很难用





##  2 node 语法

#####  1 fs模块

* 读写文件模块---node环境自带，不需要安装

```
//引入fs模块
const fs = require('fs')

//写文件
//fileName :文件路径  date: 文件内容(字符串类型)
//writeFileSync方法：如果fileName文件不存在则创建fileName，若fileName文件存在则覆盖fileName中的内容
fs.writeFileSync(fileName,date)

//读文件
//fileName :文件路径  'utf8' : 解析文件内容的编码方式  若不写此项则返回为二进制
//返回值为字符串
fs.readFileSync(fileName,'utf8')
//读取本地要上传的文件并转化成二进制  path是本地文件路径
fs.createReadStream(path)  
```

#####  2 path模块

* 读取文件路径模块---node环境自带，不需要安装

```
//引入path模块
const path = require('path')

//获取绝对路径
//__dirname指定获取当前文件的绝对路径
const fileName = path.resolve(__dirname,'./access_token.json')
console.log(fileName)
//输出
C:\code\music-imooc\music-imooc-admin-backend\utils\access_token.json
```







#  node开发服务端API

* 服务端开发
  * 读写数据库
  * 开发API

##  1 node server项目自动重启配置

1. 全局安装`nodemon`

   ```js
   npm install nodemon -g
   ```

2. 启动项目

   ```js
   nodemon 文件名
   ```

   * 启动后，当修改文件内容后，项目会自动重启

*若不是全局安装则需要通过`npx nodemon 文件名`启动项目*

