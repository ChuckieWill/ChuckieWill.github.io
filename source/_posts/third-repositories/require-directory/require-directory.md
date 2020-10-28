---
title: require-directory
date: 2020-10-17 10:33:22
tags:
- require-directory
categories:
- [third-repositories, require-directory]

---



#  require-directory

##  1 简介

> [require-directory官方文档](https://www.npmjs.com/package/require-directory)

* 安装

  ```js
  npm i require-directory
  ```

* 使用场景

  * 用于对模块的批量导入



##  2 使用

###  2.1 基础用法

* `'./api/vi'`:  将该路径下的所有文件导出的模块放到`modules`中
  * `'./api/vi'`路径下有多个文件夹，也会依次遍历所有文件夹，将所有文件导出的模块都拿到
  * 通过遍历`modules`即可拿到导出的每个模块
  * **注意点：**这里拿到的模块`r` 根据原文件导出的方式不同会有不同，所以需要计提情况具体处理

```js
const modules = requireDirectory(module, './api/v1')

for(let r of modules){
    console.log(r)
}
```



###  2.2 高级用法

* `'./api/vi'`:  作用同基础用法
* `{visit: whenLoadModu**le}`： 自定义回调函数，每次遍历到一个导出模块后，都会调用这个自定义函数
* **注意点：**这里拿到的模块`obj` 根据原文件导出的方式不同会有不同，所以需要计提情况具体处理

```js
requireDirectory(module, './api/v1', {
  visit: whenLoadModule
})

//obj即是遍历到的当前模块
function whenLoadModule(obj) {
  //对模块判断，并做需要的处理
  if(obj instanceof Router){
    app.use(obj.routes())
  }
}
```

