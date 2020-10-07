---
title: Javascript(0x07)-JS中的原型链
date: 2020-10-03 16:2:21
tags:
- Javascript
- 原型链
categories:
- [Javascript]
---

#    JS中的原型链

##  一、 原型链概念

```js
//实例化
const obj = {}
const func = () => {}
const arr = []

//原型链
obj -> Object.prototype -> null
func -> Function.prototype -> Object.prototype -> null
arr -> Array.prototype -> Object.prototype -> null
```

**在`vscode`调试中可以查看原型链的类型**

* 通过`__proto__`属性查看

![image-20201003163002972](Javascript(0x07)-JS中的原型链/image-20201003163002972.png)

 ##  二、 原型链知识点

###  2.1 知识点1 

**知识点：**

如果A沿着原型链可以找到`B.prototype`, 那么`A instanceof B 为 true`

**解析：**

* A沿着原型链可以找到`B.prototype`,即原型链上有B对象
* 例如：`func -> Function.prototype -> Object.prototype -> null`
  * `func`原型链上有`Object.prototype`
  * `func instanceof Object 为 true`



###  2.2 知识点2

**知识点：**

如果A对象上没有找到x属性，那么会沿着A对象的原型链找x属性

**解析：**

```js
const obj = {}
Object.prototype.x = 'x'  //将属性x添加到Object上
const func = () => {}
Function.prototype.y = 'y'

//访问结果
func.x = 'x'  //func原型链上有Object
func.y = 'y'

obj.x = 'x'
obj.y = undefined //obj原型链上没有Function
```



##  三、常见面试题

###  3.1 问题1 

**`instanceof`的原理，并用代码实现**

**知识点：**如果A沿着原型链可以找到`B.prototype`, 那么`A instanceof B 为 true`

```js
//代码实现instanceof
const instanceOf = (A, B) => {
  let p = A 
  while(p){
    if(p === B.prototype){
      return true
    }
    p = p.__proto__ //指针后移
  }
  return false
}
```

