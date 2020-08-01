---
title: CSS(0x02)-css随笔
date: 2020-08-01 12:01:17
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

