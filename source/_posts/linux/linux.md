---
title: linux
date: 2021-03-03 10:46:17
tags:
- linux
categories:
- [Linux]

---



#  linux

#####  shell脚本

```sh
#!/bin/bash        // 向系统表明是shell文件

echo "build 生成生产环境代码"   // echo 表示输出 输出内容用双引号
```



#####  如何查看Ubuntu版本

```
cat /etc/issue
```

#####  在test.txtd第一行加入hello

````
sed '1i\hello' test.txt > tmp; mv -f tmp test.txt
````

##### 将本地文件拷贝到远程服务器

* `scp ` 远程拷贝
* `root@121.199.70.72:   ` 远程地址
  * `root`远程服务器名
  * `121.199.70.72`远程服务器地址
* `/var/www/zhihu` 文件要拷贝到远程服务器的文件夹路径

```
 scp -i ~/.ssh/vikingship.pem -r *  root@121.199.70.72:/var/www/zhihu
```

##### Xshell XFTP链接

* 密钥密码是密钥生成的时候设置的，如果生成密钥是一路回车，那密钥密码就是空（不输入即可）
* 本地密钥文件是不带后缀的id_rsa文件

![image-20230112133716636](linux/image-20230112133716636.png)

