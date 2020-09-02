---
title: React Transition Group
date: 2020-05-08 20:45:50
tags:
- react
- React Transition Group
categories:
- [react, React Transition Group]
---

# React Transition Group

**`React Transition Group`是用于帮助`React`实现动画效果**

* 安装

  ```js
  npm install react-transition-group --save
  ```

  

##  1 CSSTransition

* [`CSSTransition`官方使用教程]( http://reactcommunity.org/react-transition-group/css-transition )
* `CSSTransition`可以实现单个标签的动画效果
* 使用流程

1. 导入`CSSTrasition`

   ```js
   import {CSSTransition} from 'react-transition-group'
   ```

2. 设置属性

   * 用`<CSSTransition>`标签包裹需要设置动画的标签
   * `in` : 绑定控制动画效果切换的属性   this.state.show的值在true和false之间切换来控制动画效果
   * `timeout`: 动画持续的时间
   * `classNames` : 动画的class名  与`.css`中样式类名一致
   * `unmountOnExit` : 动画出场即`fade-exit-done`样式效果结束后  直接将标签移除掉（若没有该属性 只是不显示标签  标签的位置任然占有）
   * `onEntered` : 钩子函数  还有很多不同时刻的钩子函数  具体查看[官网文档](http://reactcommunity.org/react-transition-group/css-transition)
   * `appear` ： 值为`true`时  第一次显示标签是也有动画效果  需要在`.css`文件中设置相关样式

   ```html
   <CSSTransition
       in = {this.state.show}
       timeout  = {1000}
       classNames = 'fade'
       unmountOnExit
       onEntered = {(el) => {el.style.color = 'blue'}}
       appear = {true}
       >
       <h1>hello world</h1>
   </CSSTransition>
   ```

3. 设置样式

   * `fade` : 对应属性`classNames = 'fade'`
   * `appear`的三个样式类：对应属性`appear = {true}`
   * `enter`的三个样式类：入场样式动画
   * `exit`的三个样式类：出场样式动画

   ```css
   .fade-appear, .fade-enter{
     opacity: 0 ;
   }
   
   .fade-appear-active, .fade-enter-active{
     opacity: 1;
     transition: opacity 1s ease-in;
   } 
   
   .fade-appear-done , .fade-enter-done{
     opacity: 1 ;
   }
   
   .fade-exit{
     opacity: 1 ;
   }
   
   .fade-exit-active{
     opacity: 0;
     transition: opacity 1s ease-in;
   }
   
   .fade-exit-done{
     opacity: 0;
   }
   ```

##  2 TransitionGroup

* [TransitionGroup官方使用教程]( http://reactcommunity.org/react-transition-group/transition-group/ )
* `CSSTransition`只能实现单个标签的动画效果，如果要实现一组标签都有动画效果（遍历的标签）则要用`<TransitionGroup>`标签将整个遍历的内容包裹

* 使用流程：

1. 导入`CSSTransition, TransitionGroup`

   ```js
   import {CSSTransition, TransitionGroup} from 'react-transition-group'
   ```

2. 对遍历的单个标签执行上面关于`CSSTransition`的属性设置和样式设置

3. 用`TransitionGroup`包裹遍历的所有内容

* 案例：

  ```js
  <TransitionGroup>
  	{ 
      	this.state.list.map((item, index) => {  
            return ( 
              <CSSTransition
                  in = {this.state.show}
                  timeout  = {1000}
                  classNames = 'fade'
                  unmountOnExit
                  onEntered = {(el) => {el.style.color = 'blue'}}
                  appear = {true}
  				key = {index}
                >
                  <TodoItem />    //遍历的单标签
                </CSSTransition>
            )
          }
  	}
  </TransitionGroup>
  ```

  

