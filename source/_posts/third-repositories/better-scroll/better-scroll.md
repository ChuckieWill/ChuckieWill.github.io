---
title: better-scroll
date: 2020-02-10 21:45:50
tags:
- better-scroll
categories:
- [third-repositories, better-scroll]
---

#  better-scroll

##  一、介绍

> [better-scroll:github]( https://github.com/ustbhuangyi/better-scroll )
>
> [better-scroll:官网]( https://better-scroll.github.io/docs/zh-CN/ )



###  安装

* 这种方式安装就包括安装了better-scroll所有插件

```
npm install better-scroll --save
```

###   基础使用

1. html 中需要滚动动的元素必须用 一个容器(wrapper)包裹

* 只有wrapper容器的第一个子元素有效果

```html
//html
//content元素有效果
<div class="wrapper" ref="wrapper">
    <ul class="content">
        <li></li>
    </ul>
</div> 
```

2. css 中设置 wrapper容器的高度或宽度 并设置 超过部分隐藏

```css
.wrapper{
	height: 150px ;     //1 设置 wrapper容器的高度或宽度
    overflow: hidden;   //2 设置 超过部分隐藏
}
```

3. js 中引入及创建

```js
import BScroll from 'better-scroll'

const bscroll = new BScroll(document.querySelector('.wrapper'))
```

**vue中建议用this.$refs.refname拿到元素或组件对象**

* 以`ref`的方式拿到的对象可以确保是唯一的
* 以`document.querySelector('.wrapper')`的方式拿到的对象可能不唯一  因为可能有多个class 名为：`wrapper`

```js
import BScroll from 'better-scroll'

const bscroll = new BScroll(this.$refs.wrapper)
```

###  注意事项

####  1 事件点击无效问题

* 设置`click:true`

####  2 pc端 鼠标滚轮滚动无效问题

* 设置`muoseWheel:true`



##  二、常用功能

###  监听滚动位置

1. 设置`probeType`属性为：2 或3

* `probeType`可取值
  * 默认值0  不能实时监听滚动位置
  * 1： 不能实时监听滚动位置
  * 2： 可监听实时滚动位置  但惯性滚动过程不能实时监听
  * 3： 可监听实时滚动位置  且惯性滚动过程也能实时监听

```js
import BScroll from ''

const bscroll = new BScroll(document.querySelector('.wrapper'),{
    probeType: 3
})
```

2. 监听`scroll`钩子函数

```js
//opsition中就可以查看当前滚动的实时位置
bscroll.on('scroll', (opsition) => {
    console.log(opsition)
})
```

###  上拉加载更多

1. 设置`pullUpLoad: true`

* `pullUpLoad`可取值
  * 默认值： false     上拉不能触发事件
  * true  : 上拉可以触发事件
  * { threshold :  数值}： threshold设置了上拉触发事件的阈值

```js
import BScroll from ''

const bscroll = new BScroll(document.querySelector('.wrapper'),{
    pullUpLoad: true
})
```

2. 监听`pullingUp`钩子函数

* **注意：**结束具体业务处理后需要执行`bscroll.finishPullUp()`函数  用于结束一次上拉加载   否则不能进行下一次下拉加载更多的操作

```js
//上拉加载事件处理
bscroll.on('pullingUp', () => {
    // 1 发送网络请求等操作
    
    // 2 结束上拉加载
    bscroll.finishPullUp()
})
```

###   回到顶部/滚动到指定位置

* 使用scrollTo(x, y, time, easing, extraTransform, isSilent)方法

- 参数：
  - {Number} x 横轴坐标（单位 px）
  - {Number} y 纵轴坐标（单位 px）
  - {Number} time 滚动动画执行的时长（单位 ms）
  - {Object} easing 缓动函数，一般不建议修改，如果想修改，参考源码中的 `packages/shared-utils/src/ease.ts` 里的写法





##  三、BUG解决

###  scroll可滚动区域计算与图片加载不匹配的问题

**根本原因：**

* 在scroll创建时会先计算可滚动区域的高度 
* 若滚动 区域里有异步加载的内容  例如图片
* 在计算可滚动区域高度时 图片可能还没有加载完成 ， 这种情况下就会使计算的高度偏小

**可能产生的表象问题：**

* 上拉加载更多时  可能界面上还没触底  就已经触发了上拉加载更多的操作

**解决方案：**

* 在每次数据加载完成时（图片加载完成时），刷新一下scroll  让scroll再重新计算一次可滚动区域的高度  这时高度的计算就包括了异步加载的内容（图片）的高度

* 刷新操作：`refresh()`方法

```
scroll.refresh
```

* 监听图片加载完成
  * 原生js监听图片：img.onload = function(){}
  * Vue中监听图片： @load= "方法"   







###  Vue框架下  页面保存状态 再次进入后仍有显示问题

**表象问题：**

* 路由设置了`<keep-alive>`     离开页面再次进入时应该保持原来的状态
* 但是页面中使用了`better-scroll`后再次回到页面会出现一些问题：
  * 不是原来的位置
  * 不能滑动

**解决方案：**

* 离开页面时保存`scroll`的位置
* 再次进入页面时根据保存的位置再次设置`scrol`l的位置    `scroll.scrollTo(x, y)`
* 重新设置位置后刷新`scroll`    `scroll.refresh()`

```
//进入页面
activated(){
    this.$refs.scroll.scrollTo(0, this.saveY, 0)  //重置位置
    this.$refs.scroll.refresh()    //刷新scroll
},
//离开页面
deactivated(){
    this.saveY = this.$refs.scroll.getScrollY()  //saveY 保存离开时的位置
},
```

