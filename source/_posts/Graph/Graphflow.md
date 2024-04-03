---
title: Graphflow的使用
date: 2023-04-12 15:43:17
tags:
- Graphflow
- 图挖掘
categories:
- [Graph]
---

github: https://github.com/queryproc/optimizing-subgraph-queries-combining-binary-and-worst-case-optimal-joins

论文：http://amine.io/papers/wco-optimizer-vldb19.pdf



```shell
 # 设置环境变量  需要是jdk11版本的
 export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

 # 初始化
 ./gradlew clean build installDist
 
 . ./env.sh
 
 cd scripts

 # 以下都是在/scripts目录下执行
 # 1 将txt的边表格式转换为csv格式数据，主要是加了标签，默认标签都是0
 python3 change_snap_to_csv.py ../../dataset/Wiki-Vote.txt ../dataset/Wiki-Vote.csv
 #    d
 python3 serialize_dataset.py /absolute/path/edges.csv /absolute/path/data -v /absolute/path/vertices.csv
 # 2 将csv格式数据转换为系统需要的格式
 python3 serialize_dataset.py ../dataset/Wiki-Vote.csv ../data/Wiki-Vote
 # 3 为数据集生成优化器，每个数据集都要执行，文件生成的地方就是数据集存储的地方
 python3 serialize_catalog.py ../data/Wiki-Vote/
 
 # 执行查询，无标记的情况，或者说标记都为0的情况
 # 如下是执行一个3角环的模式图，顶点标记都为0，使用28线程
 python3 execute_query.py "(a:0)->(b:0),(b:0)->(c:0),(c:0)->(a:0)" ../data/Ego-Facebook/ -t 28
```

退出远程连接，再次进入需要执行如下命令才能正常执行查询

* node 1

```shell
# 设置环境变量  需要是jdk11版本的
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

# 更新
./gradlew build installDist
 
. ./env.sh
```

* node3

```c++
export JAVA_HOME=/home/wangyj/jvm/jdk-11
```



查看jdk版本

```sh
java -version
```

查看 Java 安装路径

* linux上一般在/usr/lib/jvm下面

```sh
which java
```



处理顶点单标记

