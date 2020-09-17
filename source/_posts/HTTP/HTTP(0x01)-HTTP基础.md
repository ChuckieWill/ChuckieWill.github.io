---
title: HTTP(0x01)-HTTP基础
date: 2020-09-01 10:43:17
tags:
- HTTP
categories:
- [HTTP]
---









![image-20200916173624473](HTTP/image-20200916173624473.png)

* Redirect : 将URL跳转到指定的URL(如果有指定的情况)
* APP cache :  查找资源是否已经缓存
* DNS : 将域名转换成IP地址
* TCP :  三次握手
* Request : 发送请求
* Response ：响应请求



五层网络模型

![image-20200916174544942](HTTP/image-20200916174544942.png)

* TCP在传输层，HTTP在应用层，HTTP的发送是建立在TCP之上的
* HTTP1.0以前，一个TCP的建立对应一个HTTP，HTTP内容传输完成后，TCP连接会自动释放，*效率低*
* HTTP2.0开始，一个TCP的建立对应多个HTPP, 且可以是永久建立，即HTTP内容传输完成后，TCP连接任然保持建立，*效率高（减少了TCP建立的次数，从而减小了三次握手等产生的资源消耗）*



TCP连接建立与三次握手

