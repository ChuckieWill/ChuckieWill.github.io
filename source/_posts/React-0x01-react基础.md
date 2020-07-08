---
title: React(0x01)-react基础
date: 2020-07-08 18:09:00
tags:
- react
categories:
- [react, react]
---

#  React基础

## React 简介

* 2013年由Facebook推出且开源

* 函数式编程

  * 代码主要是函数构成，有利于前端自动化测试

* 声明式开发（React、Vue）

  * 命令式编程 ---- jQuery

* 单向数据流

  * 父组件只给子组件传递数据 ，子组件不能直接修改父组件的数据，如果子组件要修改父组件的数据，需要通过调用父组件的方法来实现

* 视图层框架

* [React官网]( https://zh-hans.reactjs.org/ )

  v16.3.2

## React开发环境的搭建

* 使用React脚手架（Create React App）**创建[单页](https://zh-hans.reactjs.org/docs/glossary.html#single-page-application)应用** 

  * 1 安装Create React App脚手架并用该脚手架创建新项目
    * npx:   [npm 5.2+ 附带的 package 运行工具](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) 

  ```js
  npx create-react-app <新建项目名称>  
      
  以上命令等价于
  npm install -g create-react-app
  create-react-app <新建项目名称>  
  ```

  * 2 终端切换到项目目录下

  ```
  cd <项目名称>
  ```

  * 3 开启项目

  ```
  npm start
  ```

##  React单页应用初始目录详解

**初始时主要文件目录**

* public(发布文件)
  * index.html
  * manifest.json(与pwa对应的文件)
* src(源文件)
  * App.css
  * App.js
  * App.test.js
  * index.css
  * index.js(入口文件)
  * logo.svg(首页使用的图片)
  * serviceWorker.js(与pwa对应的文件)
  * setupTest.js

**修改初始目录用于初始开发**

* public(发布文件)

  * index.html

    * 删除对`manifest.json`文件的引用

      ```
        <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />    --- 删除
      ```

  * manifest.json(与pwa对应的文件)  --- 删除

* src(源文件)

  * App.css  --- 删除

  * App.js

    ```
    //修改删除已删除的引用
    
    import React from 'react';
    
    function App() {
      return (
        <div >
          hello word
        </div>
      );
    }
    
    export default App;
    ```

  * App.test.js   --- 删除

  * index.css  --- 删除

  * index.js(入口文件)

    ```
    //修改删除已删除的引用
    
    import React from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';
    
    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );
    ```

  * logo.svg(首页使用的图片) --- 删除

  * serviceWorker.js(与pwa对应的文件)  --- 删除

  * setupTest.js   --- 删除

**PWA**

pwa（ Progressive Web App ）主要作用是软件会缓存已经加载的页面，在断网后可以正常显示已经加载过的页面，提高用户体验。



##  React编码规范

####  1 组件首字母

* 自定义组件标签首字母大写，`react`原生组件标签首字母小写

####  2 函数`this`重定向

*  写在构造函数中

```js
constructor(props) {
    super(props)
    this.itemDel = this.itemDel.bind(this)
  }
```

####  3 `this.props`解构

* 子组件使用父组件传入的数据 ---  解构后使用

  ```js
   //原始写法
   this.props.itemDel(this.props.index)
   
   //解构后的写法
   const {itemDel, index} = this.props
   itemDel(index)
  
  
  //案例：
   render() {
      const { content, text } = this.props 
      return (
        <div>
          {text}-{content}
          <button onClick = {this.itemDel} >删除</button>
        </div>
      )
    }
  ```

####  4 `setState()`传入函数

* `setState()`传入函数不再是传入对象

  ```js
  //传入对象形式
  this.setState({
      属性： 值
  })
  
  //传入函数  ----  可以实现异步
  this.setState(() => {
      return {
          属性： 值
      }
  })
  this.setState((prevState) => {    //prevState相当于this.state  
      return {
          属性： 值
      }
  })
  ```

  * 案例1

    ```js
    const value = e.target.value   // 注意现在外面定义 再用 不要直接在setState中用
    this.setState(() => {
      return {
        inputValue: value
      }
    })
    
    // ES6 箭头函数简写
    const value = e.target.value  
    this.setState(() => ({
        inputValue: value
      }))
    ```

  * 案例2 

    ```js
    //传入对象写法
    this.setState({
      list: [...this.state.list, this.state.inputValue],
      inputValue: ''
    })
    
    //传入函数写法
    this.setState((prevState) => ({
      list: [...prevState.list, prevState.inputValue],
      inputValue: ''
    }))
    ```

  * 案例3

    ```js
    //传入对象写法
    const list = [...this.state.list]
    list.splice(index, 1)
    this.setState({
      list
    })
    
    //传入函数写法
    this.setState((prevState) => {
      const list = [...prevState.list]
      list.splice(index, 1)
      return {list}
    })
    ```

    

## React组件的使用基础

*  --1-- : 组件构造函数
*  --2--: 父组件(Component) 的构造函数
*  --3--: 存储组件的数据
*  --4--: 组件的html --- JSX
*  --5--: React组件的标签必须有一个根标签，可以是`<div>`,但是`<div>`会在DOM中渲染出来，为了不渲染，用`<Fragment>`来占位，起到跟标签的作用
*  --6--: jsx中用‘{}’包裹js代码
*  --7--： React中绑定事件： 将原生事件写成驼峰式即可  在`.bind`中传递参数
*  --8--： 将事件函数的this绑定为组件的this，以便在事件函数中通过this访问组件
*  --9--: 循环数组 注意绑定`key`值
*  --10--: 修改Reat组件中的数据 需要通过this.setState({})函数来修改

```js
import React, {Component, Fragment} from 'react'

class TodoList extends Component {
  constructor(props) {  //--1-- 
    super(props)        //--2--
    this.state = {      //--3--
      inputValue: 'hello',
      list: []
    }
  }

  render() {           // --4--
    return (           // --5--
      <Fragment>      
        <div>
          <input value = {this.state.inputValue}      // --6--
                 onChange = {this.changeInputValue.bind(this)}   //--7-- --8--
          />
          <button onClick = {this.submitInput.bing(this)}>提交</button>
        </div>
        <ul>
            {
              this.state.list.map(item => {   // --9--
                return (
                <Fragment  key={index}>   
                <li key={index}>{item}</li>
                <button onClick = {this.itemDel.bind(this, index)} key={index+1}>删除</button>                                              // --8--
                </Fragment>
                )
              })
            }
        </ul>
      </Fragment>
    )
  }

  changeInputValue(e){
    this.setState({    //--8--    --10--
      inputValue: e.target.value
    })
  }

  submitInput(){
    this.setState({
      list: [...this.state.list, this.inputValue]  
    })
  }
}

export default TodoList;
```

## JSX语法

**`JSX`就是在`js`代码中写的`html`标签的语法**

* JSX中写注释

  ```
  {/*注释内容*/}
  
  {
  	//注释内容
  }
  ```

* 绑定样式: `className`   *注意不是`class`*

  ```
  <div className='calss1'></div>
  ```

* 不转义显示带有html标签的字符串

  ```
  //item:‘<h1>hello word</h1>’   -----  react组件中的数据
  
  <div dangerouslySetInnerHTML={{__html: item}}></div>
  
  
  这种方式在界面不会显示
  <h1>hello word</h1>  
  而是
  hello word  且是一号标题的字体大小
  ```

* `<label>`标签的`for`改为`htmlFor`

  ```
  <label htmlFor="insert">输入内容</label> 
  <input id="insert" />
  ```

  

##  组件间通信

###  1 父级向子级传递数据

* 实现步骤

  1. 父组件中通过属性（自定义）绑定要传递的数据

  ```js
  //子组件：<TodoItem/>
  //自定义属性：content
  //父组件中的数据：item   注意：{} 包裹
  <TodoItem content = {item}/>
  ```

  2. 子组件中通过`this.props.属性`取到父组件传入的数据

  ```js
  //自定义属性：content
  this.props.content
  ```

###  2 子级向父级传递数据

* 子级向父级传递数据是通过**子组件调用父组件的方法，给方法传值实现的。**这个操作包括两个功能：

  * 1 子组件调用父组件的方法；
  * 2 子组件向父组件传递数据

* 实现步骤：

  1. 父组件中通过属性（自定义）绑定子级可调用的方法

  ```js
  //子组件：<TodoItem/>
  //自定义属性(方法名)：itemDel
  //父组件中的方法：itemDel   注意：{} 包裹  this重定向
  <TodoItem itemDel = {this.itemDel.bind(this)}/>
  ```

  2. 子组件中通过`this.props.属性（自定义方法名）`调用父组件的方法

  ```js
  //自定义属性(方法名)：itemDel
  //父组件中该方法需要的参数： this.props.index
  this.props.itemDel(this.props.index)
  ```

  3. 父组件中接收子组件传来的数据

  ```js
  itemDel(index){
  	console.log(index)   //若该函数调用方是组件，则这个index就是子组件传来的
  }
  ```

