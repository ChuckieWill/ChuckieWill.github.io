---
title: koa
date: 2020-08-02 10:44:17
tags:
- koa
categories:
- [node, koa]
---

#  koa

##  1 koa简介

* **`koa`是基于`node`环境的后端开发框架**

* [koa官网]( https://koa.bootcss.com/ )

* 安装

  ```js
  //前提先安装node.js
  npm install koa
  ```

  * bug 

  ```js
  //问题（终端）：
  npm install出现Unexpected end of JSON input while parsing near错误
  
  //解决方案  终端输入命令：
  npm cache clean --force
  ```

##  2 koa使用流程

1. 在项目根目录下新建入口文件   一般为app.js (文件名自定)   在入口文件中导入koa、new koa、设置端口

* 一般称koa对象为  应用程序对象
* 以下3行代码即可启动koa框架
* 前端发送请求地址：localhost：3000（本地时）

```
const koa = requrie('koa')
const app = new koa()
app.listen(3000)  //该函数 接收端口号 启动koa框架
```



2. koa中间件的设置

* koa中间件返回的总是一个promise对象

```
app.use(async(ctx,next)=>{
	console.log('中间件1')
	const res = await next()    //-----若在此处接收next() 一定是一个promise对象
})

这里的res一定是一个promise对象   这里next()调用的是下一个中间件 
```

* app.use()方法中传入匿名函数
* async await是标配
* next()函数用于调用下一个函数  这里的next就简单的指中间件代码顺序上的下一个

```
app.js

const koa = requrie('koa')
const app = new koa()

app.use(async(ctx,next)=>{
	console.log('中间件1')
	await next()    //-----调用下方的’中间件2‘  若在此处接收next() 一定是一个promise对象
})

app.use(async (ctx,next)=>{
	console.log('中间件2')
})

app.listen(3000)  //该函数 接收端口号 启动koa框架
```

