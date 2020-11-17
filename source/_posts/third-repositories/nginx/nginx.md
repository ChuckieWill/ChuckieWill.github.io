---
title: nginx
date: 2020-02-06 13:45:50
tags:
- nginx
categories:
- [third-repositories, nginx]
---

#  nginx

* **`nginx`用于部署项目**

##  一、基础介绍

主机-----> 操作系统（windows/linux）----->tomcat/nginx

###  1.1 安装

* [nginx官网]( http://nginx.org/ )

* [国内镜像下载地址]( http://mirrors.sohu.com/nginx/ )

* naginx版本
  *  mainline version ：主力版本，开发版
  *  stable version ：最稳定的版本，生产环境上建议使用的版本
  * legacy versions： 遗留老版本的稳定版
  
* 下载
  
  * 选择stable version  或者   点击官网右栏download
  
  ![](nginx/1590059550958.png)
  
  
  
  ![image-20200926102856390](nginx/image-20200926102856390.png)
  
  * 选择windows
  
  ![](nginx/1590059470671.png)

###  1.2 使用

* 关于nginx的cmd终端命令  *cmd终端切换到nginx的根目录下*
  * `nginx.exe`: 启动nginx   或者直接双击nginx.exe文件也可启动nginx
    * 网址输入localhost     页面显示welcome nginx则启动成功
    * 每次配置文件修改后都需要重启nginx
  * `nginx -s stop`:中断运行
  * `nginx -s quit`:正常退出运行
  * `nginx -s reload` : 重启niginx

###  1.3 Nginx的配置文件

> [`nginx.conf`文件解析](https://www.cnblogs.com/paulwhw/articles/11116363.html)

**`nginx.conf`文件**

```json

#user  nobody;
worker_processes  1;
#worker_processes数值越大并发能力越强  

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#错误存放的地址

#pid        logs/nginx.pid;

#以上为全局快


events {
    worker_connections  1024;
}

#以上为events块
#worker_connections数值越大并发能力越强


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    #include     servers/*.conf;
    server {
      listen       80; 
      server_name  test.com;

      location / {
        proxy_pass http://127.0.0.1:8888;
        proxy_set_header Host $host;
      }
    }

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }
		#以上为location块
		#root: 将接收的请求根据html路径去查找静态资源
		#index: 默认去上述中找到index.html或index.htm 以响应用户
    }
	#以上是server块
	#listen： nginx监听的端口
	#server_name: nginx接收的ip地址或域名
}
#以上为http块
#include 表示从外部引入文件


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


```



##  二、Nginx的反向代理

###  2.1 正向代理与反向代理

**正向代理**

1. 正向代理是由客户端设立的
2. 客户端了解代理服务器和目标服务器都是谁
3. 帮助实现突破访问权限，提高访问的速度，对目标服务器隐藏客户端的ip地址

![image-20200926200045670](nginx/image-20200926200045670.png)

**反向代理**

1. 反向代理服务器是配置在服务端的
2. 客户端是不知道具体访问的是哪台服务器
3. 达到负载均衡，并且可以隐藏服务器真正的ip地址

![image-20200926200342876](nginx/image-20200926200342876.png)

###   2.2 反向代理配置

* 按`nginx-1.18.0\conf`打开`nginx.conf`文件

  * 添加`include servers/*.conf`
    * 注意添加的位置，在`server`上面
    * `include servers/*.conf;`： 导入当前文件所在文件夹下的文件夹servers下的所有.conf文件
    * servers文件夹和文件夹下的文件都是自定义的，是自定义的配置文件

  ![image-20200926123759270](nginx/image-20200926123759270.png)

* `servers/test.conf`代理配置

  * 新建`nginx-1.18.0\conf、servers、test.conf`文件

  ```js
  //test.conf
  server {                            //server表示nginx启动的本地服务
    listen       80;                  //80 是nginx本地服务的端口号
    server_name  localhost;            //localhost  是nginx本地服务的域名地址
  
    location / {
      proxy_pass http://127.0.0.1:8888/;   //nginx代理的地址访问localhost即会跳转到这个地址
      proxy_set_header Host $host;        //host延用localhost而不是http://127.0.0.1:8888的host
    }
  }
  ```

* 也可直接将`test.cong`文件的内容加在`nginx.conf`文件中，效果是一样的，只是这样分离更方便管理不同的配置

  * 添加在`nginx.conf`中`server`配置的地方

###  2.3 location路径映射

**理解`\`**

* location后面的`“/”`指的是上面`server_name  localhost;`的`localhost`

```json
server {                            
  listen       80;                  
  server_name  localhost;            

  location / {
    proxy_pass http://127.0.0.1:8888/;  
    proxy_set_header Host $host;       
  }
}
```

**location匹配方式**

```json
# 1. = 匹配
location = / {
	#精准匹配，主机名后面不带任何的字符串
} 
#假如server_name是localhost  则只能精准匹配localhost   而localhost/xxx则不能匹配  因为多了/xxx

location / {
	#只要有/即可匹配
} 
#假如server_name是localhost  则只要有localhost即可匹配   localhost和localhost/xxx等都可匹配
```

---

```json
# 2. 通用匹配
location /xxx {
	#匹配所有以/xxx开头的路径
} 
```

----

```json
# 3. 正则匹配
location ~/xxx{
	#匹配所有以/xxx开头的路径   与通用匹配一样  只是优先级高
}
```

---

```json
# 4. 匹配开头路径
location ^~/xxx{
	#匹配所有以/xxx开头的路径   与通用匹配、正则匹配一样  只是优先级高于另外两个
}
```

---

```json
# 5. 匹配结尾
location ~*\.(gif|jpg|png)${
	#匹配以gif|jpg|png结尾的路径
}
```

**location匹配优先级**

```json
(location = ) > (location /xxx/yyy/xxx) > (location^~) > (location ~ , ~*) > (location/起始路径) > (location/)
                                                                                                
=匹配 > 完全匹配 > 开头匹配 > 正则匹配和结尾匹配 > 通用匹配(只有起始路径，匹配不全) > 通用匹配(只有根路径匹配)
```

* 完全匹配： 目标路径是localhost/xxx/yyy/xxx  匹配路径也是localhost/xxx/yyy/xxx   完全一致
* 一般匹配从上面的优先级的后面往前匹配**直到`(location^~)`为止，所以写配置文件的时候按上面的优先级顺序写**





##  三、Nginx的负载均衡



##  四、Nginx的动静分离



##  五、Nginx集群

##   六、部署

###  6.1 本地部署

#### 6.1.1 方式一：

* 双击nginx-1.18.0文件夹下nginx.exe文件  启动运行
* 网址输入localhost     页面显示welcome nginx则启动成功
* 删除nginx-1.18.0\html文件夹下的文件
* **将需要部署的项目的dist文件夹下的所有文件及文件夹复制到nginx-1.18.0\html文件夹下**
* 网址再次输入localhost 则可查看部署项目了

#### 6.1.2 方式二：

* 双击nginx-1.18.0文件夹下nginx.exe文件  启动运行
* 网址输入localhost     页面显示welcome nginx则启动成功
* **将需要部署的项目的dist文件夹复制到nginx-1.18.0文件夹下**
* **修改nginx-1.18.0\conf\nginx.conf文件**

```
//修改前
location / {
    root   html;
    index  index.html index.htm;
}

//修改后
location / {
    root   dist;
    index  index.html index.htm;
}
```

###  6.2远程部署

####  6.2.1 部署流程

* 前置步骤：
  * 1 购买云服务器，Linux
  * 2 购买域名、备案、解析域名与ip匹配
* 安装软件，在云服务器上
  * mysql
  * node
  * xampp
  * nginx
* nginx配置转发
  * nginx端口设置为80
  * 在云服务器上可以开启多个服务
  * nginx根据域名转发到对应的云服务器上的服务
    * ------------- *此处可查看上文：2.3location路径映射*---------------
    * 例如：chu.com  转发到  云服务商开启的localhost:3000服务
    * 例如：ckie.com  转发到  云服务商开启的localhost:4000服务
* 启动服务（node环境）
  * 不能直接在终端启动服务，因为终端关闭后，服务也关闭了
  * 开启**守护进程**即终端关闭后，服务仍然运行
    * 使用**pm2**---->[pm2官网文档](https://pm2.keymetrics.io/)
      * 安装：`npm install pm2 -g`
      * 启动： `pm2 start app.js`
      * 查看：`pm2 list`
      * 停止：`pm2 stop app`

* tips
  * 免费的https: lets encrypt