---
title: CSS(0x01)-css常用知识点
date: 2020-02-22 11:06:17
tags:
- css
categories:
- [css, css]
---

#  零、资料

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


# 一、选择器

###  1.1选择器分类 ------待完成

* 通用选择器`*{}`
* 元素(标签)选择器`a{}`
* 类选择器`.link{}`
* ID选择器`#id{}`
* 伪类选择器`:hover{}`   例如:`a:hover{}`   用于设置**鼠标滑过的状态**
* 伪元素选择器`::before{}`
* 组合选择器`[type=checkbox] + label{}`
* 否定选择器`:not(.link){}`
* 属性选择器`[type: =radio]{}`

###  1.2选择器权重

> 注意：**权重累加不进位**，例如：11个类选择器权重任然小于1个ID选择器
>
> 内联样式 > id选择器 > 类选择器 > 标签选择器 > 通配符选择器

* ID选择器  +100
* 类选择器 、属性选择器、伪类选择器   +10
* 元素(标签)选择器、伪元素选择器    +1
* 其它选择器+0
* !important 优先级最高  **高于所有**
* 元素属性优先级高   **高于ID选择器**
  * 即写在HTML标签的属性上`<div style="...."> </div>`
* 相同权重后写的生效

###  1.3子选择器与后代选择器

* 子选择器`>`  :  作用于元素的**第一代后代**

  ```html
   <ul class="con">
        chuckie
        <li >你好</li>
        <li >我好</li>
        <ul>
          <li>hhhh</li>
          <li>hhhh</li>
          <li>hhhh</li>
        </ul>
      </ul>
  ```

  ```css
  .con>li{
     color: red; 
  }
  ```

  * 运行结果： 只有"你好"“我好”为红色， “hhhh”为默认颜色

* 后代选择器` 空格`： 作用于元素的**所有后代**

  ```css
  .con li{
     color: red; 
  }
  ```

  * 运行结果："你好"“我好”“hhhh”都为红色

###  1.4 相邻兄弟选择器

相邻兄弟选择器能够匹配指定元素后面相邻的兄弟元素。

如果需要选择紧接在一个元素后的元素，而且二者有相同的父元素，可以使用相邻兄弟选择器

例如下面：`.checkbox input + label`表示选择checkbox下的input元素的相邻兄弟元素label; `.checkbox input:checked + label`表示选择checkbox下的input选中状态的相邻兄弟元素label。

```html
    .checkbox input + label{
        background:url(./checkbox1.png) left center no-repeat;
        background-size:20px 20px;
        padding-left:20px;
    }
    .checkbox input:checked + label{
        background-image:url(./checkbox2.png);
    }

    <div class="checkbox">
        <input type="checkbox" id="handsome"/>
        <label for="handsome">我很帅</label>
    </div>
```

# 二、非布局样式

###  2.1字体样式

####  2.1.1字体

* 字体族

  * serif  ：衬线字体，例如宋体
  * sans-serif ： 非衬线字体，例如黑体
  * monospace： 等宽字体（每个字母都是等宽的）
  * cursive ： 手写体
  * fantasy： 花体

* 多字体fallback

  ```css
  font-family: "PingFang SC", "Microsoft Yahei", monospace;
  ```

  *先匹配`PingFang SC`、若匹配不到在再匹配`Microsoft Yahei`, 匹配不到再匹配`monospace`(等宽字体)*

  *注意：匹配字体族**不**加双引号*

* 网络字体、自定义字体

  ```html
  <head>
      <style>
          @font-face {
              font-family: "IF";
              src: url("./IndieFlower.ttf");//自定义字体文件
          }
          .custom-font{
              font-family: IF;
          }
      </style>
  </head>
  <body class="body" id="body">
      <div class="custom-font">你好 Hello World</div>
  </body>
  
  ```

  *`./IndieFlower.ttf`为自定义字体文件*

* iconfont

####  2.1.2 行高

#####  2.1.2.1 理解`line-box`

**代码：**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>inline</title>
    <style>
        span{
            background:red;
        }
        .c1{
            line-height: 20px;
        }
        .c2{
            line-height: 8px;
        }
        .c3{
            line-height: 30px;
        }
        .c5{
            line-height: 28px;
        }
    </style>
</head>
<body>
    <div>
        <span class="c1">inline box xfg中文</span>
        <span class="c2">inline box</span>
        <span class="c3">inline box</span>
        inline box
        <span class="c5">inline box</span>
    </div>
