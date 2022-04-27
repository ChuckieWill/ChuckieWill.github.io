---
title: 图数据挖掘的数据集
date: 2022-03-29 15:43:17
tags:
- DataSet
categories:
- [Graph]
---

#  DataSet

###  无向图同构网络

> https://law.di.unimi.it/datasets.php

###  无向图异构网络

![image-20220329144505366](https://gitee.com/ChuckieWill/picture/raw/master/img/202203291445216.png)

* 出处：GRAMI: Frequent subgraph and pattern mining in a single large graph     2014
  * Twitter : https://socialcomputing.asu.edu/datasets/Twitter
    * 顶点: 用户，     边: 用户之间有关系，    顶点标签：0~100高斯分布(图数据原本没有标签，自行添加)
  * Patents: http://datasets.syr.edu/pages/datasets.html    
    * **亚利桑那州立大学提供，可用**，还有多个数据集，（美国国家经济研究局（NBER）维护数据）   只有顶点和边
    * 顶点: 专利，     边: 引用专利，    顶点标签：论文中未提及
  * Aviation : http://ailab.wsu.edu/subdue
    * **可用**        数据格式不一致    需要自己预处理
    * 三元组的形式，点为浮点数，属性为字母
  * MiCo : https://academic.research.microsoft.com
    * 顶点：作者，   边：合作作者（边上还标有合作的数量），   顶点标签：作者的领域
  * CiteSeer : https://cs.umd.edu/projects/linqs/projects/lbc
    * 有向图
    * 顶点：出版物，   边：出版物之间有相似性（带标签 0~100 标签越小表示相似性越强），   顶点标签：计算机科学领域

<img src="C:/Users/Chuckie/AppData/Roaming/Typora/typora-user-images/image-20220329151429029.png" alt="image-20220329151429029" style="zoom: 50%;" />

* 出处：Fractal: A General-Purpose Graph Patern Mining System      2019
  * MiCo: 出处：GRAMI: Frequent subgraph and pattern mining in a single large graph
    * 顶点：作者，   边：合作作者，   顶点标签：作者的领域
  * Patents： https://www.nber.org/research/data/us-patents
    * **可用， 美国国家经济研究局（NBER）**      需要tpt文件格式的转换  直接文本打开是乱码
    * 顶点: 专利，     边: 引用专利，    顶点标签：专利发布的年份
  * Youtube ： https://netsg.cs.sfu.ca/youtubedata/
    * **可用，加拿大西蒙弗雷泽大学**       数据很多  需要预处理只提取需要的数据
    * 顶点: 视频，     边: 视频之间有关联，    顶点标签：视频的等级和长度综合计算的结果
    * 2007年2月至2008年7月发布的视频
  * Wikidata： 出处：Wikidata:A Free Collaborative Knowledgebase     2014
    * 顶点: 主体和对象，     边: 谓词，    顶点标签：不同类型的谓词

