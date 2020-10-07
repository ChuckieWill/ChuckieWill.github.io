---
title: Javascript(0x05)-异步编程
date: 2020-08-05 10:39:21
tags:
- Javascript
- 异步编程
- 回调地狱
- promise
- async await
categories:
- [Javascript]
---



#  异步编程

## 1 promise

###  1.1 promise 的本质与用法

* promise是对象不是函数    new Promise
* promise的三个状态

```
pending  fulfilled  rejected
进行中     已成功      已失败

new Promise时 promise处于进行中状态  
```

* promise使用
  * resolve()将进行中状态转换成已成功状态   reject()将进行中状态转换成已失败状态   且状态转换后不可逆
  * 根据promise内部的异步操作的成功或失败来改变promise状态 --------------------1
  * 若promise内部异步操作成功或失败有数据返回则用resolve(res) 、reject(err)返回数据----------2
  * 若promise内部异步操作成功或失败没有数据返回则用resolve() 、reject()来控制promise状态的变化就可以了

```js
//1 创建
function(){
    return new Promise((resolve,reject)=>{
        //异步操作
        //例如
        wx.getSystemInfo({
            success:(res)=>{--------------------------------1
                resolve(res)  或者 resolve() ----------------2
            },
            fail:(err)=>{-----------------------------------1
                reject(err)   或者  reject()-----------------2
            }	
        })
    }) 
}


//2 使用  ----  在text()函数中调用上面封装的function()函数
//2.1 promise.then()的接收方式
text(){
	function.then(
        (res)=>{},--------------------成功的回调函数---->对应于resolve()------3
        (error)=>{}-------------------失败的回调函数---->对应于reject()-------3
    )
}

//2.2 async await的接收方式
async text(){
	//promise.then()的接收方式中--3--的结果（res或error）直接赋值给了这里的res
	const res = await function()
}
```

###  1.2 promise.all

```js
let p1 = new Promise( (resolve, reject) => {
	setTimeout( () => {
		console.log('p1')
		resolve('p1')//执行成功后返回的值‘p1’
	},2000)
}) 

let p2 = new Promise( (resolve, reject) => {
	setTimeout( () => {
    console.log('p2')
		resolve('p2')//执行成功后返回的值‘p2’
	},1000)
}) 

let p3 = new Promise( (resolve, reject) => {
	setTimeout( () => {
    console.log('p3')
		resolve('p3')//执行成功后返回的值‘p3’
	},3000)
}) 

Promise.all([p1,p2,p3]).then((res) => {
	sonsole.log('全部完成')   //只有p1,p2,p3都成功才会调用此处
	console.log(res)    //res:['p1','p2','p3'] 即resolve返回的值
}).catch((err) => {
	sonsole.log('失败')
	console,log(err)    // 即reject返回的值
})

.all的返回值res是一个数组  对应于 [p1,p2,p3]的返回值
```

###  1.3 promise.race

```js
let p1 = new Promise( (resolve, reject) => {
	setTimeout( () => {
		console.log('p1')
		resolve('p1')//执行成功后返回的值‘p1’
	},2000)
}) 

let p2 = new Promise( (resolve, reject) => {
	setTimeout( () => {
    console.log('p2')
		resolve('p2')//执行成功后返回的值‘p2’
	},1000)
}) 

let p3 = new Promise( (resolve, reject) => {
	setTimeout( () => {
    console.log('p3')
		resolve('p3')//执行成功后返回的值‘p3’
	},3000)
}) 

Promise.race([p1,p2,p3]).then((res) => {
	sonsole.log('完成')   //只要p1,p2,p3一个成功完成就会调用此处
	console,log(res)    //res 即resolve返回的值
}).catch((err) => {
	sonsole.log('失败')
	console,log(err)    //err 即reject返回的值
})
```

### 1.4 promise 规避回调地狱

* 回调地狱

