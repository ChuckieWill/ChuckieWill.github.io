---
title: LInux系统(CentOS)上载及安装依赖的方式及命令
date: 2021-11-20 10:46:17
tags:
- CentOS
categories:
- [Linux]
---



#  LInux系统(CentOS)上载及安装依赖的方式及命令

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

