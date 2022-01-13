---
title: ECharts在vue2中的使用
date: 2021-11-05 10:43:17
tags:
- vue
- ECharts
categories:
- [vue]
---

#  ECharts在vue2中的使用

> [Echarts官网](https://echarts.apache.org/zh/index.html)

##  1 基本使用

* 安装

```js
npm install echarts --save
```

* 引入及全局挂载

```js
//vue的原型上挂载echarts
//在main.js文件中
import * as echarts from 'echarts';
Vue.prototype.$echarts = echarts;
```

