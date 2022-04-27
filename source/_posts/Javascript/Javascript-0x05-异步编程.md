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
  * pending 状态，不会触发then和catch
  * resolved状态，会触发后续的then回调函数，不会触发catch
  * rejected状态，会触发后续的catch回调函数或者then的第二个回调函数
    * 当同时有then的第二个回调函数和catch回调函数时，且是promise内部错误时，按就近原则，调用then的第二个回调函数
    * 当同时有then的第二个回调函数和catch回调函数时，且不是promise内部错误时，例如网络中断，直接调用catch

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
	//promise.then()的接收方式中--3--的结果res直接赋值给了这里的res
	const res = await function()
}
```

* 直接获取已成功或已失败的promise

```js
// 直接返回一个 resolved 状态
Promise.resolve(100).then(res => {})
cosnt p = Promise.resolve(100)
p.then(res => {})
// 直接返回一个 rejected 状态
Promise.reject('some error').catch(err => {})
cosnt p = Promise.reject('some error')
p.catch(err => {})
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

###  1.5 then 和catch改变状态

* 状态变化会触发 then catch
  * pending 不会触发任何 then catch 回调
  * resolved状态，会触发后续的then回调函数，不会触发catch
  * rejected状态，会触发后续的catch回调函数，不会触发then
* then catch 会继续返回 Promise ，**此时可能会发生状态变化！！！**
  * then 正常返回resolved ，里面有报错则返回rejected, 若没有被触发则忽略
  * catch 正常返回resolved ，里面有报错则返回rejected，若没有被触发则忽略

```js
// 第一题
Promise.resolve().then(() => {//返回 resolved 状态的 promise
    console.log(1)
}).catch(() => {//不触发
    console.log(2)
}).then(() => {// 返回 resolved 状态的 promise
    console.log(3)
})
//打印结果 1 3

// 第二题
Promise.resolve().then(() => { // 返回 rejected 状态的 promise
    console.log(1)
    throw new Error('erro1')
}).catch(() => { // 返回 resolved 状态的 promise
    console.log(2)
}).then(() => { // 返回 resolved 状态的 promise
    console.log(3)
})
//打印结果1 2 3

// 第三题
Promise.resolve().then(() => { // 返回 rejected 状态的 promise
    console.log(1)
    throw new Error('erro1')
}).catch(() => { // 返回 resolved 状态的 promise
    console.log(2)
}).catch(() => { // 不触发
    console.log(3)
})
//打印结果1 2
```



##  2 async await

###  2.1 async await的使用及前提

* 异步调用的终极处理办法      异步的一般使用场景（读写文件、操作数据库、网络请求、定时器、DOM事件）
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
  * 若返回值本身就是一个Promise对象，则将这个Promise对象本身返回，不再包装

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

###  2.4 和 Promise 的关系

* then同await,只处理Promise的resolved状态，**不能处理rejected和pending状态**
  * then接收pending状态，then不会触发，后续回调函数不会执行
  * then接收rejected状态，then不会触发，且此时没有处理内部返回的错误，**会报错**
    * 除非有then的第二个回调函数，则可以处理rejected

- await 处理 Promise的resolved状态，**不能处理rejected和pending状态**

  * await 接收pending状态，await不会触发，后续回调函数不会执行
  * await 接收rejected状态，await不会触发，且此时没有处理内部返回的错误，**会报错**

  ```js
  !(async function () {
      const p1 = new Promise(() => {})
      await p1          //  await后面的代码相当于回调函数中的代码
      console.log('p1') // 不会执行 因为结果为pending状态 await不处理  相当于不会进入回调函数
  })()
  
  !(async function () {
      const p2 = Promise.resolve(100)
      const res = await p2
      console.log(res) // 100
  })()
  
  !(async function () {
      const res = await 100
      console.log(res) // 100
  })()
  
  !(async function () {
      const p3 = Promise.reject('some err')
      const res = await p3  //会报错 因为返回的错误 await不能处理   需要用try  catch处理这个错误
      console.log(res) // 不会执行 因为结果为rejected状态 await不处理  相当于不会进入回调函数
  })()
  ```

- catch处理 Promise 的rejected状态，**不能处理resolved和pending状态**

- try...catch同promise机制的catch 处理 Promise 的rejected状态，**不能处理resolved和pending状态**

  - resolved和pending状态不会触发try...catch
  
  ```js
  (async function () {
      const p4 = Promise.reject('some err')
      try {
          const res = await p4
          console.log(res)
      } catch (ex) {
          console.error(ex)
      }
  })()
  ```

