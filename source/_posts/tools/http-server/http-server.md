---
title: 前端在本地启动服务预览html页面(http-server)
date: 2020-04-02 10:43:17
tags:
- html
categories:
- [tools,http-server]
---

> [npm: http-server](https://www.npmjs.com/package/http-server)

* 安装

```js
npm install --global http-server
```

* 使用
  * 切到需要预览的html文件的目录下执行:`http-server`
  * 此时默认启动的是`index.html`文件
  * 在网页输入：`http://localhost:8080/`,即可访问`index.html`文件的渲染效果
  * 若要访问同目录下非`index.html`的文件，则在`http://localhost:8080/`后追加文件名例如：`http://localhost:8080/index-demo.html`



