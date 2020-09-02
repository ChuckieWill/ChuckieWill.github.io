---
title: wxapplet(0x01)-小程序云开发
date: 2020-03-28 10:46:17
tags:
- wxapplet
categories:
- [wxapplet]
---

#  云开发

* 云开发包括：**云函数**、云数据库、云存储、云调用
* **小程序端和云函数端**云开发可参考以下微信官方文档： 
  * [云开发-开发指引]( https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/ )
  * [云开发-SDK文档]( https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-sdk-api/Cloud.init.html )
* **第三方**调用云函数、操作云数据库、云存储、云调用
* 

##   云函数

* 云函数是基于`node`环境的
* 使用第三方库时需要导入
* **每个云函数是独立的，需要使用第三方库时都要单独导入**
  * 例如A云函数导入m库,则m库只对A云函数起作用
  * 其他云函数需要使用m库，则需要自己再导入

###  1 云函数使用第三方库流程

* 安装：打开云函数的终端命令方式安装
  * **注意：**打开需要的云函数的终端，每个云函数是独立的

1. 安装`node`环境
2. 安装需要的第三方库
3. 在`js`文件中导入第三方库
4. 完成以上步骤即可在云函数`js`文件中使用第三方库了

###  2 云函数定时触发

* 使用场景： 云函数需要定时触发，向后端获取数据以刷新前端数据

在需要添加触发器的云函数目录下新建文件 `config.json`，格式如下：

```json
{
  // triggers 字段是触发器数组，目前仅支持一个触发器，即数组只能填写一个，不可添加多个
  "triggers": [
    {
      // name: 触发器的名字
      "name": "myTrigger",
      // type: 触发器类型，目前仅支持 timer (即 定时触发器)
      "type": "timer",
      // config: 触发器配置，在定时触发器下，config 格式为 cron 表达式
      "config": "0 0 2 1 * * *"
    }
  ]
}
```

* 相关规则可查看微信官方文档：[云函数定时触发器]( https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/functions/triggers.html)
  * 微信官方文档位置：云开发----开发指引----云函数-----定时触发器





##   云数据库

###  归纳

####  1  数据库中1对N的3中设计方式

1. N属于[0, 几十]时  存储于集合A中  N以数组的形式作为一个属性存储在每条记录中 
2. N属于[几十，几百]时   集合A 中存储1  集合B中存储N   同时集合A中用一个数组存储集合B中与之相关的记录的_id
3. N属于[几千，几万]时  集合A 中存储1  集合B中存储N   同时集合B存储它关联的集合A中的记录的_id

#### 2 查询条数权限

* 小程序端  20条
* 服务端(云函数)   100条
* HTTP API        10条(默认 若设置了skip  limit 则按limit来定)

#### 3  where() 和 doc() 的区别

* where()可以取到集合中所有符合条件的记录
* doc()只能取到集合中唯一标识的一条记录



###  0 note

####  0.1 小程序端与服务端初始化的区别

* 小程序端

```js
const db = wx.cloud.database()
```

* 云函数端

```js
const cloud = require('wx-server-sdk')
cloud.init({
  env: cloud.DYNAMIC_CURRENT_ENV
})
const db = cloud.database()
```

####  0.2 注意事项

* 在小程序端插入数据库时 数据中会自动包含openId   在服务端插入数据库不自带openid



###  1 Database

####  1.1 ServerDate()获取服务端时间

* 支持端：小程序、云函数

```
createTime: db.serverDate(), // 服务端的时间   ----比获取用户设备时间更准确

返回值：2020-02-09T06:28:42.065Z
Sun Feb 09 2020 14:28:42 GMT+0800 (中国标准时间)
```

####  1.2 ReExp实现模糊查询

```js
//案例1
w = {
    content: new db.RegExp({   //content 为集合中的属性名称
      regexp: keyword,         //查询关键词  由keyword传入
      options: 'i'
    })
}
    
blogCollection.where(w).get()

//案例2
db.collection('todos').where({
  description: new db.RegExp({  // description为集合中的属性名称
    regexp: 'miniprogram',
    options: 'i',
  })
})
```





###  2 Collection

####  2.1 查询条件

#####  orderBy排序查询

* `orderBy()`用于对从云数据库中查询到的数据进行排序
* orderBy(fieldName: string, order: string)
  * fieldNme:   用于定义需要排序的字段 
  * order: asc（正序） 、   desc（逆序）