###  2.5 await的本质是异步

* **只要遇到了 `await` ，后面的代码都相当于放在 callback 里。**

```js
async function async1 () {
  console.log('async1 start')
  await async2()  // **----先执行async2()  await再起作用----**
  //await的后面,都可以看做是callback里的内容，即异步
  //类似，event loop , setTimeout(cb1)
  //setTimeout(function() { console.log ( 'async1 end ') })
  //Promise.resolve().then(()=> { console.log( 'async1 end ')})
  console.log('async1 end') // 关键在这一步，它相当于放在 callback 中，最后执行
}

async function async2 () {
  console.log('async2')
}

console.log('script start')
async1()
console.log('script end')

//打印结果
script start
async1 start
async2
script end
async1 end



async function async1 () {
  console.log('async1 start') //2
  await async2()  
  console.log('async1 end') //5
  await async3()
  console.log('async1 end 2')  //7
   
}

async function async2 () {
  console.log('async2')  //3
}

async function async3 () {
  console.log('async3') //6
}

console.log('script start') //1
async1()
console.log('script end') //4
```



##  3 回调写法

> 回调写法很麻烦，每次都需要将回调函数传递给异步函数，这样才能实现堵塞结束后再执行下一个任务

* 网络请求函数（异步）

```js
//sCallback为回调函数
 test(sCallback){
    wx.request({
      url: 'classic/latest',
      success:(res)=>{
        sCallback(res)//将结果传给回调函数
        console.log(res)
      }
    })
  }
```

* 调用网络请求函数，并自定义回调函数内容

```js
  test((res)=>{
      console.log(res)
      this.setData({
        classic:res,
        likeCount:res.fav_nums,
        likeStatus:res.like_status
      })
    })
```

* 以上写法等同于以下写法，只是上面写法才能实现任务分离

```js
test(){
    wx.request({
      url: 'classic/latest',
      success:(res)=>{
        console.log(res)
        this.setData({
            classic:res,
            likeCount:res.fav_nums,
            likeStatus:res.like_status
        })
        console.log(res)
      }
    })
  }

test()
```



##  4 回调写法、promise写法、async await写法的比较

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



##  5 for...of 异步循环

* forEach((item) => {})    **同步执行**

* for(let i in list)     i取到的是list数组的索引值, i取到的是list对象的键   **同步执行**
* for(let item of list)   item取到的是list数组的元素， item取到的是list对象的值   **异步执行**

```js
// 定时算乘法
function multi(num) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(num * num)
        }, 1000)
    })
}

// 使用 forEach ，是 1s 之后打印出所有结果，即 3 个值是一起被计算出来的
 function test1 () {
     const nums = [1, 2, 3];
     nums.forEach(async x => {
         const res = await multi(x);
         console.log(res);
     })
 }
 test1();//隔1s后同时打印出1 4 9 

// 使用 for...of ，可以让计算挨个串行执行
async function test2 () {
    const nums = [1, 2, 3];
    for (let x of nums) {
        // 在 for...of 循环体的内部，遇到 await 会挨个串行计算
        const res = await multi(x) 
        console.log(res)
    }
}
test2() //隔1s后打印1 再隔1s后打印4  再隔1s后打印9
```

##  6 Promise中的then第二个参数和catch的区别

1. 当错误是promise内部错误时，会按就近原则处理即：

* 当Promise.then中有第二个回调函数时，执行then第二个函数

```js
let p=new Promise((resolve,reject)=>{
    throw new Error('出错了')
})
p.then(()=>{},err=>{
    console.log('reject'+err);
}).catch(err=>{
    console.log('catch'+err);
})
结果：rejectError: 出错了
```

* 当Promise.then中没有第二个回调函数时，执行catch

```js
let p=new Promise((resolve,reject)=>{
    throw new Error('出错了')
})
p.then(()=>{}).catch(err=>{
    console.log('catch'+err);
})
结果：catchError: 出错了
```

2. 当错误不是promise内部错误时，例如网络中断，则需要用catch来处理

* 即使有then的第二个参数也不会进入，会直接进入catch

3. 如果在then的第一个函数里抛出了异常，后面的catch能捕获到，而then的第二个函数捕获不到

```js
Promise.resolve('10')
.then(res => {
  console.log(res)
  console.log('resolve')
  throw new Error('hhhh')
},err => {
  console.log(err)
  console.log('err')
}).catch(erro =>{
  console.log(erro)
  console.log('reject')
})

//打印结果
10
resolve
Error: hhhh
    at F:\code\test-font\test.js:5:9
reject
```

