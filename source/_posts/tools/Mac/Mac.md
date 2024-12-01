---
title: Mac软件安装
date: 2023-03-20 10:46:17
tags:
- Mac
categories:
- [tools, mac]

---

#  Mac安装Homebrew

>  [Mac如何安装Homebrew](https://blog.csdn.net/weixin_45277161/article/details/134719230?spm=1001.2014.3001.5501)

**curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused**问题

> [curl: (7) Failed to connect to raw.githubusercontent.com port 443: Connection refused的几种解决方式](https://blog.csdn.net/qq_43531694/article/details/106862753)

* ip地址直接使用文章中的ip地址即可 `199.232.68.133`

#  Mac安装Git

>  [Mac电脑如何安装git](https://blog.csdn.net/weixin_45277161/article/details/134709441)

#  Mac安装Python

> https://blog.csdn.net/Python966/article/details/132405584



#  Mac安装Node.js

> https://www.php.cn/faq/528344.html





#   终端

显示可用的shell

```shell
xiating@192 ~ % cat /etc/shells
```

切换shell

```c++
chsh -s /bin/bash
  
chsh: no changes made  问题待解决
```

显示使用的shell

```shell
echo $SHELL 
```



修改终端显示所有路径

```shell
vim ~/.zshrc
# .zshrc开头添加下面的内容  这里的 %~ 表示当前工作目录的完整路径
export PS1="%~ %# "

source ~/.zshrc
```

