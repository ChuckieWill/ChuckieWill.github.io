---
title: CSS(0x02)-css随笔
date: 2020-02-22 12:01:17
tags:
- css
categories:
- [css, css]
---
####  nth-child() 样式选择

* 用于选择循环中的某个指定的元素

```
<block wx:for="{{comments}}" wx:key="content">
    <w-tag comment="{{item.content}}">
        <text slot="after" class="num">{{'+'+item.nums}}</text>
    </w-tag>
</block>

//选择遍历的w-tag中的第一个 并选择该自定义组件内部的view 将其的背景颜色改变
.comment-container > w-tag:nth-child(1) > view{
    background-color: #fffddd;
}
//选择遍历的w-tag中的第二个 并选择该自定义组件内部的view 将其的背景颜色改变
.comment-container > w-tag:nth-child(2) > view{
    background-color: #eefbff;
}
```

#### 后代选择器与子选择器的区别

 后代选择器与子选择器的区别，子选择器（child selector）仅是指它的直接后代，或者你可以理解为作用于子元素的第一代后代。而后代选择器是作用于所有子后代元素。后代选择器通过空格来进行选择，而子选择器是通过“>”进行选择。

**总结：**

* 子选择器：**>**作用于元素的第一代后代
* 后代选择器：**空格**作用于元素的所有后代。

####  filter

 **`filter `**CSS属性将模糊或颜色偏移等图形效果应用于元素。滤镜通常用于调整图像，背景和边框的渲染。 

#### opacity

 opacity属性指定了一个对象或一组对象的透明度，也就是说，元素后面的背景的透过率。 

####  box-sizing

  **`box-sizing`** 属性定义了 [user agent](https://developer.mozilla.org/en-US/docs/Glossary/user_agent) 应该如何计算一个元素的总宽度和总高度。 

```
content-box
尺寸计算公式：
width = 内容的宽度
height = 内容的高度

border-box
尺寸计算公式：
width = border + padding + 内容的宽度
height = border + padding + 内容的高度
```



#  面试题

##  0 总

* HTML元素的嵌套关系是怎么确定的，哪些嵌套不可以发生（a中包含div）
* CSS选择器的权重是如何计算的, 写代码时要注意什么
* 浮动布局是怎么回事，有什么优缺点, 国内用得多吗
* CSS可以做逐帧动画吗，性能如何
* Bootstrap怎么做响应式布局
* 如何解决CSS模块化过程中的选择器互相干扰问题
* 浏览器解析CSS样式的方式
  * **从右往左解析**，解析效率更高

##  1选择器

* 选择器
  
  * css选择器优先级，权重计算

##  2 非布局样式

* 非样式布局
  * 雪碧图的作用
  
  * 自定义字体的使用场景------------待完成
  
  * base64的使用，性能分析
  
  * 伪类和伪元素的区别   ------------------待完成
  
  * 如何美化checkbox
  
    * 核心要素
      * `display: none;` 隐藏checkbox, 用label来控制选择
      * `+`号相邻兄弟选择器的使用，可以控制checkbox选中与否状态下的label样式
    * `coding-164/chapter3/chechbox.html`
  
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>checkbox</title>
        <style>
            .checkbox input{
                display: none;
            }
            .checkbox input + label{
                /* left ： 左右居左， center上下居中 */
                background:url(./checkbox1.png) left center no-repeat;
                background-size:20px 20px;
                padding-left:20px;
            }
            .checkbox input:checked + label{
                background-image:url(./checkbox2.png);
            }
        </style>
    </head>
    <body>
        <div class="checkbox">
            <input type="checkbox" id="handsome"/>
            <label for="handsome">我很帅</label>
        </div>
    </body>
    </html>
    ```
  

##  3 布局

* 实现两栏(三栏)布局的方法
  * 表格布局
  * float + margin布局
  * inline-block布局
  * flexbox布局
* position:absolute / fixed有什么区别?
  * 前者相对最近的absolute/relative
  * 后者相对窗口
* inline-block布局中display:inline-block间隙问题
* float布局中的清除浮动
* 如何适配移动端页面?
  * viewport
  * rem/ viewport / media query
  * 设计上:隐藏、折行、自适应





