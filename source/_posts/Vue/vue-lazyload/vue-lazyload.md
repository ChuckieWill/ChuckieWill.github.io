---
title: vue-lazy-load
date: 2020-04-09 10:46:17
tags:
- vue
categories:
- [vue]
---
#  vue-lazyload

**`vue-lazy-load`用于实现图片懒加载，即在需要图片显示时才加载图片**

##  介绍及使用

* github:  [vue-lazyload]( https://github.com/hilongjw/vue-lazyload )

###  基础使用

* 1 安装

```
npm install vue-lazyload --save
```

* 2 导入（main.js）

```
import VueLazyLoad from 'vue-lazyload'
```

* 3 Vue.use安装

```
Vue.use(VueLazyLoad)
```

* 4 ：src-->v-lazy

```html
//原生
<img src="./img.png"/>
<img :src="./img.png"/>  //vue中传值时

//使用懒加载
<img v-lazy="./img.png"/>
```

###  带参使用

* Vue.use安装带参

```
Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: 'dist/error.png',
  loading: 'dist/loading.gif',
  attempt: 1
})
```

* 参数列表

| key               | description                                 | default                                                      | options                                                      |
| ----------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `preLoad`         | proportion of pre-loading height            | `1.3`                                                        | `Number`                                                     |
| `error`           | src of the image upon load fail             | `'data-src'`                                                 | `String`                                                     |
| `loading`         | src of the image while loading              | `'data-src'`                                                 | `String`                                                     |
| `attempt`         | attempts count                              | `3`                                                          | `Number`                                                     |
| `listenEvents`    | events that you want vue listen for         | `['scroll', 'wheel', 'mousewheel', 'resize', 'animationend', 'transitionend', 'touchmove']` | [Desired Listen Events](https://github.com/hilongjw/vue-lazyload#desired-listen-events) |
| `adapter`         | dynamically modify the attribute of element | `{ }`                                                        | [Element Adapter](https://github.com/hilongjw/vue-lazyload#element-adapter) |
| `filter`          | the image's listener filter                 | `{ }`                                                        | [Image listener filter](https://github.com/hilongjw/vue-lazyload#image-listener-filter) |
| `lazyComponent`   | lazyload component                          | `false`                                                      | [Lazy Component](https://github.com/hilongjw/vue-lazyload#lazy-component) |
| `dispatchEvent`   | trigger the dom event                       | `false`                                                      | `Boolean`                                                    |
| `throttleWait`    | throttle wait                               | `200`                                                        | `Number`                                                     |
| `observer`        | use IntersectionObserver                    | `false`                                                      | `Boolean`                                                    |
| `observerOptions` | IntersectionObserver options                | { rootMargin: '0px', threshold: 0.1 }                        | [IntersectionObserver](https://github.com/hilongjw/vue-lazyload#intersectionobserver) |
| `silent`          | do not print debug info                     | `true`                                                       | `Boolean`                                                    |