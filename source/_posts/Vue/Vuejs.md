---
title: Vue
date: 2020-06-10 10:46:17
tags:
- vue
categories:
- [vue]
---

 

#  一、Vue基础

> **[Vue官网-Vue2.x]( https://cn.vuejs.org/ )**
>
> **[Vue官网-Vue3](https://v3.cn.vuejs.org/ )**
>
> [Vue官网导航使用指南视频](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=3)

##  1 Vue安装及使用

###  1.1 直接使用`<script>`引入

> 以下安装及使用基于`Vue2.x`

**引入方式： **

* 方式一：直接CDN引入
  * 官网位置：官网-学习-教程-安装-直接用`<script>`引入-CDN
  * 可以选择引入开发环境版本还是生产环境版本，将相应地址在html中通过`<script>引入`

```js
<!-- 开发环境版本，包含了有帮助的命令行警告 --> 
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```



* 方式二： 下载和引入
  * 官网位置：官网-学习-教程-安装-直接用`<script>`引入
  * 选择开发版本或生产版本直接下载一个js文件（开发阶段下载开发版本）
  * 将下载的文件再在html文件中通过`<script>`引入

```js
开发环境 https://vuejs.org/js/vue.js   //下载的文件名：vue.js
生产环境 https://vuejs.org/js/vue.min.js   //下载的文件名：vue.min.js
```

**使用Vue:**

> 目录结构
>
> * 根文件夹
>   * test.html
>   * src
>     * vue.js  

1. 在项目中新建js文件夹 ,若是以上方式二引入Vue则需要将下载的vue.js文件 放入js文件夹下
3. 在项目中新建html文件，并通过`<script>`引入Vue
* 方式一则引入的src为网络地址
   * 方式二则引入地址为本地地址

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>vue-demo</title>
  <script  src="./src/vue.js"></script>  //方式二引入
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>//方式一引入
</head>
<body>
</body>
</html>
```

3. 创建Vue对象，传入options：{}

   * {}中包含了el属性：该属性决定了这个Vue对象挂载到哪一个元素上,以"id"确定元素。

   * {}中包含了data属性：该属性中通常会存储一些数据
     * 这些数据可以是我们直接定义出来的。
     * 也可能是来自网络，从服务器加载的。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>vue-demo</title>
  <script src="./src/vue.js"></script>
</head>
<body>
  <div id="app">{{message}}
    <h1>{{name}}</h1>
  </div>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              message:'你好呀  hello  vue',
              name : 'maxthon'
          }
      })
  </script>
</body>
</html>
```

###  1.2 npm及CLI安装

**npm直接安装vue**

```js
npm install vue 
```

**使用脚手架**

1. (仅第一次执行)∶全局安装@vue/cli
   `npm install -g @vue/cli`

2. *切换到要创建项目的目录*，然后使用命令创建项目, 执行后会弹出选择安装vue2.x或vue3，自行选择；创建好的项目就已经接受git托管了

   `vue create 项目名称`

3. 启动项目
   `npm run serve`



###  1.3 单页组件

> [单页组件视频教程](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=60)

###  1.4 初始化项目



##  2 Vue中的MVVM

* [MVVM维基百科](https://zh.wikipedia.org/wiki/MVVM)

  ![](Vuejs/vue-mvvm.png)

* View

  * DOM层。
  * 给用户展示各种信息。
  * 对应Vue中的模板：el

* Model
  
  * 数据层 
  * 对应Vue的options中的data
* VueModel
  * 视图模型层：View和Model沟通的桥梁。
  * 实现了Data Binding，也就是数据绑定，将Model的改变实时的反应到View中
  * 实现了DOM Listener，也就是DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。
  * 对应Vue中的Vue实例对象

##  3 传入Vue的options

* 创建Vue对象的时候，传入了一些options：{}
  * {}中包含了el属性：该属性决定了这个Vue对象挂载到哪一个元素上。
  * {}中包含了data属性：该属性中通常会存储一些数据
    * 这些数据可以是我们直接定义出来的。
    * 也可能是来自网络，从服务器加载的。
  * {}中包含了methods属性：该属性中定义了函数
  * {}中包含了computed 属性:  计算属性

```js
<div id="app">{{message}}
    <h1>{{name}}</h1>
    <ul>
        <li v-for="item in movies">{{item}}</li>
    </ul>
    <h1>当前计数：{{count}}</h1>
    <button v-on:click="add">+</button>
    <button v-on:click="sub">-</button>
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
        el:'#app',
        data:{
            message:'你好呀  hello  vue',
            name : 'jack ma',
            movies:['心灵捕手','阿甘正传','星际穿越'],
            count:0
        },
        methods:{
            add(){
                console.log('add')
                this.count++
            },
            sub(){
                console.log('sub')
                this.count--
            }
        }
    })
</script>
```

##  4 Vue生命周期

* 挂载
* 更新
* 销毁

<img src="Vuejs/vue-lifetime.png" style="zoom: 67%;" />

###  4.1 生命周期各时间节点易错点

1. **created()中获取不了组件及元素**

​       *报错原因：*

* el是在created（）周期函数之后才挂载到Vue中的
* 所以在created（）中获取的组件或元素为空

​      *解决办法：*

* 将获取组件及元素的操作放到mounted（）周期函数中执行

2. **mouted()中可能数据还没有加载完成**

* mouted()在el挂载完成后执行

3. **updated()中DOM渲染还没有完成**

* updated()在数据更新到DOM后执行

4. **$nextTick()中图片还没有加载完成**

* $nextTick(()=>{回调函数}) 在DOM渲染完成后执行

  * updated()钩子函数是在是数据更新到DOM完成的时间节点
  * nextTick()函数是在数据更新且DOM渲染完成后的时间节点(*DOM渲染完成 不包括图片加载完成*)

```
this.$nextTick(() => {
	//DOM渲染完成后需要执行的操作
})
```

## 5 vue.config.js配置

* vue.config.js 文件放在根目录下

```js
module.exports = {
    devServer:{
        port: 8999, //默认打开端口
        open: tru  //自动在浏览器打开
    }

}
```



#  二、Vue基础语法

##  1 插值操作

#### mustache语法（双大括号）

* 数据绑定是**响应式**的

#### v-once

* 该指令表示元素和组件只渲染一次，view层展示的数据不会随着model层数据的变化而改变。

```js
<div id="app">
    <h1 v-once>{{name}}</h1>
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
        el:'#app',
        data:{
            name : 'jack ma'
        }
    })
</script>
```

#### v-html

* 该指令用于解析html代码
* 当数据内容本身就是html代码时可以只用这个指令解析并展示
* *v-html :会有XSS风险，会覆盖子组件*

![1590900171394](Vuejs/v-thml.png)

####  v-text

* v-text作用和Mustache比较相似：都是用于将数据显示在界面中
* v-text通常情况下，接受一个string类型

```js
<div id="app">
    <h1 >{{name}}</h1>---------1
    <h1 v-text="name"></h1>----2
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
        el:'#app',
        data:{
            name : 'jack ma'
        }
    })
</script>


//展示效果相同
jack ma------1
jack ma------2
```

####  v-pre

* v-pre用于跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法。

```js
<div id="app">
    <h1 >{{name}}</h1>---------1
    <h1 v-pre >{{name}}</h1>----2
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
        el:'#app',
        data:{
            name : 'jack ma'
        }
    })
</script>


//展示效果相同
jack ma------1
{{name}}------2
```

####  v-cloak

* 在vue实例编译完成之前，浏览器会直接显示未编译的Mustache标签。
* 为了避免这个问题，设置`v-cloak`指令可以 不显示Mustache标签，直到编译结束再显示编译之后的效果
* [v-cloak]{} 设置未编译完成之前的显示效果

```js
<div id="app">
    <h1 v-cloak>{{name}}</h1>
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
        el:'#app',
        data:{
            name : 'jack ma'
        }
    })
</script>
<style>
  [v-cloak] {
  	display: none;  //设置为不显示
  }       
</style>
```

##  2 绑定属性

####  v-bind

* `v-bind`用于： 1绑定一个或多个属性；2向组件传递props值

* 可以动态绑定的属性：图片链接`src`、网站链接`href`、类、样式等

* **`v-bind`的语法糖：`:`** 

  ```js
  v-bind:src="http:\\a.png"    等同于    :src="http:\\a.png"
  ```

####  v-bind绑定class

* 1 对象语法

  ```html
  //'active'、'line'为类名
  //isActive、isLine  为标志位  只有true、false取值  控制类是否生效
  <h2 class="title" :class="{'active': isActive, 'line': isLine}">Hello World</h2>
  ```

* 2 数组语法

  * 为类名时带`引号`

  ```html
  //'active'、'line'为类名
  <h2 class="title" :class=“['active’, 'line']">Hello World</h2>
  ```

  * 为变量名时不带引号  

  ```js
  <div id="app">
      <h1 :class="[active, line]">{{name}}</h1>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              active : {color: red},
              ling: {font-size: 10px}
          }
      })
  </script>
  ```

####   v-bind绑定style

* 1 对象语法

  * style后面跟的是一个对象类型
    * 对象的key是CSS属性名称
    * 对象的value是具体赋的值，值可以来自于data中的属性

  ```js
  :style="{color: currentColor, fontSize: fontSize + 'px'}"
  ```

* 2 数组语法

  ```html
  <div :style="[baseStyles, overridingStyles]"></div>
  
  data: {
  	baseStyles:{backGroundColor:'red'}  //注意转换成驼峰形势
  }
  ```



##  3 计算属性

####   computed

* `computed`用于将数据进行转化、多个数据相结合再提供一个简单的属性绑定到html模块中

  * 案例

  ```js
  <div id="app">
      <h1>{{fullName}}</h1>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              firstName: 'jack',
              lastName: 'ma'
          },
          computed: {
              fullName(){
                  return this.firstName+ ' '+ lastName
              }
          }
      })
  </script>
  //效果
  jack ma
  ```

* `compoouted`的`get`和`set`

  * `computed`中定义的计算属性本来是包含`get`和`set`方法，`get`用于获取计算属性的结果，`set`用于设置计算属性，但是一般都只需要获取`get`，所以就简写成上面的函数形式，直接返回计算结果
  * 双向绑定（`v-model`）时，必须有`set 和 get`,否则会报错
* 案例
  
  ```js
  <div id="app">
      <h1 >{{fullName}}</h1>
      <input v-model="double2"/>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              firstName: 'jack',
              lastName: 'ma',
              num: 0
          },
          computed: {
              fullName: {
                  get(){
                      return this.firstName+ ' '+ lastName
                  },
                  set(newName) {
                      const names = newName.split(' ‘)
                      this.firsName = names[0]
                      this,lastName = names[1]
                  }
              },
               double2: {
                  get() {
                      return this.num * 2
                  },
                  set(val) {
                      this.num = val/2
                  }
              }
          }
      })
  </script>
  //效果
  jack ma
  ```

####  computed VS methods

* computed 和 methods中定义的方法都可以达到对data数据加工的效果
* 但是：**计算属性会进行缓存，如果多次使用时，计算属性只会调用一次**  即只会计算一次  然后将计算结果保存，避免了重复无用的计算，节省了计算能力。



##  4 watch

> 当属性发送变化时就会触发

* watch 监听引用类型，拿不到oldVal, 因为oldVal 和 val都是存储的引用地址，而引用地址是没有变化的只是引用的堆中的值类型发生了变化

```js
<template>
    <div>
        <input v-model="name"/>
        <input v-model="info.city"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '双越',
            info: {
                city: '北京'
            }
        }
    },
    watch: {
        name(oldVal, val) {
     		// 值类型，可正常拿到 oldVal 和 val
            console.log('watch name', val, oldVal)
        },
        info: {
            
            handler(oldVal, val) {
                // 引用类型，拿不到 oldVal 。因为指针相同，此时已经指向了新的 val
                console.log('watch info', val, oldVal) 
            },
            deep: true, // 深度监听
            immediate: true  // 该回调将会在侦听开始之后被立即调用
            
        },
        // 'info.city':{
        //   handler(oldVal, val) {
                   // 此处是值类型，也可正常拿到 oldVal 和 val
        //         console.log('watch city', val, oldVal) 
        //     }
        // },
        'info.city'(oldVal, val) {
            // 此处是值类型，也可正常拿到 oldVal 和 val
            console.log('watch city', val, oldVal) 
        }
    }
}
</script>
```



##  5 事件监听

####  v-on

* [v-on官方教程](https://cn.vuejs.org/v2/api/#v-on)
* `v-on`监听的事件包括`DOM`原生事件和组件自定义事件

* `v-on`的语法糖：`@`

* 案例

  * 三个点击按钮的效果一样

  ```js
  <div id="app">
      <h1>{{counter}}</h1>
  	<button v-on:click="counter++">点击counter+1</button>
  	<button v-on:click="onCounter">点击counter+1</button>
  	<button @:click="onCounter">点击counter+1</button>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              counter: 0
          },
          monteds: {
              onCounter(){
                  this.counter++
              }
          }
      })
  </script>
  ```

####  v-on参数

* 如果`v-on`方法不需要额外参数，那么方法后的()可以不添加

  * 不带`()`时，如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去

* 如果`v-on`需要传入某个参数，同时需要event时，可以通过$event传入原生事件。

* 案例

  ```js
  <div id="app">
      <h1>{{counter}}</h1>
  	<button @:click="addOne">点击counter+1</button>
  	<button @:click="addTen(10, $event)">点击counter+10</button>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              counter: 0
          },
          monteds: {
              addOne(event){
                  console.log(event)
                  this.counter++
              },
              addTen(count, event){
                  this.counter += count
                  console.log(event)
              }
          }
      })
  </script>
  ```

####  v-on 修饰符

* `.stop`: 阻止冒泡
* `.prevent`: 阻止默认事件的调用  例如form的submit(提交)
* `.native `:监听组件根元素的原生事件。
* `.once` :只触发一次回调。

####  DOM原生事件列表

**[DOM原生事件列表](https://developer.mozilla.org/zh-CN/docs/Web/Events)**

##  6 条件判断

####  v-if、v-else-if、v-else

* v-if的原理
  
* v-if后面的条件为false时，对应的元素以及其子元素不会渲染。也就是根本不会有对应的标签出现在DOM中。
  
* 案例

  ```js
  <div id="app">
      <div v-if=“score>=90”>优秀</div>
      <div v-else-if=“score>=60”>及格</div>
      <div v-else>不及格</div>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              score: 99
          }
      })
  </script>
  ```

####  v-if VS  v-show

* v-if条件为false时，对应的元素以及其子元素不会渲染。(通过Vue控制)
* v-show条件为false时，对应的元素以及其子元素会渲染。只是将元素的display属性设置为none，所以没有显示出来。（通过css控制）
* 总结：
  * 当需要在显示与隐藏之间切片很频繁时，使用v-show
  * 当只有少次切换时，通常使用v-if

##  7 遍历循环

####  v-for

* 遍历数组

  * item: 数组中的元素
  * index： 数组中元素的索引

  ```js
  v-for=(item, index) in items
  ```

* 遍历对象

  ```js
  (属性值，属性名，属性索引)  in   info 
  (value, key,  index) in info 
  ```

####  :key

* **key的作用主要是为了高效的更新虚拟DOM。**

* 当某一层有很多相同的节点时，也就是列表节点时，我们希望插入一个新的节点，在B和C之间加一个F

  ![1590913636403](Vuejs/diff-f.png)

  * Vue的虚拟DOM的Diff算法的处理方式是：C更新成F，D更新成C，E更新成D，最后再插入E，所以效率很低

    ![](Vuejs/diff-e.png)

  * 使用key

    * key相当于给每个节点做了唯一的标识
    * 这样就可以直接插入到B、C直接  效率更高

##  8 双向绑定

####  v-model

* `v-model`指令用于实现表单元素和数据的双向绑定
* 可使用标签：`input`、`textarea`

* 案例

  * 双向绑定流程
    * 因为input中的v-model绑定了message，所以会实时将输入的内容传递给message，message发生改变。
    * 当message发生改变时，因为上面我们使用Mustache语法，将message的值插入到DOM中，所以DOM会发生响应的改变。

  ```js
  <div id="app">
      <input type="text" v-model="message">
      <h2>{{message}}</h2>
  </div>
  <script src="../js/vue.js"></script>
  <script>
      const app = new Vue({
          el:'#app',
          data:{
              message: ''
          }
      })
  </script>
  ```

* **v-model原理**

  * 1.v-bind绑定一个value属性
  * 2.v-on指令给当前元素绑定input事件，将输入赋值给data中的数据

  ```js
  <input type="text" v-model="message">
  等同于
  <input type="text" v-bind:value="message" v-on:input="message = $event.target.value">
  ```

####  v-model:radio

* 用于多个单选框

<img src="Vuejs/v-model-radio.png" style="zoom:75%;" />

####  v-model:checkbox

* 单个勾选框：
  * v-model即为布尔值.
  * 此时input的value并不影响v-model的值。
* 多个复选框：
  * 当是多个复选框时，因为可以选中多个，所以对应的data中属性是一个数组。
  * 当选中某一个时，就会将input的value添加到数组中。

<img src="Vuejs/v-model-checkbox.png" style="zoom:75%;" />

<img src="Vuejs/v-model=checkbox-1.png" style="zoom:75%;" />

####   v-model:select

* 单选：
  * 只能选中一个值。
  * v-model绑定的是一个值。
  * 当我们选中option中的一个时，会将它对应的value赋值到mySelect中
* 多选：可以选中多个值。
  * v-model绑定的是一个数组。
  * 当选中多个值时，就会将选中的option对应的value添加到数组mySelects中

<img src="Vuejs/v-model-select-1.png" style="zoom:80%;" />

<img src="Vuejs/v-model-select-2.png" style="zoom:80%;" />

<img src="Vuejs/v-model-select-3.png" style="zoom:80%;" />

####   v-model修饰符

* lazy修饰符：
  * 默认情况下，v-model默认是在input事件中同步输入框的数据的。
  * 也就是说，一旦有数据发生改变对应的data中的数据就会自动发生改变。
  * lazy修饰符可以让数据在失去焦点或者回车时才会更新：
* number修饰符：
  * 默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当做字符串类型进行处理。
  * 但是如果我们希望处理的是数字类型，那么最好直接将内容当做数字处理。
  * number修饰符可以让在输入框中输入的内容自动转成数字类型：
* trim修饰符：
  * 如果输入的内容首尾有很多空格，通常我们希望将其去除
  * trim修饰符可以过滤内容左右两边的空格

```js
v-model.lazy="message"
v-model.number="message"
v-model.trim="message"
```

##  9本地存储

* 存: 任何位置直接使用

```js
localStorage.islogn = 'xxxx'
```

* 取： 任何位置直接使用

```js
const islogn localStorage.islogn
或者
const { isLogin } = localStorage
```



#  三、Vue组件化开发

##  1 组件化开发基础内容

![](Vuejs/image-20210727192610905.png)

###  1.1 注册组件

1. 创建组件构造器

* `Vue.extend()`
  * 传入自定义组件的模板 `template`
  * Vue2.x后这种写法就不再使用

2. 注册组件

* `Vue.component()`
  * 将组件构造器注册为一个组件，并且给它起一个组件的标签名称。
  * 传递两个参数：1、注册组件的标签名 2、组件构造器

3. 使用组件
   * 组件必须挂载在Vue实例下

```js
<div id="app">
    <!-- 3 使用组件 -->
    <m-cpn></m-cpn>
 </div>
 <script src="../js/vue.js"></script>
 <script>
    //  1 创建组件构造器
    const myComponent = Vue.extend({
        template: `
            <div>
                <h2>组件标题</h2>
                <p>我是组件内容</p>
            </div>
        `
    })
    // 2 注册组件  并定义组件标签的名称
    Vue.component('m-cpn', myComponent)
    
     const app = new Vue({
         el:'#app'
     })
 </script>
```



###  1.2 全局组件与局部组件

1. 全局组件

* 调用Vue.component()注册组件时，组件的注册是全局的。通该方法注册的组件可以在任意Vue实例下使用。

```js
// app1、app2两个实例都可以使用这个组件
<div id="app1">
    <!-- 3 使用组件 -->
    <m-cpn></m-cpn>
 </div>
<div id="app2">
    <!-- 3 使用组件 -->
    <m-cpn></m-cpn>
 </div>
 <script src="../js/vue.js"></script>
 <script>
    //  1 创建组件构造器
    const myComponent = Vue.extend({
        template: `
            <div>
                <h2>组件标题</h2>
                <p>我是组件内容</p>
            </div>
        `
    })
    // 2 注册组件  并定义组件标签的名称
    Vue.component('m-cpn', myComponent)
    
     const app1 = new Vue({
         el:'#app1'
     })
     
 	 const app2 = new Vue({
         el:'#app2'
     })
 </script>
```

2. 局部组件

* 若组件是在某个Vue实例中注册的，那么这个组件是局部组件，只能在这个Vue实例中使用。

```js
// app1可以使用这个组件   app2不会渲染出该组件
<div id="app1">
    <!-- 3 使用组件 -->
    <m-cpn></m-cpn>
 </div>
<div id="app2">
    <!-- 3 使用组件 -->
    <m-cpn></m-cpn>
 </div>
 <script src="../js/vue.js"></script>
 <script>
    //  1 创建组件构造器
    const myComponent = Vue.extend({
        template: `
            <div>
                <h2>组件标题</h2>
                <p>我是组件内容</p>
            </div>
        `
    })
    
     const app1 = new Vue({
         el:'#app1',
         components: {
             'my-cpn': myComponent   // 2 注册组件  并定义组件标签的名称
         }
     })
     
 	 const app2 = new Vue({
         el:'#app2'
     })
 </script>
```

###  1.3 父组件与子组件

* 传入组件构造器可以有： 模板`template`,还可以注册子组件`components`

```js
<div id="app">
    <!-- 3 使用组件 -->
    <parent-cpn></parent-cpn>
 </div>
 <script src="../js/vue.js"></script>
 <script>
    //  1 创建子组件构造器
    const myComponent1 = Vue.extend({
        template: `
            <div>
                <h2>我是子组件</h2>
                <p>我是子组件内容</p>
            </div>
        `
    })
    //  2 创建父组件构造器
    const myComponent2 = Vue.extend({
        template: `
            <div>
                <h2>我是父组件</h2>
                <p>我是父组件内容</p>
            </div>
        `,
        components: {
            'child-cpn': myComponent1   //父组件中注册子组件
        }
    })

     const app = new Vue({
         el:'#app',
         components: {
            'parent-cpn': myComponent2   //Vue实例中注册父组件
        }
     })
 </script>
```

###  1.4 注册组件语法糖

1. 注册全局组件语法糖

```js
Vue.component('m-cpn', {
    template: `
        <div>
            <h2>组件标题</h2>
            <p>我是组件内容</p>
        </div>
    `
})
```

2. 注册局部组件语法糖

```js
 const app = new Vue({
     el:'#app',
     components: {
        'parent-cpn': {
            template: `
                <div>
                    <h2>组件标题</h2>
                    <p>我是组件内容</p>
                </div>
            ` 
        }  
    }
 })
```

###  1.5 模板分离写法

1. 使用`<script> `标签实现

```js
<!-- 使用组件 -->
<div id="app">
    <m-cpn></m-cpn>
 </div>

 <!-- 组件模板 -->
 <script type="text/x-template" id="myCpn">
     <div>
        <h2>组件标题</h2>
        <p>我是组件内容</p>
    </div>
 </script>
 <script src="../js/vue.js"></script>

 <!-- Vue实例 -->
 <script>
     const app = new Vue({
         el:'#app',
         components: {
            'm-cpn': {
                template: '#myCpn'
            }
         }
     })
 </script>
```

2. 使用`<template>`标签实现

```js
<!-- 使用组件 -->
<div id="app">
    <m-cpn></m-cpn>
 </div>

 <!-- 组件模板 -->
 <template id="myCpn">
     <div>
        <h2>组件标题</h2>
        <p>我是组件内容</p>
    </div>
 </template>

 <script src="../js/vue.js"></script>
 <!-- Vue实例 -->
 <script>
     const app = new Vue({
         el:'#app',
         components: {
            'm-cpn': {
                template: '#myCpn'
            }
         }
     })
 </script>
```

###  1.6 组件数据存储

* **data在组件中必须是一个函数**
  * 因为组件会在多个位置使用，若data是对象形式，那么多个使用的位置就会指向同一个数据内存地址，一个位置修改数据，其他地方也会发生改变，导致组件相互影响，不能单独使用。若data是函数形式，在多个位置使用该组件时，会data函数会返回一个属于使用时组件自己的数据存储地址，使得组件使用是隔离的，不会互相影响。

```js
// 使用组件 
<div id="app">
    <m-cpn></m-cpn>
 </div>

 // 组件模板 
 <template id="myCpn">
     <div>
        <h2>组件标题</h2>
        <p>我是组件内容</p>
		<p>{{message}}</p>    
    </div>
 </template>
 <script src="../js/vue.js"></script>

 // Vue实例 
 <script>
     const app = new Vue({
         el:'#app',
         components: {
            'm-cpn': {
                template: '#myCpn',
                data(){
                    return {
                        message: '我是组件数据'
                    }
                }
            }
         }
     })
 </script>
```



##  2 父子间通信

###  2.1 父级向子级传递

* 在组件中，使用选项`props`来声明需要从父级接收到的数据。

* `props`的两种写法
  * 1 `props: {数据名： 数据类型声明}`
  * 2 `props: {数据名： {type：数据类型声明, default: 该数据默认值 }}`
  
* 注意：

  * 数据可以有多个类型： `props: {message: [string, Number]}`

  * 对象和数组默认值必须是函数

    ```js
    props: {
      cInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    }
    ```

* 案例：

 ```js
<div id="app">
  <cpn :c-info="info" ></cpn>
</div>

<template id="cpn">
  <div>
    <h2>{{cInfo}}</h2>
    <h2>{{childMyMessage}}</h2>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>
  const cpn = {
    template: '#cpn',
    props: {
      cInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }

  const app = new Vue({
    el: '#app',
    data: {
      info: {
        name: 'why',
        age: 18,
        height: 1.88
      }
    },
    components: {
      cpn
    }
  })
</script>
 ```

###  2.2 子级向父级传递

**通过自定义事件实现子级向父级传递数据和事件**

* 自定义事件流程：
  * 1 在子组件中，通过$emit()来触发事件。（出发事件 同时可将数据传递出去）

    ```js
    this.$emit('自定义事件名'，数据)
    ```

  * 2 在父组件中，通过v-on来监听子组件事件。

    ```js
    //父组件中 
    // <cpn>为子组件
    <cpn @自定义事件名="cpnClick"></cpn>
    ```

* 案例：

```js
<!--父组件模板-->
<div id="app">
  <cpn @item-click="cpnClick"></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories"
            @click="btnClick(item)">
      {{item.name}}
    </button>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>

  // 1.子组件
  const cpn = {
    template: '#cpn',
    data() {
      return {
        categories: [
          {id: 'aaa', name: '热门推荐'},
          {id: 'bbb', name: '手机数码'},
          {id: 'ccc', name: '家用家电'},
          {id: 'ddd', name: '电脑办公'},
        ]
      }
    },
    methods: {
      btnClick(item) {
        // 发射事件: 自定义事件
        this.$emit('item-click', item)
      }
    }
  }

  // 2.父组件
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn
    },
    methods: {
      cpnClick(item) {
        console.log('cpnClick', item);
      }
    }
  })
</script>

```

##  3 父子组件的直接访问

###  3.1 父访问子

####  3.1.1 $children

**语法:**

```js
//返回值为数组  拿到当前组件的所有子组件
this.$children

//以索引值拿到特定的子组件
this.$children[i]

//拿到特定组件后可以：1获取、修改子组件的数据  2调用子组件的方法 等
this.$children[i].属性
this.$children[i].方法
```

**$children的缺陷**

* 但是当子组件过多，我们需要拿到其中一个时，往往不能确定它的索引值，甚至还可能会发生变化。

* 需要快速且明确获取其中一个特定的组件，这个时候就可以使用$refs

####   3.1.2 $refs

**语法：**

* $refs和ref指令配合使用

* 1 通过ref给某一个子组件绑定一个特定的ID。

  ```html
  <child-cpn ref="child"></child-cpn>
  ```

* 2 通过this.$refs.ID就可以访问到该组件

  ```js
  this.$refs.child.属性/方法
  ```

###  3.2 子访问父

####  3.2.1 $parent

**语法：**

```js
//在子组件中  使用先的语法可以拿到父组件  
this.$parent

//拿到特父组件后可以：1获取、修改父组件的数据  2调用父组件的方法 等
this.$parent.属性
this.$parent.方法
```

* 不建议使用该方式操作父组件数据  因为这样会提高耦合度  容易引起一些问题  也不便于维护

####  3.2.2 $root

* `$root`可以在子组件中直接访问根组件也就是Vue对象，可以访问Vue对象下的数据和方法

* 语法: 

  ```js
  this.$root
  ```

###  3.3 非父子访问

* 非父子组件关系包括多个层级的组件，也包括兄弟组件的关系。

####  3.3.1 中央事件总线

**事件总线类似于Vuex， 只是Vuex用于管理状态， 事件总线用于管理事件**

**应用场景：**

* 相隔一个或多个组件的两个组件之间的通信

**使用：**

* main.js中新建Vue实例作为事件总线: `Vue.prototype.$bus = new Vue()`

```vue
Vue.prototype.$bus = new Vue()

new Vue({
  router,
  render: h => h(App),
}).$mount('#app')
```

* 组件发出全局事件：  

```
this.$bus.$emit('发出事件名'，参数)
```

* 组件监听全局事件：（一般在生命周期函数总监听）

```
this.$bus.$on('发出事件名', (参数) => {
 //处理事件       
})
```

* 组件取消全局事件的监听
  * 注意在**组件销毁的生命周期函数(beforeDestory)**中执行取消该组件自定义的全局事件

```
this.$bus.$off('发出事件名', 函数名)   //函数名：需要取消事件监听的函数
```

##  4 插槽

> [插槽官网教程]( https://cn.vuejs.org/v2/guide/components-slots.html )

* 基本使用

```vue
//父组件
<template>
	<div>
        <Son >//子组件
            <div>{{值}}</div>//放入插槽的内容
        </Son>
    </div>
</template>
```

```vue
//子组件
<template>
    <a>
        <slot>
            默认值显示当前这行话 ，即父组件不传内容时显示
        </slot>
    </a>
</template>
```



* 作用域插槽
  * 父组件可以拿到子组件data中的值并将该值作为插入的一部分插入

```vue
//父组件
<template>
	<div>
        <Son >//子组件
             <template v-slot="slotProps">//自定义名
                 //此时的slotProps.slotData及指向了子组件data的website对象并可访问
                {{slotProps.slotData.title}}//slotData子组件中定义，title子组件data中属性
            </template>
        </Son>
    </div>
</template>

```

```vue
//子组件
<template>
    <a>
        <slot :slotData="website">//父组件可以通过slotData拿到data中的website对象
            {{website.subTitle}} <!-- 默认值显示 subTitle ，即父组件不传内容时 -->
        </slot>
    </a>
</template>

<script>
export default {
    data() {
        return {
            website: {
                url: 'http://wangEditor.com/',
                title: 'wangEditor',
                subTitle: '轻量级富文本编辑器'
            }
        }
    }
}
</script>
```



* 具名插槽
  * 当有多个插槽时便于区分

```vue
//父组件
<template>
	<div>
        <Son >//子组件
            <!--缩写<template #header> -->
            <template v-slot:header>
                <h1>将插入header slot中</h1>
            </template>

            <p>将插入到main slot 中，即未命名的slot</p>

            <template v-slot:footer>
                <p>将插入到footer slot中</p>
            </template>
        </Son>
    </div>
</template>
```

```vue
//子组件
<template>
    <div>
        <header>
            <slot name="header"></slot>
        </ header>
        <main>
            <slot></slot>//默认插入位置
        </ main>
        <footer>
            <slot name="footer"></slot>
        </footer>
    </div>
</template>
```











###   脚手架3中 配置文件别名

1. 在根目录下新建vue.config.js

* '@'表示： 'src'
* alias下配置别名

```
//vue.config.js
module.exports = {
    configureWebpack:{
        resolve: {
            alias: {
                'assets': '@/assets',
                'common': '@/common',
                'components': '@/components',
                'network': '@/network',
                'store': '@/store',
                'views': '@/views',
            }
        }
    }
}
```





###  混入

**混入提供了一种将组件中共有的部分抽离实现可复用的功能。组件中的任何部分都可抽离（data,methods,生命周期函数等）**

**定义：**

```
const mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  },
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
```

**使用：**

```
//app对象中就包含了 mixin对象中的data、methods、生命周期函数
const app = new Vue({
  mixins: [mixin],
})
```

**混入规则：**

* 1 data(数据)合并冲突  以组件数据优先
* 2  同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。
* 3  值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。  



###  getters方法->组件计算属性

将vuex中的getter中的方法直接转化成组件的计算属性

**store定义：**

```js
//state
const state = {
  cartList: []
}

//getters.js
export default {
  cartLength(state){
    return state.cartList.length
  }
}
```

**使用：**

```js
//导入
import { mapGetters } from 'vuex'

//使用
computed: {
  //1 不改别名用法
  ...mapGetters(['cartLength']),
  //2 改别名用法
  ...mapGetters({
    length: 'cartLength'
  })
}

//html
<p>{{cartLength}}</p>  //1 不改别名用法
<p>{{length}}</p>      //2 改别名用法
```



###  actions方法->组件方法

将vuex中的actions中的方法直接转化成组件的方法中的方法

* **注意：actions可以返回promise**

**store定义：**

```js
//actions.js
export default {
  addCart(context, payload){
    return new Promise((resolve, reject) => {
      if(...){
        resolve('成功')
      }else{
        resolve('成功')
      }  
    })
  }
}
```

**使用：**

```js
//导入
import { mapActions }  from 'vuex'

//使用
methods: {
  //1 不改别名用法
  ...mapActions(['addCart'])	
  //2 改别名用法
  ...mapActions({
    add: 'addCart'
  })
}

//调用
//原始调用
this.$store.dispatch('addCart', cart).then((res) => {
  console.log(res)
})
//引入后调用
this.addCart(cart).then((res) => {
  console.log(res)
})
```





##  零碎知识点

* 拿到组件的元素`$el`

```
//例：获取scroll组件到顶部的距离
this.$refs.scroll.$el.offsetTop
```



#  四、Vue3

> 源码：L264-code

##  不熟悉的特性

###  Non-Props

> [非 Prop 的 Attribute](https://v3.cn.vuejs.org/guide/component-attrs.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lesson 17</title>
  <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  // Non-prop 属性
  const app = Vue.createApp({
    template: `
      <div>
        <counter msg="hello" msg1="hello1" @update="change"/>
        <counter1 msg="hello" msg1="hello1" />
      </div>
    `,
    methods:{
      change(){
        console.log('update')
      }
    }
  });

  app.component('counter', {
    // inheritAttrs: false,
    mounted() {
      console.log(this.$attrs.msg);
      console.log(this.$attrs)
      this.$attrs.onUpdate()  //事件也可通过Non-Pro的方式传入子组件并调用,注意要加onXxxx()调用
    },
    // 子组件的根元素是多元素时，父组件传入的Non-Prop属性不会自动添加到根元素上
    // 需要指定Non-Prop属性绑定到哪个元素上
    template: `
      <div :msg="$attrs.msg">Counter</div>
      <div v-bind="$attrs">Counter</div> //所有属性都绑定到该元素上
      <div :msg1="$attrs.msg1">Counter</div>
    `
  });

  app.component('counter1', {
    // inheritAttrs: false, //不接受父元素传入的Non-Prop属性
    // 子组件的根元素是单元素时，父组件传入的Non-Prop属性会自动添加到根元素上
    template: `
      <div>Counter</div>
    `
  });

  const vm = app.mount('#root');
</script>
</html>

```

###  v-model

> [v-model作用于子组件](https://v3.cn.vuejs.org/guide/component-custom-events.html#%E5%A4%84%E7%90%86-v-model-%E4%BF%AE%E9%A5%B0%E7%AC%A6)
>
> * 默认写法，单个v-model
> * 多个绑定，多个v-model
> * 处理v-model修饰符

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lesson 19</title>
  <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>
  const app = Vue.createApp({
    data() {
      return {
        count: 'a',
        name: 'chuckie'
      }
    },
    template: `
      <counter v-model:count.uppercase="count"  />
    `
  });

  app.component('counter', {
    props: {
      count: String,
      countModifiers: { //属性名+Modifiers  处理修饰符
        default: ()=> ({}) //默认为空字符串
      }
    },
    methods: {
      handleClick() {
        let newValue = this.count + 'b';
        console.log(this.countModifiers.uppercase)
        //uppercase是父组件传入的修饰符，判断是否传入，若传入则做对应的操作
        //若没传入则是undefined
        if(this.countModifiers.uppercase) {
          newValue = newValue.toUpperCase();
        }
        this.$emit('update:count', newValue);
      },
    },
    template: `
      <div @click="handleClick">{{count}}</div>
    `
  });

  const vm = app.mount('#root');
</script>
</html>

```

###   Provide / Inject

> [ Provide / Inject](https://v3.cn.vuejs.org/guide/component-provide-inject.html)
>
> 将父组件的参数直接传递到孙子组件
>
> * 传递data中的属性时，需要以返回对象的方式传递
> * 响应式的处理方式

###  directive

> [自定义指令](https://v3.cn.vuejs.org/guide/custom-directive.html)

###  plugin

> [插件](https://v3.cn.vuejs.org/guide/plugins.html)
>
> 源码：plugin  (数据校验案例)



##  Vue3中的动画

> [Animate.css](https://animate.style/)



##  新脚手架

* 卸载老版本的脚手架

```js
npm uninstall vue-cli -g
```

* 安装新的脚手架

1. (仅第一次执行)∶全局安装@vue/cli
   `npm install -g @vue/cli`

   `npm install -g @vue/cli@版本号`

2. *切换到要创建项目的目录*，然后使用命令创建项目, 执行后会弹出选择安装vue2.x或vue3，自行选择；创建好的项目就已经接受git托管了

   `vue create 项目名称`

3. 启动项目
   `npm run serve`

# 五、vue-router

> [Vue-Router官方文档](https://next.router.vuejs.org/zh/)

##  1 安装

**脚手架创建项目时就安装vue-router**

1. `vue create 项目名称`
2. 选择`Manually select features`自定义安装的插件
3. **空格选择`Router`**

<img src="Vuejs/image-20210815162248328.png" alt="image-20210815162248328" style="zoom:80%;" />

4. 选择vue3:`3.x`
5. 是否选择history模式路由，不选择，则默认选择为hash模式路由

![image-20210815162457644](Vuejs/image-20210815162457644.png)

6. 选择仅错误时提醒ESlint

![image-20210815162645470](Vuejs/image-20210815162645470.png)

**使用过程中安装**

```
npm install vue-router@版本号
```

##  2 使用路由

###  2.1 配置控制跳转

* main.js中引入router

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(store).use(router).mount('#app')
```

* `/router/index.js`设置路由

```js
import Home from '../views/home/Home'
import Login from '../views/login/Login'
const routes = [
  {  
    path: '/',      //路由地址
    name: 'Home',   //组件名
    component: Home //组件名
  },
  {
    path: '/login',
    name: 'Login',
    component: Login
  }
]
```

* app.js中使用路由

```vue
<template>
  <router-view/>    //自动根据路由地址显示相应的组件
</template>

<script>
export default {
  name: 'App'
}
</script>
```

###  2.2 js控制跳转

**跳转到指定页面**

```vue
<template>
    <div class="wrapper__login-button"  @click="handleLogin">登录</div>
</template>

<script>
import { useRouter } from 'vue-router'   //引入
export default {
  name: 'Login',
  setup () {
    const router = useRouter()           //实例化
    const handleLogin = () => {
      router.push({ name: 'Home' })      //设置要跳转的页面， Home是路由名
    }
    return { handleLogin }
  }
}
</script>
```

**返回上一个页面**

* `router.back()`

```js
import { useRouter } from 'vue-router'
const useBackEffect = () => {
  const router = useRouter()
  const handleBack = () => {
    router.back()
  }
  return { handleBack }
}
```

###  2.3 标签控制跳转

* ` <to="/shop"> </router-link>`
* 去掉a标签的下划线

```vue
// 路由跳转不带参数的情况
<template>
  <div class="nearby">
    <router-link to="/shop">
      <ShopInfo />
    </router-link>
  </div>
</template>

<style lang="scss" scoped>
.nearby{
  a{
    text-decoration: none;
  }
}
</style>
```

###  2.4 路由参数

####  2.4.1 传递参数

**配置**

```js
const routes = [
  {
    path: '/shop/:id',  //参数名为：id
    name: 'Shop',
    component: () => import(/* webpackChunkName: "shop" */ '../views/shop/Shop')
  }
]
```

**js跳转带参**

* 使用`router.push`方法，并用`${}`添加参数

```vue
<template>
  <div class="nearby">
    <ShopInfo v-for="item in nearbyList" :key="item._id" :item="item" @click="handleToShop(item._id)"/>
  </div>
</template>

<script>
import { useRouter } from 'vue-router'

// 跳转到shop页面
const useToShopEffect = () => {
  const router = useRouter()
  const handleToShop = (id) => {
    router.push(`/shop/${id}`)
  }
  return { handleToShop }
}

export default {
  name: 'Nearby',
  components: { ShopInfo },
  setup () {
    return { nearbyList, handleToShop }
  }
}
</script>
```

**标签跳转带参**

* 通过`:to=" "`指定路径，并用`${}`添加参数

```vue
// 路由跳转不带参数的情况
<template>
  <div class="nearby">
    <router-link v-for="item in nearbyList" :key="item._id" :to="`/shop/${item._id}`">
      <ShopInfo :item="item" />
    </router-link>
  </div>
</template>

<style lang="scss" scoped>
.nearby{
  a{
    text-decoration: none;
  }
}
</style>
```

####  2.4.2 获取参数

* 通过useRoute获取**当前路由**相关的信息，例如：**params**、**path**、**query**等
  * 通过`route.params`获取路由带的参数

```js
import { useRoute } from 'vue-router'

// 获取当前商铺数据
const useShopInfoEffect = () => {
  const route = useRoute()
  console.log(route.params.id)
}
```



##  3 路由守卫

####  全局守卫

> * 任何路由每次跳转之前调用
> * 使用场景：只有登录后才能正常跳转，如果没有登录，则会默认跳转到登录页面

* `/router/index.js`中使用`router.beforeEach`设置全局路由守卫
  * to : 路由跳转的目标路由
  * from ： 路由跳转的原始路由
  * next :  调用则表示允许跳转
    * 接收跳转到的路由的组件名，则干预跳转的页面
    * next(), 不传参数则表示不做干预，保持原来的跳转

```js
router.beforeEach((to, from ,next) => {
  const { isLogin } = localStorage;  //用本地存储记录是否登录了
  const { name } = to;
  const isLoginOrRegister = (name === "Login" || name === "Register");
  //如果登录了，或者路由是Login或者Register则跳转到相应页面不做干预，否则跳转到Login页面
  (isLogin || isLoginOrRegister) ? next() : next({ name: 'Login'});  //接受将要跳转到的路由页面的组件名：Login
})
```

####  单独守卫

> * 跳转到当前路由之前调用
> * 使用场景： 已经登录了，地址为登录页时，不应该显示登录页，而是自动跳转到首页

* `/router/index.js`中的`routes`模块中使用`router.beforeEach`设置单独路由守卫

```js
const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/login',
    name: 'Login',
    component: Login,
    beforeEnter (to, from, next) {  //当路由跳转到login页面时调用
      const isLogin = localStorage.isLogin  //是否登录
      if (isLogin) {
        next({ name: 'Home' })  //如果登录了则跳转到Home界面
      } else {
        next()  //如果没有登录则正常跳转，即跳转到Login页面
      }
      
      //上面的部分可以简化为下面的两行代码
      //const { isLogin } = localStorage
      //isLogin ? next({ name: 'Home' }) : next()
    }
  }
]
```

##  4 动态路由

> 又叫异步路由
>
> 在需要展示页面的时候才加载相关资源

* `/* webpackChunkName: "about" */ `: about  用于调试显示

```js
import Home from '../views/home/Home'
const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home    //普通路由
  },
  {
    path: '/shop',
    name: 'Shop',
    component: () => import(/* webpackChunkName: "shop" */ '../views/shop/Shop')   //动态路由
  }
]
```



# 六、VueX

> [Vuex官方文档](https://next.vuex.vuejs.org/zh/index.html)

**脚手架创建项目时就安装VueX**

1. `vue create 项目名称`
2. 选择`Manually select features`自定义安装的插件
3. **空格选择`Vuex`**

<img src="Vuejs/image-20210815163117742.png" alt="image-20210815163117742" style="zoom:50%;" />

#  七、插件

###  normalize.css

> 移动端不同浏览器的标签样式不一样
>
> normalize.css用于使得不同浏览器上标签样式一直  
>
> 类似css适配中用到的reset css

* 安装

```js
npm install normalize.css --save
```

* 使用:  在main.js中引入即可

```js
import 'normalize.css'
```



# 八、开发常用操作

###  `*`的方式写多个一样的标签

```vue
span.item*4


```

###  vue中全局引入样式

* 在根目录下新建`style`文件，文件夹下可以新建`css、lcss、scss`的文件
* 再在`style`下新建`index.css`将其它样式文件都引入到这个文件中，
* 再在`main.js`中引入`index.css`即可,  使得在`main.js`中只会引入一次，不用每个都引入，因为所有要引入的样式文件都在`index.css`中引入了

```js
import './style/index.css'
```

###  vue中使用scss

> [scss官网](https://www.sass.hk/)

* 在创建项目时，选择安装css预处理： `CSS Pre-processors`
* 在引入文件时同css只是文件名后缀为`.scss`
* style部分添加`lang="scss"`

```vue
<style lang="scss">
</style>
```

###  vue中scoped

* scoped 表示当前样式只对当前组件有效

```vue
<style scoped>
</style>
```