* 案例

```js
const db = wx.cloud.database()
db.collection('todos').orderBy('progress', 'asc')
  .get()
  .then(console.log)
  .catch(console.error)
```

* 微信官方文档： [排序查询orderBy]( https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-client-api/database/collection.orderBy.html )

####  2.2 请求

#####  count()获取集合记录条数

```js
const db = wx.cloud.database()
const blogCollection = db.collection('blog')
//blogCollection集合  count()方法返回记录条数的对象
const countResult = await blogCollection.count()  
//对象中的total属性记录了集合中记录的条数
const total = countResult.total
```





##  云存储

###  1 上传文件uploadFile(）

1. 小程序端uploadFile(）

* API： wx.cloud.uploadFile()

  注意： 此函数一次只能上传一个文件  若有多个文件  则要循环上传

  cloudPath  ： 云存储路径    //  “'blog/'”: 为云存储中存储这一类文件的文件夹

```js
for(let i =0 ,len = this.data.images.length;i<len;i++){
      let item = this.data.images[i]
      //取到文件扩展名
      let suffix = /\.\w+$/.exec(item)[0]
      wx.cloud.uploadFile({
      //  “'blog/'”: 为云存储中存储这一类文件的文件夹
        cloudPath: 'blog/' + Date.now() + '-' + Math.random() * 1000000 + suffix,
        filePath: item ,=======================================================不同
        success: (res) => {
          console.log(res)
        },
        fail: (err) => {
          console.error(err)
        }
      })
    }
```

2. 云函数端uploadFile(）

* API ： cloud.uploadFile()

  ```js
  await cloud.uploadFile({
      cloudPath: 'demo.jpg',
      fileContent: fileStream,================================================不同
    })
  ```




##  第三方HTTP请求

###  1 获取接口调用凭证 access_token 

* 微信官方文档： [getAccessToken]( https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/access-token/auth.getAccessToken.html )

* getAccessToken.js
  * 参数：无
  * 返回值：`access_token`
  * 第一次调用后会在同文件夹下生产`access_token.json`文件，用于存储`access_token`
  * 因为`access_token`有效期只有2小时，所以该函数调用后会以1小时55分钟为间隔循环执行
  * 使用前提
    * node环境
    * 网络请求依赖`request-promise`  可更换

```js
const rp = require('request-promise')
const APPID = 	'wx4585ed7abca6e338'
const APPSECRET = 'edf80ecdf2d5335e0ac8c41b52db06ba'
const URL = `https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=${APPID}&secret=${APPSECRET}`
const fs = require('fs')
const path = require('path')
const fileName = path.resolve(__dirname,'./access_token.json')
//console.log(fileName)

// -------------------------------定义函数----------------------------
//更新AccessToken函数
const updateAccessToken = async () => {
    const resStr = await rp(URL)
    const res = JSON.parse(resStr)
    //console.log(res)
    //写文件
    if(res.access_token){
        fs.writeFileSync(fileName, JSON.stringify({
            access_token:res.access_token,
            createTime: new Date()
        }))
    }else{
        //再次调用获取access_token的函数
        updateAccessToken()
    }
}

//从文件中读取access_token
const getAccessToken = async () => {
    try{
        const readRes = fs.readFileSync(fileName,'utf8')
        const readObj = JSON.parse(readRes)
        //console.log(readObj)
        const createTime = new Date(readObj.createTime).getTime()
        const nowTime = new Date().getTime() 
        if((nowTime - createTime) / 1000 / 60 / 60 >= 2){
            //时间超过了2小时则重新更新并获取
            await updateAccessToken()
            await getAccessToken()
        }
        return readObj.access_token
    }catch(error){
        //没获取则更新access_token
        await updateAccessToken()
        await getAccessToken()
    }
    
}

// -------------------------------调用函数----------------------------
//每两小时就更新一次  并且提前5分钟（官方建议）
setInterval(async () => {
    await updateAccessToken()
},(7200 - 300)*1000)
//updateAccessToken()
//console.log(getAccessToken())  
//将函数抛出
module.exports = getAccessToken
```

###  2 调用云函数

* 微信官方文档： [调用云函数](  https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-http-api/functions/invokeCloudFunction.html )
* 触发云函数的好处：请求或提交数据的方法在云函数中已经封装好了，直接调用就可以，不用自己再从头写请求或提交数据的方法
* 注意： 触发云函数是`POST`请求

* callCloudFn.js
  * 返回值：**Promised对象**
  * 参数：
    * env: 云环境ID
    * fnName: 云函数名称
    * params: 云函数需要的数据
  * 使用前提
    * node环境
    * 网络请求依赖`request-promise`  可更换
    * 导入`getAccessToken`用于获取`access-token`

```js
const rp = require('request-promise')
//获取access_token
const getAccessToken = require('./getAccessToken.js')

//fnName:云函数名称  params:云函数需要的数据
const callCloudFn = async(env,fnName,params)=>{
    //获取access_token
    const access_token = await getAccessToken()
    //触发云函数HTTP　API
    const url = `https://api.weixin.qq.com/tcb/invokecloudfunction?access_token=${access_token}&env=${env}&name=${fnName}`
    var options = {
        method: 'POST',
        uri: url,
        body: {
            ...params
        },
        json: true // Automatically stringifies the body to JSON
    };
    
    return await rp(options)
        .then( (res) => {
           //console.log(res)
           return res
        })
        .catch(function (err) {
            // POST failed...
        });
}

module.exports = callCloudFn
```

###  3 调用云数据库

* 微信官方文档： [调用云数据库](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-http-api/database/#数据库) 
* 注意： 触发云函数是`POST`请求

* callCloudDB.js
  * 返回值：**Promised对象**
  * 参数：
    * env: 云环境ID
    * fnName: 处理数据库的方式（13种） [官方文档查看13种方式](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-http-api/database/#数据库)
    * query: 查询指令
  * 使用前提
    * node环境
    * 网络请求依赖`request-promise`  可更换
    * 导入`getAccessToken`用于获取`access-token

```js
const rp = require('request-promise')
//获取access_token
const getAccessToken = require('./getAccessToken.js')

//fnName : 处理数据库的方式（13种）//query:查询指令
const callCloudDB = async(env,fnName,query = {})=>{
    //获取access_token
    const access_token = await getAccessToken()
    //触发云函数HTTP　API
    const url = `https://api.weixin.qq.com/tcb/${fnName}?access_token=${access_token}`
    var options = {
        method: 'POST',
        uri: url,
        body: {
            query,
            env
        },
        json: true // Automatically stringifies the body to JSON
    };
    
    return await rp(options)
        .then( (res) => {
           //console.log(res)
           return res
        })
        .catch(function (err) {
            // POST failed...
        });
}

