---
title: CSS(0x01)-css常用知识点
date: 2020-08-01 11:06:17
tags:
- css
categories:
- [css, css]
---

#  一、资料

###  1 转换base64格式

* [网址]( http://tool.chinaz.com/tools/imgtobase/ )

###  2 css色彩大全

*  [色彩地址1](https://www.cnblogs.com/xmm2017/p/8441124.html )

* [色彩地址2](https://www.w3school.com.cn/cssref/css_colors.asp )

###  3 阿里iconfont 字体图标

* [阿里iconfont官网]( https://www.iconfont.cn/ )

*  页面内使用iconfont

```
//1 在app.wxss中引入iconfont.wxss
@import "iconfont.wxss";

//2 使用1
 //<text class="iconfont 阿里上的代码"></text>
 <text class="iconfont icon-shangyishou-yuanshijituantubiao"></text>
    使用2 
   <i class="iconfont icon-fabu"></i>
```

* 组件内使用iconfont

  * 方法1

    ```
    1 将 iconfont.wxss 直接复制到组件文件里 
    2 组件wxss引入   @import "iconfont.wxss";
    ```

  * 方法2 

    ```
    //1 页面组件将样式传递给组件
    <w-search iconfont="iconfont" icon-sousuo="icon-sousuo"/>
    //2 组件接收
    externalClasses: [
        'iconfont',
        'icon-sousuo',
      ],
    // 3 组件wxml使用
    <i class="iconfont icon-sousuo  find"/>
    
    
    //注意事项
    在组件内不能修改iconfont的样式
    必须再定义class  例如：find
    .find {
      position: absolute;
    }
    ```

  * 方法3

    消除样式隔离 

    指南---自定义组件---组件模板和样式

    ```
    Component({
      options: {
        styleIsolation: 'apply-shared'
      }
    })
    
    ```

    



#  二、布局

###  1 flex布局

* [flex布局教程]( https://www.runoob.com/w3cnote/flex-grammar.html )

###  2 css布局模型

####  2.1 流动模型 flow

​    网页默认布局模型、块状自上而下、内联从左到右

####  2.2 浮动模型 float

   消除块状元素独占一行的特性

    float:left;
    float:right;


####  3.3 层模型 position

 #####  3.3.1  **绝对定位**(position: absolute) 

*  参照定位的元素（父元素）必须加入position:relative; 

 如果想为元素设置层模型中的绝对定位，需要设置**position:absolute**(表示绝对定位)，这条语句的作用将元素从  文档流中拖出来，然后使用left、right、top、bottom属性相对于其最接近的一个具有定位属性的父包含块进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于**浏览器窗口**。 

#####  3.3.2  **相对定位**(position: relative) 

 如果想为元素设置层模型中的相对定位，需要设置position:relative（表示相对定位），它通过left、right、top、bottom属性确定元素在**正常文档流中**的偏移位置。相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于**以前的位置移动，**移动的方向和幅度由left、right、top、bottom属性确定，偏移前的位置保留不动。 

#####  3.3.3  **固定定位**(position: fixed) 

 fixed：表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是视图（**屏幕内的网页窗口**）本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终位于浏览器窗口内视图的某个位置，不会受文档流动影响 。

###  

# 三、单位及适配

### 1单位

* vh（viewport height）: 视图高度  ----  vh的具体高度是根据设备而定  
  * 100vh 即整个视图的高度    ---- 及设备的整个视图界面的高度
* psd原稿的尺寸单位是：像素             1px = 2像素

###  2  适配

####  2.1 reset css

**`reset css`用于统一不同浏览器标签样式的默认设置**

* 不同浏览器对`body`等标签的`margin`、`padding`等默认设置是不同的

* `reset css`就是将这些不同的默认设置改为一样的设置，使得项目在不同浏览器的样式都一致

* [官网]( https://meyerweb.com/eric/tools/css/reset/ )

* 将下面代码添加到全局样式中

  ```js
  html, body, div, span, applet, object, iframe,
  h1, h2, h3, h4, h5, h6, p, blockquote, pre,
  a, abbr, acronym, address, big, cite, code,
  del, dfn, em, img, ins, kbd, q, s, samp,
  small, strike, strong, sub, sup, tt, var,
  b, u, i, center,
  dl, dt, dd, ol, ul, li,
  fieldset, form, label, legend,
  table, caption, tbody, tfoot, thead, tr, th, td,
  article, aside, canvas, details, embed, 
  figure, figcaption, footer, header, hgroup, 
  menu, nav, output, ruby, section, summary,
  time, mark, audio, video {
  	margin: 0;
  	padding: 0;
  	border: 0;
  	font-size: 100%;
  	font: inherit;
  	vertical-align: baseline;
  }
  /* HTML5 display-role reset for older browsers */
  article, aside, details, figcaption, figure, 
  footer, header, hgroup, menu, nav, section {
  	display: block;
  }
  body {
  	line-height: 1;
  }
  ol, ul {
  	list-style: none;
  }
  blockquote, q {
  	quotes: none;
  }
  blockquote:before, blockquote:after,
  q:before, q:after {
  	content: '';
  	content: none;
  }
  table {
  	border-collapse: collapse;
  	border-spacing: 0;
  }
  ```

###  2.2 宽高自适应

####  2.2.1 自适应屏宽高

```js
//用vh、vw来设置宽高
100vh: 100%的屏幕高
100vw： 100%的屏幕宽
```

#### 2.2.2 设置文档流固定高度后的剩余高度

* css3 中提供的`calc()`方法
* 应用场景：不知道设备的具体高度  要设置一个元素的高度为设备高度减去上导航和下导航高度之后的高度

```
.content{
	height: calc(100% - 98px)  //100%是设备的整个视图界面的高度（内容高度  要舍去padding margin）
}
```







#  四、常用效果

##  4.1布局相关

#### 4.1.1 滑动到固定位置则脱离标准流  ----->position:fixed

**语法：**

```
position: sticky;
top: 44px;         //top设置滑动到距离顶部多少时 脱离标准流(原理是设置成了fixed)
```

#### 4.1.2 原生实现局部区域的滚动

* **注意：**原生的滚动效果在移动端效果很不好，  一般使用[`better-scroll`](https://better-scroll.github.io/docs/zh-CN/ )框架

**语法：**

```
// html
<ul class="content">
    <li>分类列表1</li>
    
//css
 .content{
    height: 150px;             //1 为父元素设置高度
    background-color: red; 
    overflow: hidden;          //2 将超出的内容隐藏     ----- 有步骤3 此步骤可省略  
    overflow-y: scroll;        //3 设置在y轴是可以滚动的  
    overflow-x: scroll;        //3 设置在x轴是可以滚动的
  }
```

####  4.1.3 边框不占元素宽高

```
box-sizing:border-box;
```

####  4.1.4 块级元素居中

```
//上下左右居中  
position: absolute;
left:50%;
top: 50%;
transform: translate(-50%, -50%)
```





## 4.2文本相关

####   4.2.1 两行文字+'...'

```
  display: -webkit-box;
  -webkit-box-orient: vertical;    //	从顶部向底部垂直布置子元素
  -webkit-line-clamp: 2;           //保留两行          
  /* white-space: nowrap; */       //不换行
  overflow: hidden;
  text-overflow: ellipsis;         //’...‘样式
```

####  4.2.2 单行字符串+'...'

```css
text-overflow: ellipsis;
white-space: nowrap;
overflow: hidden;
```

####  4.2.3 文本头空两格

```js
.class{
	text-indent: 58px;
}
```



####  4.2.4 消除字体上下空白部分

字体默认情况下 上下有空白填充

```
//使字体行高=字体高度  可以消除部分上下空白部分  但不能完全消除
font-size: 24rpx;
line-height: 24rpx; //字体行高
```

## 4.3 动画效果

####  4.3.1 旋转效果

* 工具样式  直接使用下面的样式即可，不用全部弄懂  只需要在使用的地方使用rotation类即可

```
.rotation {
  -webkit-transform: rotate(360deg);
  animation: rotation 12s linear infinite;
  -moz-animation: rotation 12s linear infinite;
  -webkit-animation: rotation 12s linear infinite;
  -o-animation: rotation 12s linear infinite;
}

@-webkit-keyframes rotation {
  from {
    -webkit-transform: rotate(0deg);
  }

  to {
    -webkit-transform: rotate(360deg);
  }
}
```

####  4.3.2 渐变效果

* 过渡渐变

```css
.show{
	opacity: 1 ;  //透明度为1 即显示组件
    transition: all 1s ease-in ; // 1秒完成变化 以ease-in的方式完成
}

.hide{
    opacity: 0 ;  //透明度为0  即隐藏组件
    transition: all 1s ease-in ; // 1秒完成变化 以ease-in的方式完成
}
```

* 动画渐变

```css
.show{
    animation: show-item 1s ease-in forwards ; //forwards是在变化完成时保留渐变到100%的样式效果，若没有这个属性，变化的动画效果结束后仍然显示的是变化之前的样式
}

.hide{
    animation: hide-item 1s ease-in forwards ;
}

@keyframes show-item {
    0% {      //变化进行到0%时
       opacity: 0 ;
        color: red; 
    }
    50% {      //变化进行到50%时
       opacity: 0.5 ;
        color: green; 
    }
    100% {      //变化进行到100%时
       opacity: 1 ;
        color: blue; 
    }
}

@keyframes hide-item {
    0% {      //变化进行到0%时
       opacity: 1 ;
        color: red; 
    }
    50% {      //变化进行到50%时
       opacity: 0.5 ;
        color: green; 
    }
    100% {      //变化进行到100%时
       opacity: 0 ;
        color: blue; 
    }
}
```



#  五、常见问题



##  5.1去掉标签自带样式

###  5.1.1 去掉a标签自带样式

```css
a标签去除默认样式
/*包含以下四种的链接*/
a {
    text-decoration: none;
}
/*正常的未被访问过的链接*/
a:link {
    text-decoration: none;
}
/*已经访问过的链接*/
a:visited {
    text-decoration: none;
}
/*鼠标划过(停留)的链接*/
a:hover {
    text-decoration: none;
}
/* 正在点击的链接*/
a:active {
    text-decoration: none;
}
```

##  5.2 父元素的透明度设置影响子元素

**问题描述：**

* 设置父元素`opacity：0.5`，子元素不设置`opacity`，子元素会受到父元素opacity的影响，也会有0.5的透明度。

* 即使设置子元素`opacity：1`，子元素的`opacity：1`也是在父元素的`opacity：0.5`的基础上设置的，因此子元素的opacity还是0.5。

**解决方法：**

* 为父元素设置`background: rgba(0,0,0,0.5)`。



###   