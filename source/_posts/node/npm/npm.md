---
title: npm
date: 2020-08-02 10:47:17
tags:
- npm
categories:
- [node, npm]
---

#  npm 命令

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