</body>
</html>
```

**效果：**

![image-20201120151507282](CSS(0x01)-css常用知识点/image-20201120151507282.png)

**解释：**

* 上面一个红色块称为一个`inline-box`,任何一个字体块都会形成一个`inline-box`
  * inline-box的底部就是字体的底线
  * inline-box的上部就是字体的顶线
  * 字体是以**基线对齐的**，基线见上图，**行内元素都是以基线对齐的，包括img等**
* 上面的棕色块称为一个`line-box`，一个或多个`inline-box`就会形成一个`line-box`
  * `line-box`的高度由`line-height`属性设置的值决定，同时`line-box`会撑起外部块状元素的高度
  * `line-box`的高度是指一行的高度，注意它是可以换行的



#####  2.1.2.2 字体居中与对齐

**字体上下居中：**

* `line-height`属性只作用于行内元素
* `line-height`效果优先于`vertical-align`,  同时设置了`line-height`和`vertical-align:bottom`，文字会居中，而不是以底线对齐。

```css
font-size:**px;
line-height: **px;  //设置的line-height大于字体像素时，字体就会在inline-box中上下居中
```

**字体上下居中对齐：**

* 源码：

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>line-height</title>
      <style>
          .cc1{
              font-size:12px;
              vertical-align: middle;//字体大小不同时，以中线对齐
          }
          .cc2{
              font-size:18px;
              vertical-align: middle;
          }
          .cc3{
              font-size:24px;
              vertical-align: middle;
          }
      </style>
  </head>
  <body>
      <div class="c1">
          <span class="cc1">第一段</span>
          <span class="cc2">第二段</span>
          <span class="cc3">第三段</span>
      </div>
  </body>
  </html>
  ```

* 运行效果：居中对齐

  ![image-20201120203513476](CSS(0x01)-css常用知识点/image-20201120203513476.png)

* 设置`vertical-align`属性

  * `vertical-align:middle`: 中线对齐，*注意：需要多个字体块同时设置才有效，因为这个中线是参与字体的相对中线，例如上面三个字体块中线对齐就需要三个同时设置这个属性*
  * `vertical-align:bottom`: 底线对齐
  * `vertical-align:top`: 顶线线对齐
  * `vertical-align:*px`:  可以设置具体移动的大小（单位px）,以基线为基准向上向下调整



#####  2.1.2.3 图片空隙问题

**源码：**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>line-height</title>
    <style>
        .cc1{
            font-size:12px;
            /*vertical-align: middle;*/
        }
        .cc2{
            font-size:18px;
            /* vertical-align: bottom; */
        }
        .cc3{
            font-size:24px;
            /* vertical-align: bottom; */
        }
    </style>
</head>
<body>
    <div style="background:red">
        <span>文字</span>
        <img src="test.png"/>
    </div>
</body>
</html>
```

**效果：**

* 问题：**图片下面会多出空隙，即是没有文字部分，div包裹图片，图片下部与div底部之间也会有间隙**
* 原因：行内元素是以基线对齐的，即文字的基线与图片的底部对齐（图片的底部为基线）

![image-20201120204457480](CSS(0x01)-css常用知识点/image-20201120204457480.png)

**解决方案：**

* div只包裹图片时，设置div高度与图片高度一样就可以消除间隙
* div同时包括图片和其它行内或行内块状元素时
  * 图片设置`vertical-align: bottom`, 即以底线对齐，而不是以基线对齐

#####  2.1.2.4  消除字体上下空白部分

* 字体默认情况下 上下有空白填充
* 使字体行高=字体高度 

```
//使字体行高=字体高度  可以消除部分上下空白部分  但不能完全消除
font-size: 24rpx;
line-height: 24rpx; //字体行高
```





字重、颜色、大小、行高

###  2.2 盒子

####  2.2.1 背景

#####  2.2.1.1 背景颜色

* 文字：`background: black`
* 十六进制：`background: #FF0000;`：红色；
* rgba: `background:rgba(255,0,0, .3);`: `background:rgba(255,0,0, 透明度);`
  * rgb: `background:rgb(255,0,0);`: `background:rgb(255,0,0);`
* hsla: `background:hsla(230,10%,20%, .3);`: `background:hsla(色相,饱和度,亮度, 透明度);`
  * hsl: `background:hsl(230,10%,20%);`: `background:hsl(色相,饱和度,亮度);`

