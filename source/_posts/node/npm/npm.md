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



###  3 nrm

> 安装国内其它镜像源（包括taobao镜像源）

```js
安装：npm install nrm -g
```

**主要使用ls和use命令**

* 1)`nrm ls`是列出来现在已经配置好的所有的原地址
  * [运行`npm ls`报错解决方案](https://www.jianshu.com/p/cea5f163cb53)

```
nrm ls

  npm ---- https://registry.npmjs.org/
* cnpm --- http://r.cnpmjs.org/
  taobao - http://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn
  npmMirror  https://skimdb.npmjs.com/registry
```

* 2)`nrm use`是切换到哪个源上

```js
nrm use npm
```

* 3)`nrm add`添加源
* 4)`nrm del`删除源
* 5)`nrm test`测试源的响应时间，可以作为使用哪个源的参考