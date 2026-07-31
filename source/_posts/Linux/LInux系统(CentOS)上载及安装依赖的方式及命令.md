---
title: LInux系统(CentOS)上下载及安装依赖的方式及命令
date: 2021-11-20 10:46:17
tags:
- CentOS
categories:
- [Linux]
---



#  LInux系统(CentOS)上下载及安装依赖的方式及命令

> [vue项目部署到腾讯云轻量应用服务器](https://chuckiewill.github.io/2020/06/10/Vue/vue%E9%A1%B9%E7%9B%AE%E9%83%A8%E7%BD%B2%E5%88%B0%E8%85%BE%E8%AE%AF%E4%BA%91%E8%BD%BB%E9%87%8F%E5%BA%94%E7%94%A8%E6%9C%8D%E5%8A%A1%E5%99%A8/)

##  方式一

>  以安装pcre依赖为例

1 通过wget命令下载安装包

```
wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre 8.37.tar.gz
```

2 解压文件

```
tar -zxvf pcre 8.37.tar.gz
```

3 cd到解压的文件夹下, 进行安装

```
./configure   或    ./configure --prefix=/usr/local/pcre  安装指定路径安装
make &&  make install
```

4 查看安装的版本号

```
pcre-config --verson
```



##  方式二

* 通过yum命令直接下载并安装

```
yum install pcre pcre-devel -y
```

