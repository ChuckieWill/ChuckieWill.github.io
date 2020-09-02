---
title: chrome插件
date: 2020-01-10 10:00:00
tags:
- chrome插件
categories:
- [tools, chrome]
---

# chrome插件
##  vuejs devtools

* `vuejs devtools `用于查看`vue`框架项目的页面数据
* 安装步骤：

1. github下载vue-devtools插件  github下载地址： [https://github.com/vuejs/vue-devtools ]( https://github.com/vuejs/vue-devtools )
   * 直接下载.zip文件
   * 下载master: vue-devtools-master
2. npm install
   * cmd中cd 到vue-devtools-master文件夹下
   * 执行`npm install`
   * 再执行`npm run build`

3. chrome中引入插件
   * 位置：vue-devtools-master\shells\chrome





##  react-developer-tools

* `react-developer-tools`用于查看`React`框架开发的项目的页面数据

* 安装步骤

1. 下载地址：

   链接：https://pan.baidu.com/s/1ITWCYbcq-QTrjq2s9i7Urw 
   提取码：c2tl

   链接：https://pan.baidu.com/s/15f9sX_ZRXM5TDzWbiWLUxg 
   提取码：0i92

2. 将下载的文件解压到自定义目录中

3. 在`Chrome`浏览器扩展程序-加载已解压的扩展程序  中选择 之前解压的文件夹

4. 重新打开浏览器即可开始使用



##  redux-devtools

* `redux-devtools`用于查看`React`框架开发的项目的`store`全局数据

* 安装步骤

1. 下载地址：

   链接：https://pan.baidu.com/s/14N1MTHxGDAhASjhgNHvAvw 
   提取码：q8hs

2. 将下载的文件解压到自定义目录中

3. 在`Chrome`浏览器扩展程序-加载已解压的扩展程序  中选择 之前解压的文件夹

4. 在项目`store/index.js`中做如下配置

   * 增加：`window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()`

   ```js
   import { createStore } from 'redux'
   import reducer from './reducer'
   
   const store  = createStore(reducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
   
   export default store
   ```

5. 重新打开浏览器即可开始使用

* 常见问题：

  * `window.__REDUX_DEVTOOLS_EXTENSION__`本质是一个中间件
  * 当`store/index.js`中还有其它中间件例如`Redux-thunk`时，需要重新配置`store/index.js`

  ```js
  import { createStore, applyMiddleware, compose } from 'redux'
  import thunk from 'redux-thunk';
  import reducer from './reducer'
  
  const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?  window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;
  const enhancer = composeEnhancers(
    applyMiddleware(thunk)
  );
  
  const store  = createStore(
    reducer, 
    enhancer
  )
   
  export default store
  ```

  * [官网解决方案]( https://github.com/zalmoxisus/redux-devtools-extension#12-advanced-store-setup )

