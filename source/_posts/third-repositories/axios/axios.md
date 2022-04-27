---
title: axios
date: 2020-02-12 20:45:50
tags:
- axios
categories:
- [third-repositories, axios]
---

#  一、axios基础

##  0 官网 

> [axios官网]( http://www.axios-js.com/zh-cn/ )
>
> *  方法使用查看官网
> * 具体应该场景的封装查看本笔记



##  1 安装

```
npm install axios --save
```

* 注意：项目名称不能与axios同名  否则不能成功安装

## 2 特性

- 从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

##  3 vue中使用axios

> 主要讲封装的内容

**目录**

* 在src目录下新建`utils/resques.js`

###  3.1 封装与使用

**封装**

```js
//utils/resques.js
import axios from 'axios'

export const post = (url, data = {}) => {
  return new Promise((resolve, reject) => {
    axios.post(url, data, {
      baseURL: 'https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa19/jd',  //设置基础地址
      headers: {
        'Content-Type': 'application/json'     //设置提交的数据格式位json
      }
    }).then(res => {
      console.log(res)
      resolve(res.data)
    }, err => {
      reject(err)
    })
  })
}

```

**使用**

```vue
<script>
import { post } from '../../utils/request'
export default {
  name: 'Login',
  setup () {
    const handleLogin = () => {
      post('/api/user/login', {
        username: 'username1',
        password: 'password1'
      }).then(() => {
        alert('成功')
      }).catch(() => {
        alert('失败')
      })
    }
  }
}
</script>
```

###  3.2 实例化axios

> [创建实例官网文档](http://www.axios-js.com/zh-cn/docs/#%E5%88%9B%E5%BB%BA%E5%AE%9E%E4%BE%8B)

* 设置baseURL
* 设置请求超时时间

```js
//utils/resques.js
import axios from 'axios'

const instance = axios.create({
  baseURL: 'https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa19/jd', //设置baseURL
  timeout: 10000  //设置请求超时时间
})

export const get = (url, params = {}) => {
  return new Promise((resolve, reject) => {
    instance.get(url, { params }).then(res => {
      console.log(res)
      resolve(res.data)
    }, err => {
      reject(err)
    })
  })
}

export const post = (url, data = {}) => {
  return new Promise((resolve, reject) => {
    instance.post(url, data, {
      headers: {
        'Content-Type': 'application/json'
      }
    }).then(res => {
      console.log(res)
      resolve(res.data)
    }, err => {
      reject(err)
    })
  })
}
```

###  3.3 带参url

####  3.3.1 get

* url格式

```js
/api/shop/:id/products?tab=all   // tab = 'all' 或 'seckill' 或 'fruit'  id = 页数
```

* 传递`:id`方式的参数
  * `${}` 方式传入


```js
const res = await get(`/api/shop/${shopId}/products`, {
      tab: currentTab.value
    })
```

* 传递`?params`方式的参数
  * 键值对的形式传入，键就是定义的键,例如tab


```js
const res = await get(`/api/shop/${shopId}/products`, {
      tab: currentTab.value
    })
```





1. get方法--------传递数据用params

```
import request from '@/utils/request'
//请求后端的基础地址
const baseURL = 'http://localhost:3000'

export function fetchList (params){
    return request({
        params, //页面调用时传入的参数  在此处再传给后端
        url:`${baseURL}/playlist/list`,//后端的`${域名}/路由/方法`
        method: 'get' //此处的请求方式get"or'post'根据后端controller下方法路由设置而定    
                      router.get<====>'get'  router.post<====>'post'
    })
}



//再在页面中调用  fetchList方法  需要传入参数----在params中
fetchList({
                skip:this.playlist.length,  //params中的参数
                limit: this.count           //params中的参数
            }).then((res) => {
                console.log(res)
            })
```

2. post方法-----------传递数据用data

```
//向数据库更新歌单列表某个歌单的信息
export function update (params){
    return request({
        data:{
            ...params
        }, //页面调用时传入的参数  在此处再传给后端
        url:`${baseURL}/playlist/updatePlaylist`,//后端的`${域名}/路由/方法`
        method: 'post' //此处的请求方式get"or'post'根据后端controller下方法路由设置而定  
                       router.get<====>'get'  router.post<====>'post'
    })
}
```