#####  2.2.1.2 渐变色背景

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>background</title>
    <style>
        .c2{
            height:400px;
            
            background: -webkit-linear-gradient(left, red, green);//从左到右：red-->green
            background: linear-gradient(to left, red, green);     //从右到左：red-->green   
            background: linear-gradient(to right, red, green);    //从左到右：red-->green
            background: linear-gradient(to top, red, green);      //从下到上：red-->green
            background: linear-gradient(to bottom, red, green);   //从上到下：red-->green

            background: linear-gradient(0deg, red, green);   //从下到上： red-->green
            background: linear-gradient(45deg, red, green);  //从左下到右上：red-->green
            background: linear-gradient(90deg, red, green);  //从左到右： red-->green
            background: linear-gradient(135deg, red, green); //从左上到右下：red-->green
            background: linear-gradient(180deg, red, green); //从上到下：  red-->green

            background: linear-gradient(90deg, red 0, green 10%, yellow 50%, blue 100%);
            //从左到右，依次0%位置：red; 10%位置green; 50%位置yellow; 100%位置blue; 百分比中间位             //置为渐变区域
        }
    </style>
</head>
<body>
    <div class="c2"></div>
</body>
</html>
```



#####  2.2.1.3 多背景叠加

**单条纹：**

* 源码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>background</title>
    <style>
        .c2{
            height:400px;
            border: blue 1px solid;
            background: linear-gradient(135deg, transparent 0, transparent 49.5%,
                      green 49.5%, green 50.5%, transparent 50.5%, transparent 100%),
                        linear-gradient(45deg, transparent 0, transparent 49.5%,
                      red 49.5%, red 50.5%, transparent 50.5%, transparent 100%);
            /* background-size: 30px 30px; */
        }
    </style>
</head>
<body>
    <div class="c2"></div>
</body>
</html>
```

* 语法
  * `linear-gradient(135deg, transparent 0, transparent 49.5%,green 49.5%, green 50.5%, transparent 50.5%, transparent 100%)`:  从左上到右下，0%到49.5%位置透明，49.5%到50.5%位置green，50.5%到100%位置透明
  * `linear-gradient(45deg, transparent 0, transparent 49.5%, red 49.5%, red 50.5%, transparent 50.5%, transparent 100%)`:  从左下到右上，0%到49.5%位置透明，49.5%到50.5%位置red，50.5%到100%位置透明
* 效果：

![image-20201121193408694](CSS(0x01)-css常用知识点/image-20201121193408694.png)

**多背景叠加：**

* 源码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>background</title>
    <style>
        .c2{
            height:400px;
            border: blue 1px solid;
            background: linear-gradient(135deg, transparent 0, transparent 49.5%,
                      green 49.5%, green 50.5%, transparent 50.5%, transparent 100%),
                        linear-gradient(45deg, transparent 0, transparent 49.5%,
                      red 49.5%, red 50.5%, transparent 50.5%, transparent 100%);
            background-size: 30px 30px;
        }
    </style>
</head>
<body>
    <div class="c2"></div>
</body>
</html>
```

* 语法：
  * `background-size: 30px 30px;`: 设置了一个重复块的大小，就是设置了上面单条纹的大小
* 效果：

![image-20201121194123662](CSS(0x01)-css常用知识点/image-20201121194123662.png)



#####  2.2.1.4 背景图片和属性（雪碧图)

**属性讲解：**

* 案例源码：

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>background</title>
      <style>
          .c1{
              height:400px;
              background: red url('./test.png');
              background-repeat: no-repeat;
              background-position: center center;
              /* background-position: 20px 30px; */
              /* background-size:100px 50px; */
          }
      </style>
  </head>
  <body>
      <div class="c1"></div>
  </body>
  </html>
  ```

* 效果

  ![image-20201121195439236](CSS(0x01)-css常用知识点/image-20201121195439236.png)

