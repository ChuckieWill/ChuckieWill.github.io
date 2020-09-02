---
title: react-router-dom
date: 2020-05-09 12:07:53
tags:
- react
- react-router-dom
categories:
- [react, react-router-dom]
---

#  react-router-dom

**`react-router-dom`是`React`提供给web端的路由插件，针对`React Native`有其它的插件**

**安装：**

```js
npm install react-router-dom --save
```



##  1 路由定义

**在`App.js`中配置路由**

* `<BrowserRouter>`: 包裹所有的路由
* `<Route>`: 设置具体的路由
  * `path`:  设置路由的路径
  * `exact`： 只有路径完全匹配时才会显示
  * `component`： 配置路由对应的组件

```js
import React, { Component } from 'react';
import Header from './common/header';
import Home from './pages/home';
import Detail from './pages/detail'
import {Provider} from 'react-redux'; //导入react-redux
import store from './store';   //导入store
import { BrowserRouter, Route } from 'react-router-dom';


class  App  extends Component {
  render() {
    return (
      <div >
        <Provider store = {store}>  
          <BrowserRouter>
            <Header/> 
            <Route path='/' exact component={Home}></Route>
            <Route path='/detail' exact component={Detail}></Route>
          </BrowserRouter>          
        </Provider>   
      </div>
    )
  }
  
}

export default App;

```

##  2 单页面路由跳转

###  2.1 `Link`方式

**单页面跳转使用`Link`而不用`a`标签的原因：**

1. `a`标签跳转会再次刷新页面，再次请求`html`文件，这样会造成性能的损耗
2. `Link`实现跳转时，不会再次请求`html`文件，这样既节省了性能也满足单页面应用的规则

**`Link`的使用：**

* 路由路径的定义：
  * `path`: '路由路径'

```js
<Route path='/detail' exact component={Detail}></Route>
```

* 使用`Link`标签的组件必须被`<BrowserRouter>`包裹

```js
//导入
import { Link } from 'react-router-dom';

//使用
 <Link to='/跳转路径'>内容标签</Link>
```

###  2.2 `js`方式

* `/admin`: 目标路由地址

```js
this.props.history.push('/admin')   
```

* 跳转带参数的形式
  * 参数只能是字符串形式，对象、数组传递不了，简单数组（元素为数字或字符串）会解析成字符串后传递

```js
this.props.history.push('/admin/' + params)    //params:参数内容
```

**注意：**

1. 只有在路由管理的直接组件中可以调用上面的接口



##  3 路由参数的传递

###  3.1 动态路由方式

**动态路由方式传递参数就是在使用`Link`标签跳转时携带参数**

* 定义路由既传参名称

  ```js
  <Route path='/路径/:参数名' exact component={组件名}></Route>
   
  //案例
  <Route path='/detail/:id' exact component={Detail}></Route>
  ```

* 传递参数

  ```js
  <Link to={'/路径/' + 参数值}>
      
  //案列
  <Link to={'/detail/' + item.id}>   //item.id = 1
  ```

* 组件中接受参数

  * 可以通过取到的参数在`componentDidMount`生命周期函数中发送网络请求

  ```js
  this.props.match.params.参数名
  
  //案例
  this.props.match.params.id
  ```




###  3.2 `?id=`的形式

* 定义路由既传参名称

  ```js
  <Route path='/路径' exact component={组件名}></Route>
   
  //案例
  <Route path='/detail' exact component={Detail}></Route>
  ```

* 传递参数

  ```js
  <Link to={'/路径?id=' + 参数值}>
      
  //案列
  <Link to={'/detail?id=' + item.id}>   //item.id = 1
  ```

* 组件中接受参数

  * 可以通过取到的参数在`componentDidMount`生命周期函数中发送网络请求
  * *此处注意：返回值需要解析之后才拿得到参数*

  ```js
  this.props.location.search
  
  //返回值：?id=1
  //需要解析`?id=1`拿到1
  
  ```

