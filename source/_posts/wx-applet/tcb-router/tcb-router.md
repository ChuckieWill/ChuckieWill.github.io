---
title: tcb-router
date: 2020-03-29 10:46:17
tags:
- tcb-router
categories:
- [wxapplet, tcb-router]
---


###  1 介绍

* **`tcb-router`： 小程序->云开发->云函数 koa 路由工具 **
* **github地址:   **[tcb-router]( https://github.com/TencentCloudBase/tcb-router)

###  2 安装

```
npm install --save tcb-router

//小程序调用
$url: "user", // 要调用的路由的路径，传入准确路径或者通配符*
```

###  3 使用

```
//导入云函数
const TcbRouter = require('tcb-router');

//new新对象
const app = new TcbRouter({ event });

//路由正文

//结尾
return app.serve();
```

###  4 小程序端调用

* 相对普通的云函数调用只是多了`$url`属性
* `$url`:要调用的路由的路径

```js
// 调用名为 router 的云函数，路由名为 user
wx.cloud.callFunction({
    // 要调用的云函数名称
    name: "router",
    // 传递给云函数的参数
    data: {
        $url: "user", // 要调用的路由的路径，传入准确路径或者通配符*
        other: "xxx"
    }
});
```