module.exports = callCloudDB
```

使用：

```js
router.get('/getById',async(ctx,next)=>{
    const query = `db.collection('playlist').doc('${ctx.request.query.id}').get()`
    const res = await callCloudDB(ctx.state.env, 'databasequery',query)
    ctx.body = {
        code: 20000 ,//vue-admin-template前端模板要求的
        data: JSON.parse(res.data)
    }
})
```

###  4 调用云存储

* 微信官方文档： [调用云存储]( https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-http-api/storage/ ) 

* 注意

  *  触发云函数是`POST`请求
  * 每次上传文件返回的云存储链接（could:\\格式的）要保存到云数据中
  * 每次获取下载文件链接需要先获取云存储链接（could:\\格式的）可以去云数据库获取

* callCloudStorage.js

  * 使用前提
    * node环境
    * 网络请求依赖`request-promise`  可更换
    * 导入`getAccessToken`用于获取`access-token

* **1 获取文件下载链接download（）**

  * 将云存储链（cloud://A.png）接转成网页链接（https://A.png）

  * 参数

    * env: 云环境ID
    * fileList: 云文件列表(array)   -----  **需要特殊的格式**
      * PS: 云存储链接可以先通过HTTP云数据库调用获取（前提：云储存链接保存在数据库）

    ```js
    "file_list": [
    		{
    			"fileid":"cloud://test2-4a89da.7465-test2-4a89da/A.png",
    			"max_age":7200
    		}
    		]
    ```

  * 返回值：**Promised对象**

    * 其中`download_url即为文件的网页链接

    ```js
    res:{
        "errcode": 0,
        "errmsg": "ok",
        "file_list": [
            {
                "fileid": "cloud://test2-4a89da.7465-test2-4a89da/A.png",
                "download_url": "https://7465-test2-4a89da-1258717764.tcb.qcloud.la/A.png",
                "status": 0,
                "errmsg": "ok"
            }
        ]
    }
    ```

* **2 获取文件上传链接update（）**

  * 参数
    * ctx: 上下文，从里面需要获取：1 云环境ID、2上传文件的文件对象
      * **这里的耦合度较高，使用时需要再重构一下**
  * 返回值：云存储路径:（cloud://....）
  * 注意： 此函数一次只能上传一个文件
  * 向云存储上传文件流程

1. 发送网络请求，传入云存储路径，获取云储存上传链接及相关信息

```JS
//云存储路径：云存储文件夹名+文件名
//‘0swiper’是云存储文件夹名
const path = `0swiper/${Date.now()}-${Math.random()*1000000}-${file.name}`
```

```js
//请求返回值
{
    "errcode": 0,
    "errmsg": "ok",
    "url": "https://cos.ap-shanghai.myqcloud.com/7465-test2-4a89da-1258717764/testupload",
    "token": "Cukha70zkXI...",
    "authorization": "q-sign....",
    "file_id": "cloud://test2-4a89da.7465-test2-4a89da-1258717764/testupload",
    "cos_file_id": "HDze32/qZEN...."
}
```

2. 根据请求回的信息向上传链接发送网络请求上传文件

* **3 删除文件**

  * 参数
    * env: 云环境ID
    * fileIdList: 云存储链接列表(array)   
      * 云存储链接（cloud://格式）
      * 传入的fileIdList是数组，所以可以一次删除多个文件
  * 返回值: 返回值为一个对象，其中包括删除的文件的云存储链接

  

```js
//callCloudStorage.js
const rp = require('request-promise')
//获取access_token
const getAccessToken = require('./getAccessToken.js')
const fs = require('fs')


