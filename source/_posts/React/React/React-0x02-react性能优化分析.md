---
title: React(0x02)-react性能优化分析
date: 2020-05-10 20:37:11
tags:
- react
categories:
- [react, react]
---

#  React性能优化分析

##  1 `this`作用域修改放到构造函数中

* 整个组件同一个函数`this`作用的修改只在构造函数中执行一次，提升了性能

```js
constructor(props) {
    super(props)
    this.itemDel = this.itemDel.bind(this)
  }
```

##  2 `setSate()`修改合并

```js
setState(() => {
    return {
        
    }
})
```



* `setState()`是异步函数，在短时间内进行的多次修改，`setState`会将多次修改合并，只执行一次更新，以提升性能

##  3 虚拟DOM

* 虚拟DOM的比对比真实DOM的比对性能消耗小得多
* 通过对比，只修改DOM中有变化的部分
* `Diff`算法，逐层对比，有不同则后续不再对比，直接替换，提高了对比效率
* `key`值的使用也提高了比对的效率

##   4 shouldComponentUpdate()的使用

**`shouldComponentUpdate()`避免了子组件无用的刷新**

* `render`函数执行的条件之一是父组件`render`函数执行
* 场景：
  * 父组件有数据A、数据B
  * 子组件接收了父组件的数据A
  * 当父组件数据A变化时，父组件的`render`函数会执行，子组件的`render`函数也会执行并刷新页面，这是正常的情况
  * 当父组件数据B变化时，父组件的`render`函数会执行，子组件的`render`函数也会执行并刷新页面，但此时子组件是不需要刷新的，这就导致了性能的损耗。
* 解决方案：
  * 在子组件`shouldComponentUpdate()`函数中判断数据A是否变化，若没有变化则返回false，后续刷新就不会再执行了。
  * 当当父组件数据B变化时，子组件判断数据A没有变化，则不会执行后续刷新，从而避免了性能损耗。



##  5 PureComponent

**`PureComponent`定义的组件会自动实现`shouldComponentUpdate()`生命周期函数来避免`store`改变引起的不必要的页面刷新**

* 使用好处： 不必要一个个手动的给组件增加` shouldComponentUpdate()`生命周期函数，并判断store数据的改变是否影响本组件。
* 使用前提： `store`必须是用`immutable.js`管理的，否则会有很多报错。