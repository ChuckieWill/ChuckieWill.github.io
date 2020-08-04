---
title: axios
date: 2020-08-04 20:45:50
tags:
- axios
categories:
- [third-repositories, axios]
---

#  一、axios基础

##  0 官网 

[axios官网]( http://www.axios-js.com/zh-cn/ )

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

