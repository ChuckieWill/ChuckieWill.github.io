---
title: postman
date: 2020-10-17 10:44:17
tags:
- poatman
categories:
- [tools, postman]

---



> [postman教程大全](https://www.jianshu.com/p/97ba64888894)

##  1 新建

###  1.1 新建项目集合

![image-20201017125827721](postman/image-20201017125827721.png)

###  1.2 新建请求并添加到项目集合中

![image-20201017130032408](postman/image-20201017130032408.png)





##  2 请求自定义

###  2.2 post

1. 路径传参

   * 其中`3`为传入的参数

   ```js
   localhost:3000/v1/3/classic/latest
   ```

2. `?param`传参

   * `maxthon`：为参数

   ```js
   localhost:3000/v1/classic/latest?param=maxthon
   ```

3. `headers`传参

   * 直接添加键值对即可

   ![image-20201017130424612](postman/image-20201017130424612.png)

4. `body`传参

   * 选择‘Body->row->选择格式->输入内容’

   ![image-20201017130522706](postman/image-20201017130522706.png)