---
title: Javascript(0x08)-JS中的JSON序列化
date: 2020-11-17 10:44:17
tags:
- Javascript
- JSON序列化
categories:
- [Javascript]
---

# JS中JSON序列化

## 1 理解`toJSON()`

> 对象的`tJSON()`函数会决定对象返回的结果
>
> 注意：配合JSON.stringify()使用才有效
>
> * JSON.stringify()函数用于实现JSON序列化
> * 序列化就是将对象转化为字符串
> * JSON中都是双引号，没有单引号

* 不使用`toJSON()`

```js
const test = {
  a: 100,
  b : 'chuckie',
  c: [1,2,3,4],
  d: {
    g: 'good',
    e : {
      f: 'node.js'
    }
  }
}
console.log(test)
console.log(JSON.stringify(test))//转化为字符串

//打印结果
{
  a: 100,
  b: 'chuckie',
  c: [ 1, 2, 3, 4 ],
  d: { g: 'good', e: { f: 'node.js' } }
}
{"a":100,"b":"chuckie","c":[1,2,3,4],"d":{"g":"good","e":{"f":"node.js"}}}
```

* 使用`toJSON()`

```js
const test = {
  a: 100,
  b : 'chuckie',
  c: [1,2,3,4],
  d: {
    g: 'good',
    e : {
      f: 'node.js'
    }
  },
  toJSON(){
    return {
      a: 100,
      b : 'chuckie',
    }
  }
}
console.log(test)
console.log(JSON.stringify(test))//转化为字符串


//打印结果
{
  a: 100,
  b: 'chuckie',
  c: [ 1, 2, 3, 4 ],
  d: { g: 'good', e: { f: 'node.js' } },
  toJSON: [Function: toJSON]
}
{"a":100,"b":"chuckie"}//转化为字符串输出的结果 所以需要用JSON.stringify转化为字符串，才能使toJSON()有效
```

##  2 class对象使用toJSON()整理返回结果

> 应用场景： 需要在返回的对象中删除少数几个属性，保留大多数属性

* 主要步骤：

  * 将`toJSON()`挂载到类的原型链上

    ```js
    Model.prototype.toJSON = function(){}
    ```

  * 拿到对象上的属性

    ```js
    let data = clone(this)
    ```

  * 对属性进行相应的移除

    * 全局移除，即只要是实例化该类的对象，这些属性都会被移除

      ```js
      //移除自定义删除字段的字段,因为自定义移除属性时会添加这个属性，但是这个属性是和业务无关的，所以在返回时需要移除
      unset(data, 'exclude')
      //以下是结合具体业务设置的的全局移除的属性
      unset(data,'a')
      unset(data,'d.e.f')
      ```

    * 实例化对象自定义移除需要移除的属性

      ```js
        // exclude 模型上要删除的字段 提供模型实例自定义需要删除的字段
        //lodash-isArray,判断是否为数组
        if(isArray(this.exclude)){
          //遍历数组
          this.exclude.forEach((value)=>{
            //移除需要自定义需要删除的字段
            unset(data,value)
          })
        }
      ```

  * 实例化对象自定义需要移除的属性：使用

    ```js
    const model = new Model()
    model.exclude = ['b']//实例化对象通过exclude设置自定义需要移除的字
    ```

* 完整代码理解

  ```js
  const {unset, clone, isArray} = require( 'lodash ' )
  
  class Model {
    constructor(){
      this.a = 100,
      this.b = 'chuckie',
      this.d = {
        g: 'good',
        e : {
          f: 'node.js'
        }
      }
    }
  }
  
  //JSON序列化
  Model.prototype.toJSON = function(){
    //浅拷贝 this可以拿到模型上所有属性
    let data = clone(this)
    //lodash-unset,移除对象属性,全局移除，所有实例化Model类的对象以下设置的属性都会被移除
    unset(data, 'exclude')//移除自定义删除字段的字段
    //以下是结合具体业务设置的的全局移除的属性
    unset(data,'a')
    unset(data,'d.e.f')
  
    // exclude 模型上要删除的字段 提供模型实例自定义需要删除的字段
    //lodash-isArray,判断是否为数组
    if(isArray(this.exclude)){
      //遍历数组
      this.exclude.forEach((value)=>{
        //移除需要自定义需要删除的字段
        unset(data,value)
      })
    }
    return data
  }
  
  const model = new Model()
  model.exclude = ['b']//实例化对象通过exclude设置自定义需要移除的字段
  console.log(model)
  console.log(JSON.stringify(model))//转化为字符串
  
  //打印结果
  Model {
    a: 100,
    b: 'chuckie',
    d: { g: 'good', e: { f: 'node.js' } },
    exclude: [ 'b' ]//Model类toJSON()中全局移除了
  }
{"d":{"g":"good","e":{}}} //a、 d.e.f是类中定义的全局移除掉的， b是实例化model对象自定义移除的
  ```
  
  

##  3 遍历JSON

```js
const json = {
  a : { b : { c : 1}},
  d : [110 , 112]
}

//root: json对象， path：访问节点的路径
const dfs = (root, path) => {
  console.log(root,'------',path)
  Object.keys(root).forEach(item => {
    dfs(root[item], path.concat(item))//concat是数组的链接函数
  })
}

//json对象，[]数组
dfs(json, [])



//打印结果
{ a: { b: { c: 1 } }, d: [ 110, 112 ] } ------ []
{ b: { c: 1 } } ------ [ 'a' ]
{ c: 1 } ------ [ 'a', 'b' ]
1 ------ [ 'a', 'b', 'c' ]
[ 110, 112 ] ------ [ 'd' ]
110 ------ [ 'd', '0' ]
112 ------ [ 'd', '1' ]
```

