---
title: GraphPi的使用
date: 2022-03-02 15:43:17
tags:
- GraphPi
- 图挖掘
categories:
- [Graph]

---

#  GraphPi

> [GraphPi论文](https://pacman.cs.tsinghua.edu.cn/~zjd/publication/sc20-graphpi/sc20-graphpi.pdf)
>
> [GraphPi-github](https://github.com/thu-pacman/GraphPi)

##  实现第一个案例

> linux系统

按如下步骤就可以实现第一个案例：

* 从GitHub上克隆下GraphPi,   终端切到根目录GraphPi下，执行如下命令进行编译：
  * `cmake .`
  * `make`
* 终端切到`/bin`目录下，执行如下命令启动程序：
  * `./baseline_test Wiki-Vote ../dataset/wiki-vote_input 3 011101110`
    * `./baseline_test`: 可执行文件，项目本身的案例
    * `Wiki-Vote`:  数据集的名字， 项目自带
    * `../dataset/wiki-vote_input`:  数据集文件地址， 项目自带
    * `3` : 模式图顶点个数
    * `011101110`: 模式图的邻接矩阵
* 执行结果如下：

```
Load begin in ../dataset/wiki-vote_input
Success! There are 7115 nodes and 201524 edges.   //真实图图信息
Load data success!
Can not use in_exclusion_optimize with this schedule
ans 608389    //挖掘结果
time 0.018873
Schedule:
011
101
110
2 (1,2)(0,1)
```



##  快速上手

> 更多自定义的编程可查看-->[GraphPi-Github-Readme.md](https://github.com/thu-pacman/GraphPi)

利用项目本身提供的案例做自定义的修该， 项目本身案例文件位置：`GraphPi/tianhe/baseline_test.cpp`

###  自定义模式图

运行可执行文件时，终端输入即可

```
./baseline_test Wiki-Vote ../dataset/wiki-vote_input 3 011101110
```

* `3` : 模式图顶点个数  , 可根据自己的需求修改
* `011101110`: 模式图的邻接矩阵， 可根据自己的需求修改

###  自定义真实图

真实图的格式：第一行应该包含两个整数：顶点数和边数。 从第二行开始，每行包含两个整数 `x` 和 `y`，表示图中的一条无向边 `(x,y)`。

终端输入真实图：

```
./baseline_test Wiki-Vote ../dataset/wiki-vote_input 3 011101110
```

* `Wiki-Vote`:  数据集的名字， 自定义
* `../dataset/wiki-vote_input`:  数据集文件地址， 自定义

源码需要做修改的部分查看----->[GraphPi-Github-how-to-enable-your-own-dataset](https://github.com/thu-pacman/GraphPi#how-to-enable-your-own-dataset)

###  提升性能的配置

仍然在项目提供的案例上修改配置：`GraphPi/tianhe/baseline_test.cpp`

在main函数中根据需要修改如下配置：

```
int performance_modeling_type = 0;  //0:不使用性能模型， 1：使用
int restricts_type = 0;             //0: 不使用限制，1：使用
int use_in_exclusion_optimize = 0;  //0: 不使用包含排除优化，1：使用
```

