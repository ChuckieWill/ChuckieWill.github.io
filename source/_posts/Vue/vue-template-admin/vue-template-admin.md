---
title: vue-templete-admin
date: 2020-04-10 10:46:17
tags:
- vue
categories:
- [vue]
---
#  vue-templete-admin

* **`vue-templete-admin`是一个后台管理系统的前端模板**
* 组件开发依赖于`element UI`  [element-UI官网]( https://element.eleme.cn/#/zh-CN )

##  1 vue-admin-template（简版）

* GitHub地址： [vue-admin-template](https://github.com/PanJiaChen/vue-admin-template/blob/master/README-zh.md)

* 安装

  ```
  # 克隆项目
  git clone https://github.com/PanJiaChen/vue-admin-template.git
  
  # 进入项目目录
  cd vue-admin-template
  
  # 安装依赖
  npm install
  
  # 建议不要直接使用 cnpm 安装以来，会有各种诡异的 bug。可以通过如下操作解决 npm 下载速度慢的问题
  npm install --registry=https://registry.npm.taobao.org---------可以不用
  
  # 启动服务
  npm run dev
  ```

* 用户名和密码

  ```
  admin
  111111
  ```

## 2 vue-template-admin(详版)

* github地址： [vue-template-admin](https://github.com/PanJiaChen/vue-element-admin/blob/master/README.zh-CN.md )

## 3  注意事项

* //后端方法返回值中要有code:20000

```js
//后端方法返回值中要有code:20000
ctx.body = {
            data,
            code:20000 //vue-admin-template前端模板要求的
        }
```

