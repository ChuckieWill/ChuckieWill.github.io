---
title: Styled-Components
date: 2020-07-31 12:10:52
tags:
- react
- Styled-Components
categories:
- [react, Styled-Components]
---

#  Styled-Components

##  1 基础

* **`styled-components`用于管理`React`样式**

  * 默认情况下，react中引入的样式会作用于其所在组件及其所有子组件，这样就会相互影响
  * `styled-components`的核心功能就是，使组件的样式只作用于组件本身，不作用于其子组件

* Github地址：[styled-components](  https://github.com/styled-components/styled-components  )

* 安装：

  ```js
  npm install styled-components --save
  ```

##  2 使用

**样式文件后缀都由`.css`改为`.js`**

###  2.1 全局样式

1. 定义

```js
//style.js
import { createGlobalStyle } from 'styled-components'

export const Globalstyle = createGlobalStyle`　
  body{
  　　margin: 0;
  　　padding: 0;
      background-color: green;
  }
`
```

2. 使用
   * 以标签的形式放到`App`组件中即可生效

```js
import React, { Component } from 'react';
import {Globalstyle} from './style';

class  App  extends Component {
  render() {
    return (
      <div >
        <Globalstyle/>
        <h1>hello word</h1>
      </div>
    )
  }
}

export default App;
```

###  2.2 组件样式

**此定义样式只作用于组件，不会影响其它组件**

1. 定义

```js
//style.js
//模板
import styled from 'styled-components'

export const <样式组件名> = styled.<样式标签>`
  //样式内容
`

//案例
import styled from 'styled-components'

export const HeaderWrapper = styled.div`
  height: 58px;
  background-color: green
`
```

2. 使用

```js
import React, { Component } from 'react';
import { HeaderWrapper } from './style'   //导入样式标签

class  Header  extends Component {
  render() {
    return (
      <div >
        <HeaderWrapper>hello word</HeaderWrapper>   //使用样式标签
      </div>
    )
  }  
}
export default Header;
```

##  3 样式类的使用

* 定义

```js
<组件名 className="类名">首页</组件名>

export const 组件名 = styled.标签`
  &.类名 {
    样式
  }
`
```

* 案例

```js
<NavItem className="left active">首页</NavItem>
<NavItem className="left">下载App</NavItem>
<NavItem className="right">Aa</NavItem>
<NavItem className="right">登录</NavItem>

export const NavItem = styled.div`
  line-height: 56px;
  padding: 0 15px;
  font-size: 17px;
  color: #333;
  &.left {
    float: left;
  }
  &.right {
    float: right;
  }
  &.active {
    color: #ea6f5a;
  }
`
```

##  4 标签原生属性的绑定

1. t通过`attrs({})`设置

```js
export const NavSearch = styled.input.attrs({
  placeholder："搜索"
})`
  &::placeholder{
    color: #999;
  }
`
```

2. 直接加在标签上

```js
 <NavSearch placeholder="搜索" ></NavSearch>
```

##  5 标签原生属性样式修改

* 定义

```js
export const 组件名 = styled.标签.attrs({
  标签原生属性="搜索"
})`
  &::标签原生属性{
    color: #999;
  }
`
```

* 案例

```js
export const NavSearch = styled.input.attrs({
  placeholder="搜索"
})`
  &::placeholder{
    color: #999;
  }
`
```

##   6 设置子样式标签的样式

* 定义

```js
<SearchWrapper>
  <i className="样式类名">&#xe614;</i>  //子标签
</SearchWrapper>

export const SearchWrapper = styled.div`
  父标签样式
  .样式类名{
    子标签样式
  }
`
```

* 案例

```js
<SearchWrapper>
  <i className="iconfont">&#xe614;</i>
</SearchWrapper>

export const SearchWrapper = styled.div`
  position: relative;
  float: left;
  .iconfont{
    position: absolute;
    right: 5px;
    bottom: 5px;
    width: 30px;
    line-height: 30px;
    border-radius: 15px;
    text-align: center;
    background: red;
  }
`
```

##  7 给样式传递参数

* 传递

  * 传值属性名自定义： 例`imgUrl`

  ```js
  <RecommenItem  imgUrl={item.imgUrl}></RecommenItem>
  ```

* 接收

  * `${}` ： 包裹js语法
  * 通过`props`取值

  ```js
  export const RecommenItem = styled.div`
    background: url(${(props) => props.imgUrl});
  `
  ```

  



