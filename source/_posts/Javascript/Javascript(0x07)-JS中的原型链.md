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

##  一、原型

```js
// 父类
class People {
    constructor(name) {
        this.name = name
    }
    eat() {
        console.log(`${this.name} eat something`)
    }
}

// 子类
class Student extends People {
    constructor(name, number) {
        super(name)
        this.number = number
    }
    sayHi() {
        console.log(`姓名 ${this.name} 学号 ${this.number}`)
    }
}

// 实例
const xialuo = new Student('夏洛', 100)


// class实际上是函数，可见是语法糖
typeof People //  'function'
typeof Student // 'function'
typeof xialuo  // 'object'
//隐式原型和显示原型
console.log( xialuo.__proto__) //隐式原型
console.log( Student.prototype) //显示原型
console.log( xialuo.__proto__ === Student.prototype)  // true
```

![image-20210313211343029](Javascript(0x07)-JS中的原型链/image-20210313211343029.png)

![image-20210117162535083](Javascript(0x07)-JS中的原型链/image-20210117162535083.png)

**原型关系：**

* 每个**class**都有显示原型`prototype`
* 每个**实例**都有隐式原型`__proto__`
* 实例的`__proto__` 指向对应 class的`prototype`

**原型的执行规则：**

* 获取属性xialuo.name或执行方法xialuo.sayhi()时
* 先在自身属性和方法寻找
* 如果找不到则自动去`__proto__ `中查找





##  二、 原型链

```js
// 父类
class People {
    constructor(name) {
        this.name = name
    }
    eat() {
        console.log(`${this.name} eat something`)
    }
}

// 子类
class Student extends People {
    constructor(name, number) {
        super(name)
        this.number = number
    }
    sayHi() {
        console.log(`姓名 ${this.name} 学号 ${this.number}`)
    }
}

// 实例
const xialuo = new Student('夏洛', 100)

console.log( Student.prototype.__proto___)
console.log( People.prototype )
console.log( People.prototype === Student.prototype.__proto__) // true
```

![image-20210313211743939](Javascript(0x07)-JS中的原型链/image-20210313211743939.png)

![image-20210117163632152](Javascript(0x07)-JS中的原型链/image-20210117163632152.png)



![image-20210117164136590](Javascript(0x07)-JS中的原型链/image-20210117164136590.png)



**在`vscode`调试中可以查看原型链的类型**

* 通过`__proto__`属性查看

![image-20201003163002972](Javascript(0x07)-JS中的原型链/image-20201003163002972.png)

 ##  三、 原型链知识点

###  3.1 知识点1 

**知识点：**

如果A沿着原型链可以找到`B.prototype`, 那么`A instanceof B 为 true`

```js
xialuo是Student类实例化的， Student类的父类是People
xialuo instanceof Student / / true 
xialuo instanceof People , / true
xialuo instanceof Object , / true
[] instanceof Array // true 
[] instanceof Object // true
{} instanceof Object // true
```

**解析：**

* A沿着原型链可以找到`B.prototype`,即原型链上有B对象
* 例如：`func.__proto__ -> Function.prototype, Function.prototype.__proto__ -> Object.prototype, Object.prototype.__proto__ -> null`
  * `func`原型链上有`Object.prototype`
  * `func instanceof Object 为 true`



###  3.2 知识点2

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



##  四、常见面试题

#### 4.1 问题1 

**`instanceof`的原理，并用代码实现**

**知识点：**如果A沿着原型链可以找到`B.prototype`, 那么`A instanceof B 为 true`

```js
//代码实现instanceof
const instanceOf = (A, B) => {
  let p = A 
  while(p){
    if(p.__proto__ === B.prototype){
      return true
    }
    p = p.__proto__ //指针后移
  }
  return false
}
```

