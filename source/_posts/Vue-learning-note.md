---
title: Vue-learning-note
date: 2020-06-22 18:13:47
tags:
---

#  一、Vue基础

##  1 Vue安装

* **[Vue官网]( https://cn.vuejs.org/ )**

* **方式一：直接CDN引入**

  * 你可以选择引入开发环境版本还是生产环境版本

  ```
  <!-- 开发环境版本，包含了有帮助的命令行警告 --> 
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <!-- 生产环境版本，优化了尺寸和速度 -->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ```

* **方式二： 下载和引入**

```
开发环境 https://vuejs.org/js/vue.js 
生产环境 https://vuejs.org/js/vue.min.js
```

1. 官网下载vue.js   官网--->安装---->直接用`<script>`引入方式(下载文件就是vue.js)  -----  开发阶段下载开发版本
2. 在项目中新建js文件夹  再将vue.js文件  放入js文件夹下
3. 引入及使用

   * 创建Vue对象的时候，传入了一些options：{}
     * {}中包含了el属性：该属性决定了这个Vue对象挂载到哪一个元素上,以"id"确定元素。
     * {}中包含了data属性：该属性中通常会存储一些数据
       * 这些数据可以是我们直接定义出来的。
       * 也可能是来自网络，从服务器加载的。

```
    <div id="app">{{message}}
        <h1>{{name}}</h1>
    </div>
    <script src="../js/vue.js"></script>
    <script>
        const app = new Vue({
            el:'#app',
            data:{
                message:'你好呀  hello  vue',
                name : 'maxthon'
            }
        })
    </script>
```

* **方式三：NPM安装**

```js
npm install vue --save
```



##  2 Vue中的MVVM

* [MVVM维基百科](https://zh.wikipedia.org/wiki/MVVM)



  ![](Vue-learning-note/vue-mvvm.png)

* View

  * DOM层。
  * 给用户展示各种信息。

* Model

  * 数据层 -->传入Vue的options中的data

* VueModel

  * 视图模型层：View和Model沟通的桥梁。
  * 实现了Data Binding，也就是数据绑定，将Model的改变实时的反应到View中
  * 实现了DOM Listener，也就是DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。

  #  这是一次决定性的测试  命运于此