* 属性

  * `background: red url('./test.png');`:   背景可以同时设置背景颜色和背景图，背景图之外的位置就显示背景色的颜色，如上图
  * `background-repeat: no-repeat;`
    * no-repeat:  背景图不重复
    * repeat-y:   在y轴上重复
    * repeat-x:   在x轴上重复
  * `background-position: center center;`
    * center  center :   背景图在上下左右居中
    * center  top  :        背景图在左右居中，上下的上部
    * center  bottom：背景图在左右居中，上下的下部
    * npx    mpx：        背景图相对默认位置（左上角）向右移动npx,  向下移动mpx
  * `background-size:100px 50px`:    设置背景图的图片大小
    * cover：  等比例放大缩小，并铺满容器
    * contain:   等比例放大缩小，并完整显示在容器中（长或宽占满容器，另一个方向则有空白）

**雪碧图：**

> 雪碧图的意思是：将需要展示的图标集中到一张图片上，通过css调节图片位置，显示出所有图标
>
> 这样做的意义是：减少网络请求，如果是一个个图标请求，就会发送多个请求，但是集中到一张图片上就只用发送一次网络请求

**案例：**

将下面这张图的两个红色图标分开显示在界面上

![image-20201121201052990](CSS(0x01)-css常用知识点/image-20201121201052990.png)

* 源码：

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>background</title>
      <style>
          .c2{
              width:20px;
              height:20px;
              background:url(./test_bg.png) no-repeat;
              background-position: -17px -5px;
              background-size: 261px 113px;
          }
          .c3{
              width:20px;
              height:20px;
              margin-top: 10px;
              background:url(./test_bg.png) no-repeat;
              background-position: -169px -23px;
              background-size: 261px 113px;
          }
      </style>
  </head>
  <body>
      <div class="c2">
      </div>
      <div class="c3">
      </div>
  </body>
  </html>
  ```

* 效果：

  ![image-20201121201214317](CSS(0x01)-css常用知识点/image-20201121201214317.png)

* 解析：

  * 主要是利用了`background-position`属性，首先设置只展示图标大小的容器，然后将背景图中的图标调整到容器位置展示



#####  2.2.1.5 base64和性能优化

>[图片转base64在线网址]( http://tool.chinaz.com/tools/imgtobase/ )

* base64优势
  * 不用请求图片，减少了http连接数
* base64缺点
  * 增大了体积的开销
    * base64加密后图片体积会放大3/1， 也就是图片会变成原来的3/4
    * css文件也会变大，因为base64解码代码很长
  * 增大了解码的开销
* base64使用场景：  **小图标**



#####  2.2.1.6 多分辨率适配

**多分辨率适配问题的由来：**

* 移动端一般分辨率比较高，而pc端相对较低，在编码时如果图片直接按`1:1`的尺寸来展示就会使得移动端看着很模糊

**解决方案：**

* 将图片以`1:n`的比例展示，即将图片缩小展示

**案例解析：**

* 假设图片大小为400px*400px
* 按`1:1`展示： ` background-size: 400px 400px;`
* 按`1:4`展示:   `background-size: 100px 100px;`   ,在这种情况下，移动端显示就会更加清晰



####  2.2.2 边框

> [css3边框](https://www.runoob.com/css3/css3-borders.html)
>
> [图像边框](https://www.runoob.com/cssref/css3-pr-border-image.html)
>
> * 边框要点：
>   * 利用边框颜色交汇处的斜切实现**css绘制三角形**
>   * 利用图片制作图形边框

**案例：**

资源：border.png

![image-20201121212940001](CSS(0x01)-css常用知识点/image-20201121212940001.png)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>border</title>
    <style>
        //边框基本样式
        .c1{
            width:400px;
            height: 200px;
            /* border: 1px solid red; */
            /* border:5px solid red; */
            /* border:5px dotted red; */
            border:5px dashed red;//虚线效果
        }
        //制作图形边框
        .c2{
            width:400px;
            height: 200px;
            /* border-width: 30px; */
            border:30px solid transparent;//边框宽30px，实线，透明
            border-image:url(./border.png) 30 round;//用图片制作图像边框
        }
        //css制作三角形
        .c3{
            width:0px;
            height: 20px;
            border-bottom:30px solid red;
            /* border-right:30px solid blue; */
            border-left:30px solid transparent;
            border-right:30px solid transparent; //边框宽30px，实线，透明
        }
    </style>
</head>
<body>
    <div class="c1">

    </div>
    <div class="c2">

    </div>
    <div class="c3">

    </div>
</body>
</html>
```

效果：

![image-20201121212838218](CSS(0x01)-css常用知识点/image-20201121212838218.png)

###  2.3 滚动

