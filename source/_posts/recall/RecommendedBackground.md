---
title: Recommended Background
date: 2024-12-09 12:25:48
tags:
- recall
categories:
- [recall]
---

#  Recommended Background

[github推荐系统学习](https://github.com/datawhalechina/fun-rec/blob/master/readme.md)

[双塔召回模型](https://github.com/datawhalechina/fun-rec/blob/master/docs/ch02/ch2.1/ch2.1.2/DSSM.md)

[王树森推荐系统视频](https://www.bilibili.com/video/BV1PS4y1A7za/?vd_source=7230a052308bbb41976f248d2c778e3a)

[王树森推荐系统课](https://github.com/wangshusen/RecommenderSystem/blob/main/README.md)

* 笔记： https://www.bilibili.com/opus/985130073123192855?spm_id_from=333.999.0.0
* 笔记: https://www.bilibili.com/opus/915316101279121442?spm_id_from=333.999.0.0
* 语雀全面笔记：https://www.yuque.com/yuejiangliu/recommended-system-in-the-industry/resource



#  名词

深度神经网络——DNN(Deep Neural Networks)

图神经网络（Graphic Nuaral Network，GNN）

* 类似于一般的神经网络（[DNN](https://so.csdn.net/so/search?q=DNN&spm=1001.2101.3001.7020)）一样，会对输入的数据进行改变得到输出数据，不同的是**GNN的输入是一个图，输出也是一个图。**
* [什么是图神经网络GNN？](https://blog.csdn.net/qq_44452377/article/details/127042725)



#  相似性计算

- **欧式距离**：适合用于衡量用户或物品在特征空间中的绝对距离，距离越小表示越相似。
- **内积**：用于衡量用户对物品的偏好程度，值越大表示偏好越强。
- **余弦相似度**：用于衡量用户或物品之间的方向相似性，值越接近1表示越相似。

[余弦距离、欧氏距离和杰卡德相似性度量的对比分析 ](https://www.cnblogs.com/chaosimple/p/3160839.html)

[向量、内积、余弦定理](https://17aitech.com/?p=1922)

[数学基础](https://17aitech.com/?p=1922)

```
a*b = |a|*|b|*cos<a,b>
cos<a,b> = (a^2+b^2-c^2)/2bc
```