const cloudStorage = {
    //---------------------------1 获取文件下载链接--------------------------------------
    async download(env,fileList) {
    //获取access_token
    const access_token = await getAccessToken()
    //触发云函数HTTP　API
    const url = `https://api.weixin.qq.com/tcb/batchdownloadfile?access_token=${access_token}`
    var options = {
        method: 'POST',
        uri: url,
        body: {
            env,
            file_list: fileList
        },
        json: true // Automatically stringifies the body to JSON
    };
    
    return await rp(options)
        .then( (res) => {
           //console.log(res)
           return res
        })
        .catch(function (err) {
            // POST failed...
            console.log(err)
        });
    },
	//---------------------------2 获取文件上传链接--------------------------------------
    async update(ctx){
        //2.1 获取文件上传链接
        const file = ctx.request.files.file
        //自定义文件路径，‘0swiper’是云存储文件夹名
        const path = `0swiper/${Date.now()}-${Math.random()*1000000}-${file.name}`
        //获取access_token
        const access_token = await getAccessToken()
        //触发云函数HTTP　API
        const url = `https://api.weixin.qq.com/tcb/uploadfile?access_token=${access_token}`
        var options = {
            method: 'POST',
            uri: url,
            body: {
                path,
                env: ctx.state.env
            },
            json: true // Automatically stringifies the body to JSON
        };
        const info = await rp(options)
            .then( (res) => {
                return res
            })
            .catch(function (err) {
                console.log(err)
            });

        //2.2 上传文件
        const params = {
            method: 'post',
            headers: {
                'content-type': 'multipart/form-data'
            },
            uri: info.url,
            formData:{
                key:path,
                Signature:info.authorization,
                'x-cos-security-token':info.token,
                'x-cos-meta-fileid':info.cos_file_id,
                file:fs.createReadStream(file.path)  //读取本地要上传的文件并转化成二进制
            }
        }
        await rp(params)
        return info.file_id
    },
	
    //----------------------------- 3 删除文件 --------------------------------------
    //参数 fileIdList : 文件ID （cloud://格式）
    async delete(env,fileIdList){
        //获取access_token
        const access_token = await getAccessToken()
        //触发云函数HTTP　API
        const url = `https://api.weixin.qq.com/tcb/batchdeletefile?access_token=${access_token}`
        const options = {
            method: 'POST',
            uri: url,
            body: {
                env，
                fileid_list: fileIdList
            },
            json: true // Automatically stringifies the body to JSON
        };
        return await rp(options)
            .then( (res) => {
            //console.log(res)
                return res
            })
            .catch(function (err) {
                // POST failed...
                console.log(err)
            });
    }
}

module.exports = cloudStorage
```