> 更好用的滚动库：**[better-scroll](https://chuckiewill.github.io/2020/02/10/third-repositories/better-scroll/better-scroll/)**

![image-20201121213659679](CSS(0x01)-css常用知识点/image-20201121213659679.png)



**实现滚动的条件：**

* 容器设施固定高度
* 容器内的内容在容器高度内无法全部展示

**滚动属性：**

*mac上的效果如上图所示，windows上的效果看下面解释*

* `overflow:visable`： 同mac
* `overflow:hidden`:  同mac， 不能滚动
* `overflow:scroll`: 可滚动，上下和左右都有滚动条
* `overflow:auto`:  可滚动，根据内容情况显示上下或左右滚动条
* 不设置`overflow`时，效果同`overflow:visable`



###  2.4 文本

> [css3文本效果](https://www.runoob.com/css3/css3-text-effects.html)

####   2.4.1 两行文字+'...'

```
  display: -webkit-box;
  -webkit-box-orient: vertical;    //	从顶部向底部垂直布置子元素
  -webkit-line-clamp: 2;           //保留两行          
  /* white-space: nowrap; */       //不换行
  overflow: hidden;
  text-overflow: ellipsis;         //’...‘样式
```

####  2.4.2 单行字符串+'...'

```css
text-overflow: ellipsis;//三个点的效果
white-space: nowrap;//单行显示，不换行
overflow: hidden;//隐藏超出容器的部分
```

####  2.4.3 文本头空两格

```js
.class{
	text-indent: 58px;
}
```

####  2.4.4 单词拆分换行

* `word-break`
  * 注意：`word-break: keep-all`时，**中日韩的一段话也不能换行**，相当于把一段话也当做的一个单词

```css
word-break: break-all//所有单词都能拆分换行
word-break: keep-all //所有单词都不能拆分换行,包括中日韩汉的一段话也不能换行
```

* `overflow-wrap`或`word-wrap`

```css
overflow-wrap: break-word;//只有仅仅一段话或者一个单词就超过容器的宽度后才拆分
```



###  2.5 装饰

####  2.5.1 粗体

```css
font-weight: normal; //等于400
font-weight: bold;   //等于700
font-weight: bolder;
font-weight: lighter;
font-weight: 200;    //100,200,,,,900
```

####  2.5.2 斜体

```css
font-style: italic;
```

####  2.5.3 下划线

> [菜鸟教程下划线](https://www.runoob.com/cssref/pr-text-text-decoration.html)

```css
text-decoration: none;//去掉下划线，例如a标签的下划线
```

####  2.5.4 鼠标指针

> [菜鸟教程鼠标指针](https://www.runoob.com/cssref/pr-class-cursor.html)
>
> cursor属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状

```
cursor: pointer;//手型
```





#  三、布局

##  0 盒子模型

元素宽=内容宽+padding+border

元素高=内容高+padding+border

margin左右不会重叠，**上下交界的部分会重叠**

## 1 flex布局

> 用`display:flex`属性设置

* [flex布局教程]( https://www.runoob.com/w3cnote/flex-grammar.html )

## 2 元素的显示类型

> 用`display`属性设置

* **块级  block**

  * 块级元素特点：

    1、每个块级元素都从新的一行开始，并且其后的元素也另起一行。

    2、元素的高度、宽度、行高以及顶和底边距都可设置。

    3、元素宽度在不设置的情况下，是它本身父容器的100%（和父元素的宽度一致），除非设定一个宽度。

  * 常见块级元素：`<div>、 <p>、<h1>、<form>、<ul> 、<ol>、<li>`

* **行内  inline**

  * 内联元素特点：

    1、和其他元素都在一行上；

    2、元素的高度、宽度及顶部和底部边距**不可**设置；

    3、元素的宽度就是它包含的文字或图片的宽度，不可改变。

  * 常见行内元素：`<span>、<a>、<label>、 <strong>、<em>`

* **inline-block**

  * inline-block 元素特点：

    1、和其他元素都在一行上；

    2、元素的高度、宽度、行高以及顶和底边距都可设置。

  * 常见内联块级元素：`<img>、<input>`

### 2.1 inline-block布局

> inline-block布局不利于适配，因为字体是固定的，只适合一些固定字体的布局

* inline-block布局就是将块级元素转化为内联块级元素，这样块级元素就可以在一行显示了
* 但是这种布局多个块级元素直接会有**间隙**，原因是转化为内联块级元素后一个块级就相当于一个字，而每个字之间不是紧密相连的，而是有空隙的，
* 所以解决方案就是将父元素的`font-size`设置为0,字体大小为0 ，自然空隙也就为0，就没有空隙了
* 同时子元素设置自己的字体大小，因为父元素`font-size:0`，子元素不设置自己的大小就无法显示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .container{
            width:800px;
            height:200px;
            font-size:0;     //关键
        }
        .left{
            font-size:14px;  //关键
            background:red;
            display: inline-block;
            width:200px;
            height:200px;
        }
        .right{
            font-size:14px;   //关键
            background:blue;
            display: inline-block;
            width:600px;
            height:200px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="left">
            左
        </div>
        <div class="right">
            右
        </div>
    </div>
</body>
</html>
```

## 3 css布局模型

### 3.1 流动模型 flow

​    网页默认布局模型、块状自上而下、内联从左到右

### 3.2 浮动模型 float

##### 3.2.1 基础特性

* 1 消除块状元素独占一行的特性
* 2 元素设置float后会变成一个块 ，即可以设置宽高了， 例如`<span>、<a>、<label>、 <strong>、<em>`元素设置float后就可以设置宽高了

#####  3.2.2 没有脱离文本流

* 设置了float的元素**脱离了文档流**，即不对其他标签的布局产生影响，但是**没有脱离文本流**，即会对其他标签的文本内容显示产生影响

  * p1是在p2上面的，也就是p2其实是包含了p1（绿色块）部分的，只是p1脱离了文档流，所以在上面一层，覆盖了下面的红色
  * 但是p1没有脱离文本流，所以会将p2中的文字挤到右边和下边去

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
      <style>
          .container{
              background:red;
              width:400px; 
              margin:20px;
          }
          .p1{
              background:green;
              float:left;
              width:200px;
              height:50px;
          }
  
          
      </style>
  </head>
  <body>
      <div class="container">
          <span class="p1">
              float
          </span>
          <div class="p2">
              很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字很长的文字
          </div>
      </div>
  
  </body>
  </html>
  ```

  ![image-20201122164030542](CSS(0x01)-css常用知识点/image-20201122164030542.png)

#####  3.2.3 父元素高度塌陷（清除浮动）

* 设置float后，元素会脱离文档流，所以不能撑起父元素，此时就会出现常见问题：**父元素高度塌陷**
  * 高度塌陷的原因是：父元素没有设置高度，子元素脱离文档流后父元素不会去计算它的高度并调整高度
  * p1（绿色块）没有撑起container（红色块）
  * 下一个div(蓝色块) 是直接从container（红色块）下直接开始布局的，而不是p1的下面开始布局，证明了p1是脱离了文档流的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .container{
            background:red;
            margin:20px;
            width:500px;
        }
        .p1{
            background:green;
            float:left;
            width:200px;
            height:50px;
        }
        
    </style>
</head>
<body>
    <div class="container ">
        <span>写几个字</span>
        <span class="p1">
            float
        </span>
        <span class="p1">
            float
        </span>
        
    </div>

    <div class="container" style="height:200px;background:blue;">
    </div>
</body>
</html>
```

![image-20201122164933713](CSS(0x01)-css常用知识点/image-20201122164933713.png)

* 解决方案

  * 方案1： 给设置了float的元素的父元素设置`overflow`, 设置下面两个都可以使得父元素的高度被撑起来
    * 本质是让父元素主动计算了其内容部元素的总高度，并做了高度的调整，

  ```html
  overflow:hidden
  overflow:auto
  ```

  * 方案2： 给父元素设置一个伪元素

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
      <style>
          .container{
              background:red;
              width:500px;
              margin:20px;
          }
          .p1{
              background:green;
              float:left;
              width:200px;
              height:50px;
          }
          .container2::after{
              content: '';//必须设置伪元素的内容，否则不会生效
              clear:both;//使伪元素两边都没有float元素
              display: block;//使伪元素为块状元素，独占一行，这时伪元素就在父元素的最下面，且在float元素下面
              visibility: hidden;//隐藏content内容
              height:0;//设置伪元素高度为0，不影响父元素的高度
          }
          
      </style>
  </head>
  <body>
      <div class="container container2">
          <span>写几个字</span>
          <span class="p1">
              float
          </span>
          <span class="p1">
              float
          </span>
          
      </div>
  
      <div class="container" style="height:200px;background:blue;">
      </div>
  </body>
  </html>
  ```

  ![image-20201122171040743](CSS(0x01)-css常用知识点/image-20201122171040743.png)

##### 3.2.4 多栏典型布局

* 注意布局顺序
  * 若与设置float元素同级的元素是行内元素或行内块状元素，则不用考虑顺序
  * 若与设置float元素同级的元素是块级元素则需要考虑顺序，因为块级元素独占一行，即是设置float的元素会脱离文档流，但是仍然无法覆盖块状元素，所以布局时需要将块级元素放在最后，例如下面的middle放在left和right后面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .container{
            width:800px;
            height:200px;
        }
        .left{
            background:red;
            float:left;
            height:100%; 
            width:200px;
        }
        .right{
            background:blue;
            float:right;
            width:200px;
            height:100%;
        }
        .middle{//以下设置让中间元素的内容正好在中间显示
            margin-left:200px;//核心点，抵消左元素的宽度
            margin-right:200px;//核心点，抵消右元素的宽度
        }
        
    </style>
</head>
<body>
    <div class="container">
        <div class="left">
            左
        </div>
        <div class="right">
            右
        </div>
        <div class="middle">//注意此处中间元素一定在最后
            中间
        </div>
    </div>
</body>
</html>
```

![image-20201122203448854](CSS(0x01)-css常用知识点/image-20201122203448854.png)

### 3.3 层模型 position

 #####  3.3.1  **绝对定位**(position: absolute) 

*  参照定位的元素（父元素）需要加入position:relative或position: absolute; 最好是加入relative
*  参照定位的直接父元素如果没有设置relative或absolute，则会继续向上找，直到直到body标签
*  该布局是脱离文档流的，也是脱离文本流的， *对别的元素不会造成影响*
*  由于该布局脱离了文档流，不能参照父元素的尺寸，所以不能使用`width:*%`百分比来设置宽高

 如果想为元素设置层模型中的绝对定位，需要设置**position:absolute**(表示绝对定位)，这条语句的作用将元素**从文档流中拖出来**，然后使用left、right、top、bottom属性相对于其最接近的一个具有定位属性的父包含块进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于**浏览器窗口**。 

#####  3.3.2  **相对定位**(position: relative) 

* 该布局仍然是在正常文档流中的，即占有正常文档流的位置
* 只是相对于文档流中自己的位置有偏移  *对别的元素是有影响的*

 如果想为元素设置层模型中的相对定位，需要设置position:relative（表示相对定位），它通过left、right、top、bottom属性确定元素在**正常文档流中**的偏移位置。相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于**以前的位置移动，**移动的方向和幅度由left、right、top、bottom属性确定，偏移前的位置保留不动。 

#####  3.3.3  **固定定位**(position: fixed) 

* 该布局是脱离文档流的
* 该布局是相对于浏览器窗口定位的

 fixed：表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是视图（**屏幕内的网页窗口**）本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终位于浏览器窗口内视图的某个位置，不会受文档流动影响 

##### 3.3.4 静态定位(position: static)

按文档流默认情况布局

### 3.4 层叠循序 z-index

> [菜鸟教程z-index](https://www.runoob.com/cssref/pr-pos-z-index.html)

**注意：z-index 只作用于使用了定位元素(position:absolute, position:relative, or position:fixed)的容器**

## 4 响应式布局

#### 4.1单位

* vh（viewport height）: 视图高度  ----  vh的具体高度是根据设备而定  
  * 100vh 即整个视图的高度    ---- 及设备的整个视图界面的高度
* psd原稿的尺寸单位是：像素             1px = 2像素

#### 4.2  适配

##### 4.2.1 reset css

**`reset css`用于统一不同浏览器标签样式的默认设置**，将标签自带样式归零

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

#### 4.3 宽高自适应

##### 4.3.1 自适应屏宽高

```js
//用vh、vw来设置宽高
100vh: 100%的屏幕高
100vw： 100%的屏幕宽
```

##### 4.3.2 设置文档流固定高度后的剩余高度

* css3 中提供的`calc()`方法
* 应用场景：不知道设备的具体高度  要设置一个元素的高度为设备高度减去上导航和下导航高度之后的高度

```
.content{
	height: calc(100% - 98px)  //100%是设备的整个视图界面的高度（内容高度  要舍去padding margin）
}
```





####  4.4 屏幕大小适应

> 处理屏幕大小的方式主要有：
>
> * 隐藏：某个模块在pc端显示，在移动端隐藏
> * 折行：在pc端横向显示，在移动端竖向显示
> * 自适应空间：利用`flex：n`等方式让元素自适应大小，即只设置占的比例大小

1. `@media`匹配不同屏幕尺寸，设置相应尺寸下的效果

   * 例如匹配到屏幕宽度小于等于320px时，某个模块就不显示了

     ```css
      @media (max-width: 320px){
                 .left{
                     display: none;//不显示
                 }
             }
     ```

2. `rem`使不同屏幕大小自动缩放

   * 1rem = html的font-size的大小

   * 结合`@media`匹配不同尺寸，并设置`html的font-size大小`

3. `<meta content="width">`设置屏幕宽度

   * `<meta name="viewport" content="width=device-width, initial-scale=1.0">`表示设备屏幕宽度就是视图宽度
   * ` <meta name="viewport" content="width=320px">`表示页面按320px展示，若设备屏幕大于320px，则将元素整体放大铺满整个视图

**案例：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>responsive</title>
    <style>
        /*1rem就等于这里html字体大大小：1rem=20px  */
        html{
            font-size: 20px;
        }
        .container{
            margin:0 auto;
            max-width:800px;
            border:1px solid black;
        }
        .intro{
            display: inline-block;
            width:9rem;
            height:9rem;
            line-height: 9rem;
            text-align: center;
            border-radius: 4.5rem;
            border:1px solid red;
            margin:.3rem;
        }
        /* 屏幕宽度小于等于375px时 */
        @media (max-width: 375px){
            /*此时1rem=24px  */
            html{
                font-size:24px;
            }
        }
        /* 屏幕宽度小于等于320px时 */
        /* 设置相同的属性，越小的越放在下面，因为匹配时从上往下的
           例如310px满足上面的<=375px，就不会匹配到下面的<=320px */
        @media (max-width: 320px){
            /*此时1rem=20px  */
            html{
                font-size:20px;
            }
        }
        /* 屏幕宽度小于等于320px时 */
        @media (max-width: 640px){
            .intro{
                margin:.3rem auto;
                display: block;
            }
        }
        
    </style>
</head>
<body>
    <div class="container">
        <div class="intro">
            介绍1
        </div>
        <div class="intro">
            介绍2
        </div>
        <div class="intro">
            介绍3
        </div>
        <div class="intro">
            介绍4
        </div>
    </div>
</body>
</html>
```









#  四、效果属性

###  box-shadow

* 外部阴影本质是把原元素复制了一份且大小尺寸相同，
  * 外部阴影时
    * cpx不为0时有模糊效果，
    * dpx不为0是有放大和缩小的效果
    * a、bpx则是移动这个阴影的位置
  * 内部阴影时
    * cpx不为0时有模糊效果，
    * dpx>0px时，可以向内扩大阴影范围
    * a、bpx则是移动这个阴影的位置，超出原元素的部分隐藏
* 外部阴影时，当cpx为0 、dpx为正值时，可以实现边框的效果
  * **该方式实现的边框效果不会影响其他元素的布局，即边框不占像素**

```css
box-shadow: apx bpx cpx dpx rgba(0,0,0,.2);         //外部阴影
box-shadow: inset apx bpx cpx dpx rgba(0,0,0,.2);   //内部阴影
inset: 内部阴影
apx: 水平偏移量 正-右
bpx: 上下偏移量 正-下
cpx: 阴影程度  值越大  阴影越模糊范围越大
dpx: 阴影大小 
     外部阴影时：正值越大阴影面积越大   负值时阴影面积缩小
	 内部阴影时：正值时阴影向内放大     负值时无效

```

###  text-shadow

```css
text-shadow: apx bpx cpx rgba(128,128,128,.2)
apx: 水平偏移量 正-右
bpx: 上下偏移量 正-下
cpx: 阴影程度  值越大  阴影越模糊范围越大
```

###  border-radius

```css
border-radius: 10px 10px 10px 10px / 20px 20px 20px 20px;
10px: 是水平方向四个角的弯曲半径
20px: 是垂直方向四个角的弯曲半径
border-top-left-radius: 100px 50px;
100px: 是左上角水平方向角的弯曲半径
50px: 是左上角垂直方向角的弯曲半径
```



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