```js
假设封装的model中有3个API接口（getApi1,getApi2,getApi3）,
且它们是链式调用getApi1-->getApi2--->getApi3

const model = model() //实例化
const promise = model.getApi1() //return回来的是promise对象
promise.then((res) => {---------------------调用api1
  console.log(res, 'api1')
  model.getApi2().then((res) => {---------------调用api2
    console.log(res, 'api2')
    model.getApi3().then((res) => {----------------调用api3
      console.log(res, 'api3')
    })
  })
})
```

* promise正确使用规避回调地狱
  * 前提：每个api调用都要是promise的，即每次api调用都要是返回的promise对象
  * 关键是：不在内部调用下一个api且处理该api,而是只在内部调用，再将调用return到外部，在外部再处理

```js
const model = model() //实例化
model.getApi1()-----------------------------调用api1 
  .then((res) => {
    console.log(res, 'api1')
    return model.getApi2()------------------调用api2 并return到外部
  })
  .then((res => {---------------------------在外部处理api2的调用
    console.log(res, 'api2')
    return model.getApi3()------------------调用api3 并return到外部
  })
  .then((res)=>{----------------------------在外部处理api3的调用
    console.log(res, 'api3')
  })

```

##  2 async await

###  2.1 async await的使用及前提

* 异步调用的终极处理办法      异步的一般使用场景（读写文件、操作数据库、发送HTTP请求）
* 前提：--------等待的异步函数返回的必须是一个promise对象-------
* 使用
  * 在函数头部用async
  * 在异步函数前面写await用于等待异步函数的完成

```
async text(){
	const pro = await function() //-------function()函数返回的必须是一个promise对象
}
```

###  2.2 async字段的理解

* async将一个函数的返回值强制包装成一个Promise对象

```js
async test(){
	return 'hahahhaha'
}

console.log(test())
打印出的是一个promise对象

test().then((res)=>{
	console.log(res)
})
打印出的是 'hahahhaha'
```

###  2.3 await字段的理解

* await 可以堵塞当前线程  等待异步调用完成
* await  是一个求值关键字  可求值promise  还可以求值表达式 
  1. 求值promise   相当于解析了promise对象  直接拿到了对象成功回调返回的数据

```js
text(){
	const pro = function() //-------function()函数返回的必须是一个promise对象
}
此时res是一个promise对象
需要用.then()方法拿到这个对象的成功数据
pro.then((res)=>{
	console.log(res)----------1
})


async text(){
	const pro = await function() //-------function()函数返回的必须是一个promise对象
}
此时的pro就是上面回调函数中的res  相当于直接拿到了数据
console.log(pro)---------------1

---------------1 打印的结果一样

```

​        2.  求值表达式

```js
async test(){
	const a = await 100*100
	console.loe(a)
}

打印结果为10000
```

##  3 回调写法、promise写法、async await写法的比较

```js
//promise方式
 userAuthorized1() {
    promisic(wx.getSetting)()
      .then(data => {
        if (data.authSetting['scope.userInfo']) {
          return promisic(wx.getUserInfo)()
        }
        return false
      })
      .then(data => {
        if (!data) return 
        this.setData({
          authorized: true,
          userInfo: data.userInfo
        })
      })
  },

//async await方式
//-------promisic(wx.getSetting)()函数内部是一个promise对象、promise内部返回的是resolve(res)、reject(err)的数据  这个数据可以直接在外部赋值接收 不用在.then()函数的回调函数中取到-------------
  async userAuthorized2(){
    const data = await promisic(wx.getSetting)()
    if (data.authSetting['scope.userInfo']) {
       const res =  await promisic(wx.getUserInfo)()
       const userInfo = res.userInfo
       this.setData({
         authorized: true,
         userInfo
       })
    }
  },

//回调函数方式
  userAuthorized() {
    wx.getSetting({
      success: data => {
        if (data.authSetting['scope.userInfo']) {
          wx.getUserInfo({
            success: data => {
              this.setData({
                authorized: true,
                userInfo: data.userInfo
              })
            }
          })
        }
      }
    })
  },
```



