---
title: npm
date: 2020-03-02 10:47:17
tags:
- npm
categories:
- [node, npm]
---

#  npm 

###  1 项目初始化

> 在开发React、Vue、koa等框架项目时（基于node时），需要初始化项目

* 命令：`npm init`

* 使用前提：终端切换到根目录下

* 初始化完成后会在根目录下生成`package.js`文件

  ```js
  //package.js
  {
    "name": "blink-backend", //项目名称
    "version": "1.0.0",  //项目版本号
    "description": "",   //项目描述
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "",
    "license": "ISC"
  }
  
  ```

  

###  2 npm命令

* --save-dev   :安装开发时依赖，项目打包后不需要继续使用的。

```
//webpack 只是一个模块打包工具 在真实上线是不需要这个依赖 所以只需要在开发时安装这个依赖
npm install webpack@3.6.0 --save dev
```

* --save  :安装发布时依赖

```
//Vue 在发布版中是需要这个依赖的  所以需要安装发布时依赖
npm install vue --save
```

* npm uninstall ： 卸载
* npm install :  将`package.js`中的库全部安装



