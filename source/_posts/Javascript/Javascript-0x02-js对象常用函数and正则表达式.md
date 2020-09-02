---
title: Javascript(0x02)-js对象常用函数and正则表达式
date: 2020-04-04 10:36:47
tags:
- Javascript
categories:
- [Javascript]
---

#  一、js对象常用函数

##  String对象

* toString()：转换为字符串   

* split('.')：以`.`分割

  * 返回值：数组

* substring（a,b）：字符串截取  截取索引a和b之间的部分

  * 返回值：字符串

* parseFloat（）：转化为浮点数

* parseInt( ): 转化为int类型数 

* trim(): 去除字符串中的空格

  ```js
  let content = ' hdfak  hfkahd'
  content.trim()
  返回值： 'hdfakhfkahd'
  ```

  

##  Math对象

* Math.random(): 获取随机数  范围为：[0,1)
* Math.floor(   ): 向下取整
* Math.ceil(    ):向上取整

##  Array对象 

* reduce()

  * > [详细教程]( https://www.jianshu.com/p/e375ba1cfc47 )

  * 该函数相当于遍历数组，且可以保存每次操作的结果，提供给下次操作使用

  * 参数：函数

    ```js
    reduce((pre, cur, index, arr) => {
        
    }, Initial)
    //initial: 传入的初始值即pre的初始值
    ```

    * 函数四个参数含义

    ```js
    1、previousValue （上一次调用回调返回的值，或者是提供的初始值（initialValue））
    2、currentValue （数组中当前被处理的元素）
    3、index （当前元素在数组中的索引）
    4、array （调用 reduce 的数组）
    ```

  * 案例

  ```js
  var arr = [1, 2, 3, 4];
  var sum = arr.reduce(function(prev, cur, index, arr) {
      console.log(prev, cur, index);
      return prev + cur;//遍历数组的循环体
  }, 0)//这里的0是传入的初始值
  console.log(arr, sum);
  ```

* forEach()

  * 遍历数组
  * 参数：函数
  * 案例

  ```js
  line.forEach((item) => {              //item 为 line数组中的元素   forEach方法
  	let time = item.match()
  })
  ```

* unshift() ： 加在数组头

* push() :  加在结尾

* splice()： 根据索性在数组中删除或添加元素

  * 语法： *array*.splice(*index*,*howmany*,*item1*,.....,*itemX*) 
  * 参数： 
    * index: 必填。规定从何处添加/删除元素。 该参数是开始插入和（或）删除的数组元素的下标，必须是数字。
    * 可选。规定应该删除多少元素。必须是数字，但可以是 "0"。
      如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。 
    * 可选。要添加到数组的新元素
  * 返回值： array------返回删除部分的数组   而不是删除之后的 数组



##  Number对象

* toFixed(n): 保留小数点后n位

  ```js
  (58 * 1.6).toFixed(2)
  ```

## Date对象

```js
//返回值类型为: "2020-02-17T09:58:45.123Z" 
createTime : new Date()  

//getTime()将时间转化为秒  返回值为毫秒  从1970年1月1日开始算
const nowTime = new Date().getTime() 
```

* 不同时间类型的转换

```
//时间戳-----> Date（）对象
//时间戳
const createdTime = 1535329958   
 //将时间戳转化成时间对象  ----- 注意*1000 --- 时间戳单位是秒  时间对象单位是毫秒
const date = new Date(createdTime * 10000)  
```

```js
//readObj.createTime为字符串 : "2020-02-17T09:58:45.123Z"  将字符串类型时间转化以毫秒为单位的时间
const createTime = new Date(readObj.createTime).getTime()
```



##  JSON

* JSON.stringify(): 将对象转成字符串

* JSON.parse(): 将字符串转成对象





##  定时器

* setTimeout()   执行一次

```
setTimeout(() => {
	//console.log(1)
},1000)              //定时1秒。1秒后输出1     
```

* setInterval（） 执行多次

```js
setInterval(async () => {
    await updateAccessToken()
},1000)  //以“1000ms”为时间间隔一直执行
```



#  二、正则表达式

> **[文字版教程](https://www.runoob.com/regexp/regexp-tutorial.html )**
>
> **[视频教程]( https://www.bilibili.com/video/av80214387)**

