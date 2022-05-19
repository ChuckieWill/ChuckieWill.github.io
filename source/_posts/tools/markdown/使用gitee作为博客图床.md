---
title: 使用gitee作为博客图床
date: 2022-01-13 10:21:53
tags:
- 使用gitee作为博客图床
categories:
- [tools, markdown]
---




> [使用gitee(码云)作为博客图床](https://blog.csdn.net/zenglintao/article/details/106076822)


####  参考上面的博客需要注意一下几点

* “二、创建仓库”步骤中需要初始化，生成master分支
* “三、获取token令牌”步骤中生成的私人令牌只显示一次，需要保存好
* “五、gitee插件配置”步骤中配置repo的仓库路径，注意是可以在浏览器中打开
* 下载PicGo，安装包位置

> [PicGo下载地址](https://github.com/Molunerfinn/PicGo/releases)

![image-20220113110414403](https://gitee.com/ChuckieWill/picture/raw/master/img/202201131104572.png)





####  Typroa中上传图片的配置

* 配置

![image-20220113114617302](https://gitee.com/ChuckieWill/picture/raw/master/img/202201131147788.png)



* 上传图片

  * 右击图片选择上传图片即可上传
  * *不能实现全文一键上传，只能一个一个图片的上传*
  * 如果要实现复制到文档中后直接自动上传到图床，可以做如下配置的修改

![image-20220113110923795](https://gitee.com/ChuckieWill/picture/raw/master/img/202201131109840.png)

* 配置图片重命名（命名去重，否则相同图片不能多次上传）
  * PicGo软件中勾选上传前重命名或者时间戳重命名

![image-20220113111205937](https://gitee.com/ChuckieWill/picture/raw/master/img/202201131113940.png)





####  注意事项

* 在gitee创建图床仓库时，一定要选为公开，否则在typroa和PicGo中都不能正常显示图片
* 修改为为公开的步骤如下

![image-20220113112714563](https://gitee.com/ChuckieWill/picture/raw/master/img/202201131127515.png)







![image-20220113112742663](https://gitee.com/ChuckieWill/picture/raw/master/img/202201131127936.png)



