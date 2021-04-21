title: HTTP(0x03)-跨域问题
date: 2020-09-03 10:43:17
tags:
- HTTP
categories:
- [HTTP]



跨域问题的情况、原理及解决方案

> [HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

#  一、跨域与同源策略

###  1.1 同源策略

* ajax请求时，**浏览器**要求当前网页和server必须同源（安全),服务端爬虫等时则不一定需要同源，因为这个同源要求是浏览器提出的保护机制
* 同源:协议、域名、端口，三者必须一致

* 加载图片、css 、js 可无视同源策略

  * `<img />`可用于统计打点，可使用第三方统计服务
  * `<link /> 、<script>`可使用CDN ，CDN一般都是外域
  * `<script>`可实现JSONP

  ```html
  <img src=跨域的图片地址/>
  <link href=跨域的css地址/>
  <script src=跨域的js地址></script>
  ```

  

###  1.2 出现跨域问题的几种情况

* 即不符合同源策略

```js
1 域名不同
http://www.a.com   http://www.b.com
2 端口不同
http://www.a.com:8080   http://www.b.com:8081
3 主域与子域之间访问
http://www.a.com   http://news.a.com(子域)
4 协议不同
http://www.a.com   https://www.a.com
```



#  二、跨域报错的原理

* 现有域名A: `http://localhost:8888`和域名B: `http://localhost:8887`
* 当在域名A中访问域名B时会报如下错误：

![image-20200921162845065](HTTP(0x03)-跨域问题/image-20200921162845065.png)

* 报错的原因指出了：是在域名B中没有设置`Access-Control-Allow-Origin`,若没有在域名B中做这个设置，则不能跨域访问它，所以跨域的本质是**被访问的一方需要做设置，允许访问方访问**
* 事实上，域名A访问域名B时是可以正常访问的，只是浏览器在访问的返回结果中解析到`Access-Control-Allow-Origin`没有设置允许域名A访问，所以报错，**所以报错的本质不是不能访问而是浏览器做了拦截工作，报错提醒**



#  三、跨域问题的解决方案

###  3.1 标签方式

* 在访问时，通过`script`、`image`、`link`等标签的`src`和`ref`直接访问

  ```js
  <script src="http://loaclhost:8887"></script>
  ```

* 通过标签直接访问时，不会有跨域的问题，无论被访问方是否设置了允许访问

###  3.2 JSONP方式

> JSONP返回的不是数据，而是一段js代码，执行前端已经定义好的函数，同时将后端的参数携带在函数中执行
>
> 借助下面的例子理解：
>
> ```html
>  <script src="http://localhost:8002/jsonp.js?username=xxx&callback=abc"></script>
> ```
>
> 等价于
>
> ```
>  <script>
>     abc(  
>         { name: 'xxx' }
>     )   
>  </script>
> ```

* JSONP方式本质还是利用了标签方式，`<script>`可绕过跨域限制

* 服务器可以任意动态拼接数据返回
* `<script>`就可以获得跨域的数据，只要服务端愿意返回

```html
//jsonp.html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>jsonp 演示</title>
    </head>
    <body>
        <p>一段文字 1</p>

        <script>
            window.abc = function (data) {
                console.log(data)
            }
        </script>
        <script src="http://localhost:8002/jsonp.js?username=xxx&callback=abc"></script>
    </body>
</html>
```

```js
//jsonp.js
abc(  
    { name: 'xxx' }
)
//此处的函数名abc可以是前段传入动态设置的，name的具体值也可以是前端传入的
```

* JSONP方式发送请求时也可以传入参数，
* 服务端接收参数后，会动态拼接成需要的格式，然后返回客户端

###  3.3 CORS方式

* 设置`Access-Control-Allow-Origin`

  * ` 'Access-Control-Allow-Origin': '*'`:  允许所有第三方访问

  ```js
  const http = require('http')
  
  http.createServer(function (request, response) {
    console.log('request come', request.url)
  
    //设置允许跨域访问
    response.writeHead(200, {
      'Access-Control-Allow-Origin': '*'
    })
    response.end('123')
  }).listen(8887)
  
  console.log('server listening on 8887')
  ```

  * `Access-Control-Allow-Origin': 'http://127.0.0.1:8888`:设置指定网址可以访问

  ```js
  //设置允许跨域访问
    response.writeHead(200, {
      'Access-Control-Allow-Origin': 'http://127.0.0.1:8888'
    })
  ```

  

​	

#  四、跨域限制及预请求验证

##  4.1 跨域请求

*以下是默认允许跨域跨域的，不需要额外设置*

**允许跨域的HTTP方法：**

* GET
* HEAD
* POST

**允许跨域的Content-Type:**

* text/plain
* multipart/form-data
* application/x-www-form-urlencoded

**请求头限制，只允许一下请求头：**

**允许跨域的请求头：**

- ``Accept``

- ``Accept-Language``
- ``Content-Language``
- `Content-Type`

##  4.2 预请求验证

预请求是指在存在跨域问题时，当发送正式请求前，浏览器会先发送一个`OPTIONS`请求，验证服务端是否有声明默认设置之外的请求头、请求方法等可以访问，若声明了则正式请求跨域访问，否则报错。**预请求本质就是通过`OPTIONS`请求验证了服务端是否允许访问**



**跨域预请求设置：**

* 在被访问端设置（服务端）
* `Access-Control-Allow-Origin`: 允许访问的域名
* `Access-Control-Allow-Methods`:设置默认方法之外可以访问的方法
* `Access-Control-Allow-Headers`:设置默认请求头之外的跨域访问的请求头
* `Access-Control-Max-Age`:设置预请求最大可以持续时间（在该时间段内可以直接发正式请求，不用每次都发预请求）

```js
  //设置允许跨域访问
  response.writeHead(200, {
    'Access-Control-Allow-Origin': 'http://127.0.0.1:8888',
    'Access-Control-Allow-Methods': 'POST, GET, OPTIONS',  //设置允许的默认方法之外的方法
    'Access-Control-Allow-Headers': 'X-PINGOTHER, Content-Type'//设置允许的默认请求头之外的请求头
    'Access-Control-Max-Age': '86400' //
  })
```



#  五、前端框架跨域问题

##  5.1 React跨域解决方案

> [React(0x04)-react跨域问题](https://chuckiewill.github.io/2020/04/12/React/React/React-0x04-react%E8%B7%A8%E5%9F%9F%E9%97%AE%E9%A2%98/)



#  六、koa中设置跨域访问（待完成）

> [koa2-cors官网](https://www.npmjs.com/package/koa2-cors)
>
> [koa2-cors应答跨域请求实现](https://www.cnblogs.com/fangshidaima/p/10495890.html)
>
> [koa2 允许跨域](https://www.jianshu.com/p/6009513b931e)

