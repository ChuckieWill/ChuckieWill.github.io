---
title: Javascript(0x01)-js基础andES6
date: 2020-04-02 10:33:22
tags:
- Javascript
- ES6
categories:
- [Javascript]
---



#  一、JavaScript基础内容

> [菜鸟教程]( https://www.runoob.com/js/js-tutorial.html )

##  1 JavaScript简介

* JavaScript包含三部分：ECMAscript（该语言的语法和基本对象）、DOM、BOM

* 高级语言分类：

  * 编译性语言（C、C++）：源码一次性全部编译成二进制文件，再执行 。   特点：需要编译    源码一次性编译
  * 解释性语言（Python、JavaScript）：读一行源码  执行一行源码。             特点：不需要编译    源码一行一行的执行
  * java是先编译再解释  比较特殊

* 动/静态类型语言：

  * 动态类型语言（灵活但不安全）：不确定一个变量（标识符）的类型，在代码执行过程中可动态修改

  ```js
  var name = '123'
  name = 123   //可行  不会报错
  ```

  * 静态类型语言：在代码执行之前，可以确定一个变量（标识符）准确的类型，并且之后不允许修改

  ```ts
  let name :string = 'abc';
  name = 123    //报错  因为改变了数据类型
  ```

* JavaScript的应用场景

  * 1 网页的交互
  * 2 服务端开发（Node.js）
  * 3 命令行工具（Node.js）
  * 4 桌面程序  vscode 就是用TypeScript开发的
  * 5 APP（React Native）
  * 6 游戏开发（cocos2d-js）
  * 7 微信小程序开发

* JavaScript文档注释   一般用于给函数作说明

```js
/**
*这个函数是一个工具函数 需要传入index数字
*/
function text(index){
    return index*100
}
```

* 并行、并发、单线程
  * 并行是指同时执行，只有多线程的语言才支持
  * 并发本质是充分利用cpu资源（多个任务交替使用cpu），并没有正真同时执行
  * JavaScript、Python都是单线程语言，只能实现并发，不能实现并行
  * Python的多线性是通过伪线程实现的

  * JavaScript 并发
    * 宏任务，微任务机制实现并发，EventLoop
    * 本质是CPU足够快，当请求资源时（查询数据库、网络请求等）,此时CPU就空出来了，而这时CPU会切换到其他任务，只要切换的足够快，就实现了并发。
  * Node.js 优势是只能异步编程，不支持同步编程，异步编程就大大提升了资源的利用率



##  2 JavaScript编码规范

[airbnb/javascript规范](https://github.com/airbnb/javascript)

* 变量命名规范：驼峰式

* = 赋值 两边空格

* 变量的定义

  ```
  用let、const而不用var
  var没有块级作用域
  ```

* 类的定义

  ```
  const obj = {}//大括号
  ```

* 数组的定义

  ```
  const arr = []
  ```

* 方法的定义

  ```
  fangfa(){
  
  }
  ```

* 简写

  ```
  const name = 'xioaming'
  const person = {
  	name,     //简写、且在未简写属性上面
  	age: 18,  //属性后有','
  }
  ```

* 使用箭头函数

  ```
  (res) => {
  
  }
  ```

* 参数写法

  ```
  1 参数一个个列出来，最好不用params对象（内部参数明确）
  2 用{}包裹参数，以便调用时仍然可以以对象的形式传入
  request({url,data={},method='GET'}){
  
  }
  ```



##  3 基础语法

####  3.1 for循环

> [Javascript(0x05)-异步编程-5for...of  异步循环](https://chuckiewill.github.io/2020/08/05/Javascript/Javascript-0x05-异步编程/)

* forEach((item) => {})    **同步执行**

* for(let i in list)     i取到的是list数组的索引值, i取到的是list对象的键   **同步执行**
* for(let item of list)   item取到的是list数组的元素， item取到的是list对象的值   **异步执行**



####  3.2 Object.keys()

* ` Object.keys()`返回结果为对象直接子节点的键，数据类型为Array

```js
const json = {
  a : { b : { c : 'helo'}},
  d : [110 , 112]
}

const temp = Object.keys(json)

//temp结果
['a', 'd']
```

####  3.3 构建数组

* 构建二维数组

```js
const flow1 = Array.from({length: 数组长度}, () => new Array(数组长度).fill(数组填充内容))
const flow1 = Array.from({length: m}, () => new Array(n).fill(false))

flow1 为一个m*n的二维数组，且全部填充为false
```

* 字典转数组

```js
let list = Array.from(map)

//map:字典
Map(3) { 1 => 3, 2 => 2, 3 => 1 } 
//list：字典转换为二维数组
[ [ 1, 3 ], [ 2, 2 ], [ 3, 1 ] ]
```

####  3.4 原型链上添加属性或方法

> [Javascript(0x07)-JS中的原型链](https://chuckiewill.github.io/2020/10/03/Javascript/Javascript(0x07)-JS%E4%B8%AD%E7%9A%84%E5%8E%9F%E5%9E%8B%E9%93%BE/)

* 将方法或属性挂在数组原型链上
  * 新建任何一个数组，都可以访问这个属性或方法
    * 利用了原型链的特性：如果A对象上没有找到x属性（方法），那么会沿着A对象的原型链找x属性（方法）
  * 这个方法可以通过this拿到实例化的数组

```js
Array.prototype.bubbleSort = function () {
  console.log(this) //this可以拿到实例化的数组
}

const arr = [5, 4, 3, 2, 1]
arr.bubbleSort()
```

####  3.5 forEach（）

```js
const res = [1,2,3,4,5]
res.forEach((val , index) => {
    console.log(val, index)
  })

//输出
值 索引
1 0
2 1
3 2
4 3
5 4
```

####  3.6 获取项目根目录地址

```js
process.cwd()
```

####  3.7 拼接对象

```js
const classicFields = {
  title: Sequelize.STRING,
  type: Sequelize.INTEGER
}
//拼接
Object.assign({url:Sequelize.STRING},classicFields)
//拼接结果
const classicFields = {
  title: Sequelize.STRING,
  type: Sequelize.INTEGER,
  url:Sequelize.STRING
}
```

####  3.8 对象键值对定义

```js
//正常定义
test: {
      a: 400
    }
//键本质都是字符串
test: {
    'a' : 400
}
//[]形式 []可以计算一个值，将计算结果作为键
test: {
    [a]: 400
}
//以下两种方式等价
//方式1
a = 100
test: {
    [a]: 400
}
//方式2
test: {
    '100': 400
}
```

####  3.9 util内置帮助函数

```js
const util = require('util')

//拼接字符串
//先用%s占位需要填充的位置
loginUrl: 'https://api.weixin.qq.com/sns/jscode2session?appid=%s&secret=%s&js_code=%s&grant_type=authorization_code'
//调用util.format()函数填充
const url = util.format(loginUrl,appId,appSecret,code)
```

####  3.10 判断属性不是原型上的属性

```js
for (let key in obj) {
        // 保证 key 不是原型的属性
        if (obj.hasOwnProperty(key)) {
            
        }
    }

obj.hasOwnProperty(key)
hasOwnProperty(key)函数判断属性是否为obj对象原型上的属性
若key属性是原型上的属性则返回false
若key属性不是原型上的属性则返回true
```

####  3.11 obj[键]式获取对象值

```js
let test = {
  a: 100,
  c: 300
}

 test['b'] =200
 console.log(test)
 console.log(test['b'])//注意键是字符串形式
 console.log(test.b)

 for(let i in test){
   console.log(test[i],'hhh')
 }

//打印结果
{ a: 100, c: 300, b: 200 }
200
200
100 hhh
300 hhh
200 hhh
```

####  3.12 对象与类区别

* 直接定义的简单对象中可以定义方法，且可以通过对象直接调用方法
* 定义的类需要实例化后才能调用方法
  * 类的本质是函数
  * 实例化后为对象

```js
const wangwu = {
  name:'王五',
  sayHi( ) {
    // this 即当前对象
    console. log ( this)
  },
  waitAgain() {
    //箭头函数的this取上级作用的this
    setTimeout(() => { 
      //this即当前对象
      console. log( this)
    })
  }
}
wangwu.sayHi()
wangwu.waitAgain()//直接调用方法



class People {
  constructor(name) {
    this.name = name
    this.age = 20
  }
  sayHi() {
    console.log (this)
  }
}
const chuckie = new People('chuckie')
chuckie.sayHi()// chuckie对象
People.sayHi()//报错，因为没有实例化
```

####  3.13 匿名函数

> [js中的匿名函数](https://www.cnblogs.com/ranyonsue/p/10181035.html)

```js
//匿名函数包裹一个括号
//匿名函数在其它应用场景括号可以省略
(function (){
    //由于没有执行该匿名函数，所以不会执行匿名函数体内的语句。
    console.log("张培跃");
})

//如果需要执行匿名函数，在匿名函数后面加上一个括号即可立即执行！
(function (){
    //此时会输出张培跃
    console.log("张培跃");
})()

//倘若需要传值，直接将参数写到括号内即可：
(function (str){
    //此时会输出张培跃好帅！
    console.log("张培跃"+str);
})("好帅！")
```

* 用`!`分隔匿名函数(多个匿名函数时，不加`!`会引发错误)，`!`不会影响代码执行

```js
!(async function () {
  const res = await 100
  console.log(res) // 100
})()
```





##  4 阅读代码提示

> [函数提示解释视频](https://www.bilibili.com/video/BV1nJ411J7a2?p=27 )

##  5 易错点

###  5.1 循环导入

* 循环导入会导致，导入的类为undefined，以下面的情况为例，a.js、b.js互相循环导入对方的模块，则报错，报错提示导入的模块为undefined

  * 目录结构：
    * a.js
    * b.js

  ```js
  //a.js
  const a = require('./a.js')
  
  //b.js
  const b = require('./b.js')
  ```

* 解决方案
  
  * 不在文件头部导入，而是在使用的地方导入





#  二、ES6

##  0 ES6教程地址

> [菜鸟教程](https://www.runoob.com/w3cnote/es6-tutorial.html )
>
> [阮一峰ES6]( https://es6.ruanyifeng.com/ )

##  1 块级作用域  var ---> const let

* ES5之前没有块级作用域的概念（if  for都没有块级作用域   但是function有块级作用域），所以很多时候必须借助function作用域解决引用块级外变量的问题
* ES6加入了let和const 都有了块级作用域  

```
//ES5
var btns = document.getElementsByTagName('button')
//for没有块级作用域  会报错
for(var i= 0;i<btns.length;i++){
    btns[i].addEventListener('click',function(){
        console.log('第'+i+'个按钮被点击')
    })
}
//通过function闭包解决for没有块级作用域的问题
for(var i= 0;i<btns.length;i++){
    (function(i){
        btns[i].addEventListener('click',function(){
        console.log('第'+i+'个按钮被点击')
        })
    })(i) 
}

//ES6
const btns = document.getElementsByTagName('button')
for(let i =0;i<btns.length;i++){
    btns[i].addEventListener('click',function(){
        console.log('第'+i+'个按钮被点击')
    })
}
```

* const使用注意事项

  * 定义之后不能修改

  * 在定义时  必须附初始值

  * 定义的对象不能修改  但是对象内的属性可以修改

    ```
    //修改const 对象中的属性不报错
    
    const config = {
      api_base_url: 'http://bl.7yue.pro/v1',
      appkey:'K57S1kGd4CLBz2dw'
    }
    
    config = 1  //报错
    config.appkey = 2 //不报错
    ```

    



##  2 对象增强写法

1. 属性的增强写法
2. 函数的增强写法

```
//ES5
test : function(){

}
//ES6
test(){

}

```

##  3 字符串 

###  3.1 标签字符串` `

* ES5之前字符串是 ' '  或  " "  这方式字符串不支持换行
* ES6字符串用``  这种方式支持字符串换行

###  3.2 模板字符串

```
用`${}`包裹数据或函数 
```

* 数据替换

```
let a = 123
console.log(`${a}456`)
输出：123456
```

* 函数替换

```
test(){
return 123
}
console.log(`${this.test()}456`)
输出：12356
```

###  3.3 字符串识别函数

* includes()
* startsWith()
* endsWith()

###  3.4 字符串补全函数

* padStart（）
* padEnd()



##  4 模块化 导入导出

###  4.1 ES6 export import

* 使用前提：  script  type类型为module

```
<script src='../text.js' type='module'></script>
```

* 导出：export

  * 逐一导出

  ```js
  export let myName = "Tom";
  export let myAge = 20;
  export let myfn = function(){
              return "My name is" + myName + "! I'm '" + myAge + "years old."
          }
  export let myClass =  class myClass {
              static a = "yeah!";
          }
  ```

  * 统一导出，等同于逐一导出

  ```js
  let myName = "Tom";
  let myAge = 20;
  let myfn = function(){
      return "My name is" + myName + "! I'm '" + myAge + "years old."
  }
  let myClass =  class myClass {
      static a = "yeah!";
  }
  export { myName, myAge, myfn, myClass }
  ```

  * export default 
    * 在一个文件或模块中，export、import 可以有多个，export default 仅有一个。
    * export default 中的 default 是对应的导出接口变量。
    * 通过 export 方式导出，在导入时要加{ }，export default 则不需要。
    * export default 向外暴露的成员，可以使用任意变量来接收。

  ```js
  var a = "My name is Tom!";
  export default a; // 仅有一个
   
  import b from "./xxx.js"; // 不需要加{}， 使用任意变量接收
  ```

  

* 导入：import

  * 导入时  路径一定是相对路径

```
1 逐个导入
import { myName, myAge, myfn, myClass } from "./test.js";
console.log(myfn());// My name is Tom! I'm 20 years old.
console.log(myAge);// 20
console.log(myName);// Tom
console.log(myClass.a );// yeah!

2 统一导入
import * as aaa from "./test.js"
console.log(aaa.myAge);// 20
```

* as用法

```
/*-----export [test.js]-----*/
let myName = "Tom";
export { myName as exportName }
 
/*-----import [xxx.js]-----*/
import { exportName } from "./test.js";
console.log(exportName);// Tom
使用 as 重新定义导出的接口名称，隐藏模块内部的变量
/*-----export [test1.js]-----*/
let myName = "Tom";
export { myName }
/*-----export [test2.js]-----*/
let myName = "Jerry";
export { myName }
/*-----import [xxx.js]-----*/
import { myName as name1 } from "./test1.js";
import { myName as name2 } from "./test2.js";
console.log(name1);// Tom
console.log(name2);// Jerry
```


###  4.2对比commonJS模块

* 使用前提：需要node.js环境解析  所以 使用前要先安装node.js

* 导出

```
module.exports = {变量名，函数名，对象名}
```

* 导入

```
let {变量名，函数名，对象名}  = require('../aaa.js')
```

* 别名

  * 导出

  ```js
  module.exports = {
    别名:原名
  }
  
  module.exports = {
    db:sequelize
  }
  ```

  * 导入

  ```js
  const {原名 : 别名} = require('../../core/db')
  
  const {sequelize : db} = require('../../core/db')
  ```

  

##  5 箭头函数--this

* 箭头函数与其函数写法对比

```
//箭头函数
success: (res) => {    //注意有‘:’
console.log(res)
}

//常规写法1
success:founction (res){
console.log(res)
}

//常规写法2
success (res){        //注意无箭头 无‘:’
console.log(res)
}
```

* 不同情况下的箭头函数写法

```
//1 无参数
const test = () => {
	console.log(res)
}

//2 只有一个参数 ---- 可以省去（）
const test = res => {
	console.log(res)
}

//3 函数内部语句只有一句 ----   可以省去{}  若有return  也可以省去return
const test = res => console.log(res)
```





## 6 扩展运算符...

```
...res  :展开对象，获得对象中的每一个元素
```



##  7 对象、数组解构  

###   7.1 对象解构

```js
const obj = {
	name : 'why',
    age : 18,
    height : 1.88
}

const {neme, age, height} = obj    //----解构

//结果
name = 'why',
age = 18,
height = 1.88
```

###  7.2 数组解构

```js
const array = ['why', 'code', 'maxthon']

const [name1, name2, name3] = array  //---解构

//结果
name1='why', name2='code',  name3='maxthon'
```

##  8 类 class

> [js类中实例方法、静态方法、原型方法的区别](https://blog.csdn.net/qiuqiula_/article/details/100138750?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)