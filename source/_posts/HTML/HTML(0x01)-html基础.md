---
title: HTML(0x01)-html基础
date: 2020-01-23 10:43:17
tags:
- html
- 浏览器
categories:
- [html]
---

# HTML 

##  1 浏览器内核

Trident （ 三叉戟）：IE、360安全浏览器、360极速浏览器、搜狗高速浏览器、百度浏览器、UC浏览器

Gecko（ 壁虎） ：Mozilla Firefox

Presto（急板乐曲）-> Blink （眨眼）：Opera

Webkit ：Safari、360极速浏览器、搜狗高速浏览器、移动端浏览器（Android、iOS）

Webkit -> Blink ：Google Chrome



##  2 HTML常见元素

meta

* `width=device-width`:   页面宽度等于设备视口宽度  **适配移动端的第一步**
* `initial-scale=1.0`:  默认缩放比是1
* `maximum-scale=1.0`： 最大缩放比是1
* `user-scalable=no`:   用户不可缩放

```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```



##  3 HTML元素分类

###  3.1 按默认样式分

> 用`display`属性设置

* **块级  block**

  * 块级元素特点：

    1、每个块级元素都从新的一行开始，并且其后的元素也另起一行。

    2、元素的高度、宽度、行高以及顶和底边距都可设置。

    3、元素宽度在不设置的情况下，是它本身父容器的100%（和父元素的宽度一致），除非设定一个宽度。

  * 常见块级元素：`<div>、 <p>、<h1>、<form>、<ul> 、<li>`

* **行内  inline**

  * 内联元素特点：

    1、和其他元素都在一行上；

    2、元素的高度、宽度及顶部和底部边距**不可**设置；

    3、元素的宽度就是它包含的文字或图片的宽度，不可改变。

  * 常见行内元素：`<span>、<a>、<label>、 <strong>、<em>、<button>`

* **inline-block**

  * inline-block 元素特点：

    1、和其他元素都在一行上；

    2、元素的高度、宽度、行高以及顶和底边距都可设置。
    
  * 常见内联块级元素：`<img>、<input>`

###  3.2 按内容分

> HTML5新增分类方式
>
> [官网](https://html.spec.whatwg.org/multipage/dom.html#phrasing-content)

![image-20201118165349115](HTML(0x01)-html基础/image-20201118165349115.png)

#  面试问题

##  1 HTML版本

* HTML4/4.01 (SGML)
* XHTML(XML): 严格约束各种规则
* HTML5

![image-20201118161807688](HTML(0x01)-html基础/image-20201118161807688.png)

###  1.1 HTML、HTML5、XHTML的关系

* HTML属于SGML
* XHTML属于XML，是HTML进行XML严格化的结果
* HTML 5不属于SGML或XML，比XHTML宽松



##  2 HTML5的变化

> * 新的语义化元素
> * 表单增强
> * 新的API (离线、音视频、图形、实时通信、本地存储、设备能力)
> * 分类和嵌套变更

### 2.1 HTML5新增内容

* 新区块标签
  * section
  * article
  * nav
  * aside
* 表单增强
  * 时间、日期、搜索
  * 表单验证
  * Placeholder自动聚焦


###  2.2 HTML5新增语义

* header/footer  头尾
* section/article 区域
* nav   导航
* aside   不重要的部分
* em/strong   强调
* i    icon图标

## 3 DOCTYPE的意义

* 让浏览器以标准盒模式渲染
  * 例如IE的padding是包含在width里的，但是标准模式下padding是不包含在width里的

* 验证浏览器元素的合法性
  * `<!DOCTYPE html>` 就会以`html5`的标准检测文档

##  4 嵌套关系

###  4.1 基本原则

* 块级元素可以包含行内元素
* 行内元素一般不能包含块级元素（在某些情况下a可以包含div）
* 块级元素不一定可以包含块级元素（p不能包含div）

###  4.2 a包含div是否合法

* 以下情况合法

  ```html
  <div>
    <a href="#">DIV > A</a>
  </div>
  <a href="#">
    <div>A > DIV</div>
  </a>
  ```

  * 合法的原因是：**`<a>`标签在嵌套合法性验证的过程中会被忽略，**所以上面的情况相当只有div标签，所以是合法的

* 以下情况是不合法的

  ```html
  <p>
      <a href="#">
        <div>P > A > DIV</div>
      </a>
  </p>
  ```

  * 不合法的原因是：去掉a标签后做合法性验证，此时为:

    ```html
    <p>
          <div>P > A > DIV</div>
    </p>
    ```

  * 而p标签是不能嵌套div的，所以不合法

##  5 em和i的区别

* em是**语义化**的标签，表强调
* i是纯样式的标签，表斜体
* HTML .5中i不推荐使用，一般用作图标

##  6 语义化的意义是什么

* 开发者容易理解，易读可读
* 机器容易理解结构(搜索、读屏软件)、有助于SEO

##  7 哪些元素可以自闭合

* input
* img
* br hr
* meta  link

##  8 HTML和DOM的关系

* HTML是“死”的，只是一堆字符串
* DOM是由HTML解析而来的，是“活”的
* JS可以维护DOM

##  9 property和attribute的区别

* attribute是html的属性，设置之后是不变的，只是用于生成DOM是给DOM的property一个初始值
* property是dom的特性（其实也是属性），只是这个属性是可以根据dom操作或者用户的输入而改变的

##  10 form的作用有哪些

* 直接提交表单
* 使用submit / reset按钮
* 便于浏览器保存表单
* 第三方库可以整体提取值
* 第三方库可以进行表单验证

##  附录

###  1 From表单详细教程

> [From表单教程]( https://blog.csdn.net/Jonsnow1457/article/details/88781945 )

