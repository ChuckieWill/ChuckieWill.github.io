---
title: Javascript(0x04)-js自定义工具类函数
date: 2020-06-03 10:50:15
tags:
- Javascript
- 时间格式化
- 防抖函数
categories:
- [Javascript]
---



#  1格式化时间

###  1.1 date->自定义格式

```js
//formatTime.js
//date：时间对象(传入时必须是Date对象)  fmt：格式化的格式（yyyy-MM-dd hh:mm:ss yy/MM/dd 等）
export function formatDate(date, fmt) {
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length));
  }
  let o = {
    'M+': date.getMonth() + 1,
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds()
  };
  for (let k in o) {
    if (new RegExp(`(${k})`).test(fmt)) {
      let str = o[k] + '';
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? str : padLeftZero(str));
    }
  }
  return fmt;
};

//补零操作 4-->04
function padLeftZero (str) {
  return ('00' + str).substr(str.length);
};
```

###   1.2 second->h​,m,​s

```js
//传入参数：秒
//将秒转换成相应的h,m,s返回
module.exports = {
  repairZero(num) {
    return num < 10 ? ("0" + num) : num
  },
  formatMs2Obj(total) {
    var h = this.repairZero(Math.floor(total / 3600))
    var m = this.repairZero(Math.floor((total - h * 3600) / 60))
    var s = this.repairZero(Math.floor(total - h * 3600 - m * 60))
    // s6结构h: h
    return {
      h,
      m,
      s
    }
  }
}
```

#  2获取不重复的文件名

```js
//index.js
export default getNoPrpetName(){
    return  Date.now() + '-' + Math.random() * 1000000 
}
```



# 3 防抖函数

**应用场景：**

* 防止页面频繁刷新
* 防止函数频繁调用

**定义：**

* 若调用的时间间隔小于delay则取消上一次延时；后续不再调用的时间间隔大于delay才执行需要防抖的函数--------1
* 需要防抖的函数------------2

```js
//防抖函数
//func:需要防抖的函数  delay：延迟执行的时间
debounce(func, delay=1000){
    let timer = null
    return function (...args){
          if(timer) clearTimeout(timer)     //--------1
          
          timer = setTimeout(() => {
            func.apply(this, args)         //---------2
          },delay)
    }
},
```

**使用：**

```js
//需要防抖的函数
text(){
	console.log('需要防抖的函数')
}

//使用防抖函数
const fangdou = deboumce(text， 300)   
//调用
fangdou()
```

#  4 字符串随机生成器

**定义：**

```js
//commen.js
const chars = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'];

const random = function generateMixed(n) {
  var res = "";
  for (var i = 0; i < n; i++) {
    var id = Math.ceil(Math.random() * 35);
    res += chars[id];
  }
  return res;
}

export {random}
```

**使用：**

```js
import {random} from '/commen.js'

const words = random(n)   //n：生成n位字符串   返回n位字符串
```

# 5 判断输入是否为数字

```js
//验证是否为数字
  isNumber(value) {         
    var patrn = /^(-)?\d+(\.\d+)?$/;
    if(value === ''){return true}
    if (patrn.exec(value) === null ) {
      return false
    } else {
      return true
    }
  }
```



