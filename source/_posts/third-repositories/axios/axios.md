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

###  3.2 配置默认值

> [配置默认值官网文档](http://www.axios-js.com/zh-cn/docs/#%E9%85%8D%E7%BD%AE%E9%BB%98%E8%AE%A4%E5%80%BC)

* 全局默认配置

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';

// 若要访问: https://api.example.com/colums  因为已经设置的了默认baseURL，所以访问时只url只用传入path
axios.get('/colums').then((res) => {....})
```

* 实例化自定义配置

```js
// Set config defaults when creating the instance
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// Alter defaults after instance has been created
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;

```



###  3.3 拦截器

> [拦截器官网](http://www.axios-js.com/zh-cn/docs/#%E6%8B%A6%E6%88%AA%E5%99%A8)

* 分为请求拦截器和相应拦截器

```js
// 使用场景案例1
// 访问地址：http://apis.imooc.com/api/columns?icode=C6A6C4086133360B
// 每个url访问都要加上icode的身份码，很麻烦， 则可以利用请求拦截器，将请求的url添加上icode

axios.defaults.baseURL = 'http://apis.imooc.com/api/'
axios.interceptors.request.use( config =>{    // 请求拦截器
    // 将icode添加到get请求的参数中
	config.params = { ... config.params, icode: 'C6A6C4086133360B'}  //... config.params是保留原本的get请求参数
    return config
})
axios.get( '/columns' ).then( resp => {
    console.log(resp.data)
})

// 使用场景案例2
// 所有网络请求时都需要添加加载的提示，则在vuex中定义全局变量，有请求时为true,请求结束后为false

axios.interceptors.request.use(config => {
  store.commit('setLogin', true)  // 发送请求时，设置为true ， 页面则相应的显示加载提示
  return config
})
axios.interceptors.response.use(config => {
  store.commit('setLogin', false) // 请求响应时，设置为false， 页面则相应的b显示加载提示
  return config
})

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

#  二、fetch

> https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch
>
> https://www.ruanyifeng.com/blog/2020/12/fetch-tutorial.html
>
> https://blog.csdn.net/qq_53225741/article/details/125239106
