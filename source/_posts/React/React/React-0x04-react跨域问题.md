---
title: React(0x04)-react跨域问题
date: 2020-04-12 11:51:14
tags:
- react
- react跨域
categories:
- [react, react]
---

# `React`跨域问题

## 1 出现跨域问题的几种情况

```
1 域名不同
http://www.a.com   http://www.b.com
2 端口不同
http://www.a.com:8080   http://www.b.com:8081
3 主域与子域之间访问
http://www.a.com   http://news.a.com(子域)
4 协议不同
http://www.a.com   https://www.a.com
```

##  2 react中解决跨域的三种方式

###  2.1  在`papackage.json`里配置

```js
//方法一：create-react-app脚手架低于2.0版本时候，可以使用对象类型，否则会报错
//后面2项（pathRewrite，secure）可以不写
"proxy":{
   "/api":{
      "target":"http://m.kugo.com",//目标服务器
      "changeOrigin": true//默认false，是否需要改变原始主机头为目标URL
      "pathRewrite": {"^/api" : "/"},// 重写请求，比如我们源访问的是api，那么请求会被解析为/
      "secure": false, //如果是https接口 需要配置这个参数为true
    }
}
    

//方法二：最新的create-react-app脚手架2.0版本以上只能配置string类型
"proxy": "http://m.kugo.com",
```

###  2.2  `middleware`中间件进行配置 

```js
//方法三：
//1.先安装http-proxy-middleware
npm install http-proxy-middleware --save
//2.然后在src目录下创建 setupProxy.js 文件
//3.最后设置代理（setupProxy.js文件内容）
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    proxy.createProxyMiddleware('/api', {  //`api`是需要转发的请求 
      target: 'https://www.baidu.com/',  // 这里是接口服务器地址
      changeOrigin: true,
      pathRewrite: {'^/api': ''}
    })
  )
}
```

*  一般下载的新脚手架的项目根目录中没有`config`文件夹,暴露出项目配置文件，项目下执行: 

  ```js
  npm run eject
  ```

  * 前提： 先`git add 和git commit`否则执行上面的命令会报错

