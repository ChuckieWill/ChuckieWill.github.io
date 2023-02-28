---
title: 图挖掘系统研究现状梳理
date: 2022-06-02 15:43:17
tags:
- 图挖掘
categories:
- [Graph]


---

#  图挖掘系统研究现状梳理

* 没有实现异构的原因，为什么不支持有向图和顶点、边都带属性，难点在哪
* 研究内容
* 优化方向
* 编程模型
* 记录各种问题的论文出处

##  期刊

* **SOSP**、**OSDI**  软件工程/系统软件/程序设计语言        A类会议
* **MICRO**、**SC**、 **ASPLOS**、 **ISCA**、**USENIX ATC**  计算机体系结构/并行与分布计算/存储系统    A类会议
* **ICDE**、 **VLDB**、 **SIGMOD**  数据库／数据挖掘／内容检索     A类会议
* **ICS**、**EuroSys**、**PACT** 计算机体系结构/并行与分布计算/存储系统   B类会议
* **CIKM**  数据库／数据挖掘／内容检索 B类会议
* **TODS **(ACM Trans) 数据库/数据挖掘/内容检索 A类期刊

##  论文

> https://xxxxyu.github.io/
>
> undirected

### **软件**

####  基于CPU

* **Arabesque**: a system for distributed graph mining
  * SOSP 15
  * 枚举所有可能的嵌入，然后使用同构测试排除冗余嵌入
* **Scalemine**: scalable parallel frequent subgraph mining in a single large graph
  * SC 2016
  * 频繁子图专用系统  分布式
* **NScale**: neighborhood-centric large-scale graph analytics in the cloud
  * VLDB 2016
  * 基于MapReduce
* **DistGraph**     A distributed approach for graph mining in massive networks
  * 2016
  * 频繁子图专用系统  分布式
* **G-thinker:** Big Graph Mining Made Easier and Faster
  * CORR  2017
* **EmptyHeaded**: A Relational Engine for Graph Processing
  * ACM trans  TODS  2017 10
  * CPU
  * DFS
* **Graphflow**:An Active Graph Database
  * SIGMOD 2017 5
  * CPU
  * DFS
* **ASAP**: Fast, Approximate Graph Pattern Mining at Scale
  * OSDI 18
  * 近似计算
* **RStream**:Marrying Relational Algebra with Streaming for Efficient Graph Mining on A Single Machine
  * OSDI 18
  * *不支持自定义模式图* 
  * 依赖SSD来减少中间嵌入的内存消耗。
  * 相关工作：
    * 分布式系统
      * DistGraph、Arabesque、G-thinker
    * 专用图挖掘算法
      * gSpan[78]是一种高效的频繁子图挖掘算法，设计用于挖掘多个输入图
* **G-miner**: An efficient task-oriented graph mining system
  * EuroSys 18 *长文、SIGMOD 19 *短文*  
  * 带标签 分布式
  * 相关工作
    * NScale、G-thinker、Arabesque
* **Fractal**: A general-purpose graph pattern mining system
  * SIGMOD 19
  * 支持异构
  * 分布式
  * 利用算法技术来减少中间嵌入的内存消耗
  * 相关工作
    * Arabesque[53]和NScale[45]代表了第一代通用分布式系统，它们基于以子图为中心的编程模型进行图形处理
    * NScale（建立在Hadoop上）不是为了处理FSM和相关GPM问题而设计的，它是否能够扩展到生成大型中间状态（可能会压倒Hadoop文件系统）的问题尚不清楚
* A Deeper Dive into Pattern-Aware Subgraph Exploration with PEREGRINE.
  * 在扩展过程中进行同构检测去重，避免了冗余计算
* **Graphzero**: Breaking symmetry for efficient graph mining
  * CORR 1911
  * 在扩展过程中进行同构检测去重，避免了冗余计算
* **Automine**: harmonizing high-level abstraction and high performance for graph mining.
  * SOSP 19
  * 未开源 *不支持自定义模式图* 
  * 在扩展过程中进行同构检测去重，避免了冗余计算
  * 相关工作
    * 基于Giraph[1]构建的Arabesque[46]是第一个通用的分布式图形挖掘系统，引起了社区的极大兴趣
    * G-thinker[55]和G-miner[10]解决了Arabesque的一些性能问题，具有较低级别的接口。不幸的是，当前版本的系统不支持频繁的子图挖掘和基序计数
    * DistGraph、 ScaleMine、ASAP、Rstream
* **G-thinker:** A Distributed Framework for Mining Subgraphs in a Big Graph
  * ICDE 20 4yue
  * 相关工作
    * 
* **DwarvesGraph:** A High-Performance Graph Mining System with Pattern Decomposition
  * CORR 2008
* **Kaleido**: An Efficient Out-of-core Graph Mining System on A Single Machine.
  * ICDE 20
  * 未开源 *不支持自定义模式图* 
  * 核外处理  和Rstream类似
  * **标记图**
* **GraphPi**: High Performance Graph Pattern Matching through Effective Redundancy Elimination
  * SC 20
  * 在扩展过程中进行同构检测去重，避免了冗余计算
  * 没有明确边的方向性
* **Pangolin**: An Efficient and Flexible Graph Mining System on CPU and GPU.
  * VLDB 20 
  * 未开源  *不支持自定义模式图*
  * 在扩展过程中进行同构检测去重，避免了冗余计算
* **Peregrine**: A Pattern-Aware Graph Mining System. 
  * EuroSys 20
  * 支持异构
  * 在扩展过程中进行同构检测去重，避免了冗余计算
* **aDFS**: An Almost Depth-First-Search Distributed Graph-Querying System
  * USENIX ATC 21
* **GraphZero**: A High-Performance Subgraph Matching System
  * SIGOPS 21 6月
  * 要求模式图无向  输入图可有向
* **Sandslash**: A Two-Level Framework for Efficient Graph Pattern Mining
  * ICS 21
  * 只基于CPU
* **Tesseract**: Distributed, General Graph Pattern Mining on Evolving Graphs
  * EuroSys 21
  * 进化图，时序图


###  硬件

####  基于GPU

* **PBE**: GPU-Accelerated Subgraph Enumeration on Partitioned Graphs
  * SIGMOD 20 5月
  * 基于GPU的子图匹配  不属于GPM
* **GSI**: GPU-friendly Subgraph Isomorphism.
  * ICDE 20 5yue
  * 基于GPU的特定图算法  不属于GPM
* **PBE**: Exploiting Reuse for GPU Subgraph Enumeration
  * IEEE Trans  20 11yue
  * 基于GPU的子图匹配  不属于GPM
* **Pangolin**: An Efficient and Flexible Graph Mining System on CPU and GPU.
  * VLDB 20  5
  * 未开源  *不支持自定义模式图*
  * 在扩展过程中进行同构检测去重，避免了冗余计算
  * 最早的基于CPU的CPM系统
* **GAMMA** : A GPU-based Graph Pattern Mining System
  * CIKM 22 10月  短文5页
  * GPU加速
* **G2Miner**: Efficient and Scalable Graph Pattern Mining on GPUs.
  * OSDI 22  7月
  * GPU加速
  * 明确定义无向图

####  **加速器**

* **Gramer**: A Locality-Aware Energy-Efficient Accelerator for Graph Mining Applications
  * MICRO 20  10月
  * 最早的图形挖掘加速器之一
  * 基于模式遗忘，比AutoMine速度还慢
  * 图形处理和GPM工作负载具有不同的内存访问模式
* **TrieJax**: The TrieJax Architecture: Accelerating Graph Operations Through Relational Joins.
  * ASPLOS 20
  * 基于模式感知
* **IntersectX**: An Accelerator for Graph Mining
  * CORR 2012(20年12月)
  * 硬件加速
  * 扩展ISA和架构支持，优化CPU上的GPM执行
* **SISA**: Set-Centric Instruction Set Architecture for Graph Mining on Processing-in-Memory Systems
  * MICRO 21
  * 硬件加速
  * 建议通过提出以集合为中心的ISA和快速集合交集算法来减轻现有PIM架构上的计算负担
* **FlexMiner**: A Pattern-Aware Accelerator for Graph Pattern Mining
  * ISCA 21
  * 硬件加速
  * 提出一种模式感知的GPM加速器，提高了现有加速器[24，60]的性能和通用性
* **FINGERS**: exploiting fine-grained parallelism in graph mining accelerators
  * ASPLOS 22  2月
  * 硬件加速
* **NDMiner**: Accelerating Graph Pattern Mining Using Near Data Processing
  * ISCA 22  6月
  * 硬件加速
  * 使用特定于域的优化（硬件负载消除、编译器优化和设置操作重新排序）改进了SISA
  * 通过采用特定于域的NDP架构，包括新颖的优化，如循环嵌套扁平化和设置操作重新排序  优于FlexMiner

##  相关工作

* **Kaleido**: An Efficient Out-of-core Graph Mining System on A Single Machine.

  * ICDE 20

  * 核外处理  和Rstream类似

  * 图挖掘算法： 

    * gSpan[38]是一种高效的频繁子图挖掘算法，设计用于挖掘多个输入图，然而，gSpan是为多个挖掘问题图设计的。如果我们有一个单一的输入图，我们必须在同一个图中找到多个实例，因此这就复杂了问题
    * Michihiro等人[20]首先提出了从单个图中挖掘模式的算法。他们使用基于最大独立集的支持的昂贵的反单调定义来寻找边缘不相交嵌入
    * GraMi[12]在单个大型图中提出了一种有效的方法，并给出了支持结构约束的扩展版本和近似版本
    * Prüzulj等人[28]介绍了基序计数问题。Ribeiro等人[30]提出了G-Tries，这是一种存储和查找基序频率的有效方法。Apar´ıcio等人[4]设计并实现了G-Tries的并行版本。最大集团是一个研究得很好的问题

  * 图形挖掘系统： 

    * Arabesque[36]是一个支持流行挖掘算法的分布式图挖掘系统。Arabesque提出了一种具有嵌入概念的图探索模型。Arabesque探索了在用户定义的过滤器约束下的所有嵌入，开发人员使用过滤器过程编程模型处理每个嵌入。与Kaleido相比，Arabesque需要对移动嵌入中的每个嵌入进行另一次规范检查

    * ScaleMine[1]是一个并行频繁子图挖掘系统，它首先计算频繁模式的近似解，并通过使用第一步的结果来统计精确解，以修剪搜索空间

    * NScale[29]旨在使用MapReduce框架解决图挖掘问题。它提出了一个邻域中心模型，其中一个兴趣点的k跳邻域子图由k轮MapReduce构建，每轮MapReducte都扩展了1跳新邻域，然而，MapReduce在处理候选子图时的开销非常高

    * G-Miner[9]是一个分布式图挖掘系统，它将子图处理建模为独立的任务，并为任务流水线设计合适的调度。然而，G-Miner不支持FSM和基序计数

    * ASAP[18]是一种用于图形模式挖掘的分布式、基于采样的近似计算引擎。

      ASAP利用图近似理论，并将其扩展到分布式环境中的一般模式。它允许用户权衡结果延迟的准确性。然而，ASAP只计算用户的兴趣，并有一个可接受的错误，如图案计数和模式匹配，但不能返回频繁模式的准确结果

    * RStream[37]是第一个单机核心外图挖掘系统。RStream采用GRAS编程模型，该模型结合了GAS模型和关系代数来支持多种挖掘算法。然而，RStream中输入图的子图和边的连接仍然是一个昂贵的操作。子图的边缘诱导探索也使一些挖掘问题复杂化，如基序计数和集团发现

  * 图同构检查库

    * 

* **GraphPi**: High Performance Graph Pattern Matching through Effective Redundancy Elimination

  * SC 20
  * 通用图形挖掘系统：
    * Arabesque[14]是第一个提供高级抽象和灵活编程模型的分布式图挖掘系统，它维护子图的一些中间数据，并通过将边附加到候选子图并使用用户定义的过滤器和过程函数过滤新生成的候选来生成所有模式实例
    * G-thinker(2017)[16]提供了一个直观的图形探索API，用于实现各种图形挖掘算法和高效的运行时引擎
    * G-Miner[17]将图形挖掘作业的处理建模为一个独立的任务，并通过新颖的设计简化了任务处理
    * RStream[15]是一个**单机系统**，它通过元组流高效地实现关系代数。为了支持可扩展的图形挖掘，RStream使用核心外处理来利用磁盘支持来存储中间数据
  * 图形模式匹配系统
    * 专门的模式匹配系统[19]-[21]，[“Turboiso:29]，[39]，[40]
    * Automine[18]建立在基于集合的表示上，并使用编译技术生成高效的模式匹配代码。然而，由于结构模式的固有对称性，Automine算法导致了大量的计算冗余
    * 基于Automine，GraphZero[12]提供了一种基于群论的算法，以打破模式的固有对称性并消除冗余计算
    * Peregrine[41]是另一个基于DFS的系统，它提供了基于模式的编程模型和类似于GraphZero的工作流。Peregrine还有一个日程生成模块。然而，Peregrine生成的时间表仅基于模式，没有考虑数据在不同数据图中的分布
  * 近似子图计数
    * 一些计算引擎和系统被设计用于估计嵌入的近似数量[2]、[42]、[43]
    * ASAP[23]是其中最先进的一种。ASAP是一个用于图形模式匹配的分布式近似计算引擎。基于邻域采样算法，ASAP从图的边缘集的流中采样嵌入以进行近似估计。尽管这些近似系统具有良好的可扩展性，但它们不能列出所有嵌入。

* **Peregrine**: A Pattern-Aware Graph Mining System. 

  * EuroSys 20
  * 通用图形挖掘系统: 
    * 已经开发了几种通用图形挖掘系统[8，12，Fractal 22，34，Arabesque 52，57]
      * Arabesque[52]是一个分布式图挖掘系统，它遵循在map reduce之上开发的过滤过程模型,它提出了“像嵌入一样思考”的处理模型
      * Fractal[12]将此扩展到分形的概念，它将用户程序的一部分暴露给系统；结合深度优先探索，分形允许系统更智能地规划其执行
      * G-Miner[8]是一个面向任务的分布式图挖掘系统，能够使用分布式任务队列构建自定义图挖掘用例
      * RStream[57]是一个利用SSD存储中间解决方案的**单机核心外**图挖掘系统。它使用关系代数将挖掘任务表示为表连接
      * AutoMine[34]是一个最新的**单机系统**，它生成有效的代码来匹配常见的图挖掘任务的模式
    * 问题
      * 虽然Fractal在模式匹配用例中使用了对称破坏，但其他应用程序（如FSM和基序计数）不受对称破坏的指导，因此它们最终会进行不必要的探索
      * AutoMine也没有对任何用例使用对称破坏，要求用户在枚举模式时通过单独检查每个匹配来过滤重复匹配
    * ASAP[22]是一个用于近似图挖掘的可编程**分布式系统**，其中用户基于采样边和顶点编写程序，以推理模式的概率计数
  * 专门构建的图形挖掘解决方案：
    * 
  * 图形处理系统：
    *  [11, PowerGraph:14, GraphX:15, DistTC:21, GraphOne:29, Pregel:32, GraphBolt:33, 42, Chaos:45, GPS:46, Ligra:48, KickStarter:ASPIRE:CoRAL:54–56, Gemini:62]

* **aDFS**: An Almost Depth-First-Search Distributed Graph-Querying System

  * USENIX ATC 21
  * 数据库管理系统（DBMS):............................
  * 图形算法: 
  * 图形查询: 
    * 学术界提出了许多单节点图查询系统：
      * Sun等人[66]和Lin等人[49]构建关系和事务系统
      * Graphflow[44]是一个支持评估一次性和连续子图查询的活动图数据库
      * TurboFlux[46]在快速图更新流上优化快速连续子图匹配
      * CECI[22]使用多个嵌入簇和邻域列表的交集来优化子图匹配
    * 工业图形查询解决方案
      * Neo4j[10]是单机，支持Cypher[11]查询
      * Amazon Neptune[1]是为亚马逊云构建的
      * Facebook Dragon[3]根据访问数据的更新建立索引
      * Microsoft Graph Engine[8]是一个基于Trinity[63]的内存数据处理系统，
      * TigerGraph[18]基于给定查询跳的源顶点数据分发GSQL[4]查询
      * JanusGraph[6]使用分布式图形存储，但不分布式计算
      * GraphFrames[31]使用数据帧的连接实现了与Spark的图形模式匹配
      * Wukong[64，73]是一个基于图的分布式RDF存储，它利用了硬件特性，如RDMA和GPU，而我们并不关注这些特性
      * **aDFS是第一个真正的分布式图形查询系统，它可以处理完全分区的图形，并严格限制内存，同时保持良好的性能**
  * 图形挖掘系统:
    * 图挖掘侧重于通过探索图的子图结构来提取结构属性和计算图的复杂聚集统计[34，74], 示例包括三角形计数、最大集团发现、社区检测和图形匹配[G-Miner: 27，AutoMine: 54，NScale(2016): 59]
    * 图形挖掘系统通常利用自同构消除[29，Fractal:32, Peregrine:42]，而图形查询引擎生成同态来回答用户图形查询
    * 最近的**单机系统**（ single-machine systems）包括RStream[72]、AutoMine[54]和Peregrine[42]
    * **分布式系统**(Distributed systems)包括Arabesque[69]、NScale[59]、G-thinker（2017）[77]、BiGJoin[19]、G-Miner[27]、ASAP[41]和Fractal[32]
    * 异步计算的形式在G-Miner[27]（使用“任务管道”来隐藏通信开销）和BiGJoin[19]（使用数据并行数据流计算来提取匹配最少的动态连接列）中使用
    * G-Thinker[77]（在基于磁盘的优先级队列中缓冲过多的子图任务）、BiGJoin[19]（主要使用批处理来限制内存消耗，但不像aDFS那样用于中间结果）和Fractal[32]使用了减少内存消耗的技术
  * BFS/DFS:
    * 通常，DFS用于调度任务图以缩减内存[28]，BFS被机会主义地使用（通常称为“工作窃取”）以最大化并行性[23，24]

* **GraphZero**: A High-Performance Subgraph Matching System 

  * SIGOPS 21 6月
  * 基于AutoMine优化
  * 通用图形分析系统:
    *  GraphLab [35], Graph [30], Gemini [61], Pregel [36], GridGraph [62], XStream [42], and Ligra [46]
    *  **分布式**系统专注于优化通信[45]、局部性[17]和负载平衡[27]，而**单机**系统则大量优化I/O调度[57]、最小化数据负载[52]或以精度换取性能[28]
  * 子图匹配系统:
    * **单机**子图匹配系统侧重于优化匹配顺序和避免冗余计算
      * Turboiso[21]使用查询图的生成树来加速匹配，但它列举了大量的假阳性匹配，并且无法强制执行高性能的匹配顺序
      * Han等人[20]提出了自适应匹配顺序，以通过失败集来提高性能和修剪，从而减少冗余
      * Sun等人[48]设计了LIGHT系统，该系统延迟了图案顶点的物化，并将候选集计算转换为寻找最小集覆盖以消除冗余
    * **分布式**子图匹配系统努力优化用于匹配的模式分解[6，31，41，44，49]，最小化机器间通信[19，54]，或改善负载平衡[8]
    * 几乎所有的子图匹配系统都使用Grochow和Kellis[18]提出的**对称破缺技术**
    * GraphZero总结。。。。。。。。。。。。。。。。。
  * 图形挖掘系统：
    * Arabesque[50]是第一个支持高级界面的分布式图形挖掘系统，用户可以方便地指定和挖掘模式。
    * 然后，提出了具有优化内存消耗[G-miner 11]、深度优先搜索[Fractal:14]、核外处理[Rstream:53，Kaleido:59]或近似挖掘支持[ASAP:25，Approxg:38]的多个图挖掘系统
    * 如[AutoMine 37]所指出的，这些系统实现了通用但效率低下的挖掘算法，并导致了不必要的全局同步
      * AutoMine是一个基于集合表示的独特的图形挖掘系统
      * EmptyHeaded[3]和GraphFlow[26]是类似的系统，但它们专注于优化集合交叉操作，无法处理模式中的缺失边
  * 为图形处理优化编译器：
    * GraphIt[58]使用户能够用算法语言描述图形算法，以及如何用调度语言优化算法。这种分离允许用户专注于算法设计，并将优化任务分配给编译器。
    * Green Marl[23]、SociaLite[43]和Abelian[16]可以自动并行化和优化图算法，但与GraphIt相比，它们可以探索的优化空间要小得多
    * Pai和Pingali[40]提出了一套编译器优化技术，以有效地将图形算法映射到GPU架构
  * 方向优化
    * 

* **Pangolin**: An Efficient and Flexible Graph Mining System on CPU and GPU.

  * VLDB 20  5
  * GPM应用：..............................
  * GPM框架：
    * Fractal
    * AutoMine
  * 近似GPM：
    * ASAP

* **Sandslash**: A Two-Level Framework for Efficient Graph Pattern Mining

  * ISCA 21
  * 对比系统：AutoMine、Pangolin、Peregrine
  * [Pangolin: 13，Peregrine: 31，GraphZero: 40，AutoMine:41，Arabesque:57，RStream:59，Kaleido: 64]
  * Low-level GPM Systems： 
    * BFS
      * Arabesque[57]是一个**分布式**GPM系统，使用以嵌入为中心(模式遗忘)的编程范式。
      * RStream[59]是一个**单台机器**上的核心外GPM系统，使用基于关系代数的模型。
      * Kaleido[64]是一个使用压缩稀疏嵌入（CSE）格式来减少内存消耗的**单机系统**。
      * G-Miner[12]是一个使用任务并行处理的**分布式**GPM系统。
      * Pangolin[13]是一个针对CPU和GPU的共享内存GPM系统。
    * DFS
      * Fractal[20]使用DFS枚举分布式平台上的子图
  * High-level GPM Systems:
    * BFS
    * DFS
      * AutoMine[41]是一个基于DFS的系统，目标是单个机器。它提供了高级编程接口，并使用编译器生成高性能GPM程序。
      * GraphZero[40]通过引入对称破坏来避免过度计数，从而改进了AutoMine
      * GraphPi[50]通过更好的冗余消除性能模型进一步改进了GraphZero
      * Peregrine是最先进的高级GPM系统
  * GPM 算法......................................

* **FINGERS**: exploiting fine-grained parallelism in graph mining accelerators

  * ASPLOS 22  2月
  * 对比系统：FlexMiner
  * 在FlexMiner基础上该改进
  * 通用图挖掘系统
    * Arabesque [53], G-Miner [7], G-thinker（2020） [56], RStream [55], Fractal [16], AutoMine [39], GraphZero [38], GraphPi [50], Pangolin [10], Peregrine [29], Sandslash [9], DwarvesGraph [8], and aDFS [54].
    * 模式遗忘(pattern-oblivious(embedding-centric))系统：[G-Miner 7，Pangolin 10，Fractal 16，Arabesque 、aDFS 、RStream 、G-thinker53–56]
      * 直接通过深度优先和广度优先进行搜索，搜索过程中剪枝，叶子节点同构检测，没有一开始就会模式图分析生成执行计划
    * 模式感知(pattern-aware（set-centric）)系统：[Sandslash 9,  Peregrine  29, GraphZero 38, AutoMine  39, GraphPi  50]
      * 利用模式的结构来生成有效的执行计划，计划包括将每个新顶点添加到候选嵌入中的顺序和条件
  * 硬件加速器
    * 模式遗忘
      * Gramer[57]是最早的图形挖掘加速器之一，它使用专门设计的缓存来利用局部性并将频繁访问的数据固定在芯片上, 然而，Gramer遵循了次优模式遗忘范式，与模式感知算法相比，巨大的性能差距无法通过硬件加速来弥补，这使得Gramer甚至比AutoMine等基于软件的系统更慢[39] ，但基于模式遗忘，比AutoMine速度还慢
    * 模式感知
      * TrieJax[30]使用最坏情况下的最优连接算法来执行集合交集，但由于缺乏集合减法支持，它无法挖掘顶点诱导的子图
      * IntersectX[47]通过流指令集扩展增强了传统的通用处理器，以实现高效的集操作。它添加了一个特殊的硬件单元，将两个输入集作为流经数字比较器的流，用于集的交集和减法
      * SISA[4]还设计了一个特殊的指令集，用于内存处理（PIM）架构上的设置操作。
      * FlexMiner[11]作为一种最先进的加速器设计，采用了多个硬件处理元件（PE）来利用图挖掘中的大量粗粒度并行性。

* **NDMiner**: Accelerating Graph Pattern Mining Using Near Data Processing

  * ISCA 22  6月
  * 对比系统：GraphPi、Pangolin、FlexMiner
  * 软件加速： 同构检测在扩展中进行，减少中间结果集的内存消耗
  * 硬件加速：
    * NDP框架： 降低在带宽有限且耗能巨大的CPU内存总线上传输数据的成本
      * OMEGA[1]和PHI[35]使用低成本的计算单元来增加CPU内存，用于图形处理
    * 图形计算转移到HMC的逻辑层： *该硬件加速方式只适用于图形处理，不能直接应用于GPM加速*
      * A scalable processing-in-memory accelerator for parallel graph processing、GaphVine、GraphH、GraphP、Graphq
    * SISA[8]建议通过提出以集合为中心的ISA和快速集合交集算法来减轻现有PIM架构上的计算负担
    * NDMiner使用**特定于域(Domain-specific)**的优化（硬件负载消除、编译器优化和设置操作重新排序）**改进了SISA**
  * **特定于域(Domain-specific)**加速器
    * ExTensor[21]为张量代数使用了快速交集电路，由于它不支持模式枚举等关键操作，因此无法用于GPM
    * 许多图形处理加速器旨在通过内存系统优化来改善不规则的内存访问[3，5，20，33，34，37，49，54，59]，图形处理和GPM工作负载具有不同的内存访问模式。因此，当应用于GPM时，图形处理加速器的有效性可能会受到限制
    * 最近的工作**[Gramer 60、IntersectX 43、TrieJax 24、FlexMiner 12]**为GPM设计了专门的架构
      * FlexMiner[12]通过提出一种模式感知GPM加速器，改进了现有加速器[24，60]的性能和通用性
      * DMiner通过采用特定于域的NDP架构，包括新颖的优化，如循环嵌套扁平化和设置操作重新排序  优于FlexMiner
      * IntersectX[43]通过扩展ISA和架构支持，优化CPU上的GPM执行。然而，这种方法存在高片上存储要求和来自DRAM的不必要片外数据传输的问题。NDMiner将计算卸载到低成本的NDP单元，并进行了特定领域的优化。

* **Efficient and Scalable Graph Pattern Mining on GPUs.**

  * OSDI 22  7月

  * 对比系统：
    * 基于CPU: GraphZero、Peregrine
    * 基于GPU: Pangolin、PBE[42、43]

  * G2Miner，这是第一个在多个GPU上高效运行的GPM框架
  * 广度优先系统：[G-miner 20、Pangolin 26、Arabesque: 101、Rstream: 107、Kaleido: 120]
    * 当它们进行逐级子图扩展时，它们生成大量的中间数据，因此仅限于小型图和模式
  * 深度优先系统(基于CPU)：[Sandslash: 25、Fractal:  33、Peregrine: 53、Automine: 74]
    * 支持更大的数据集
  * Pangolin 是目前唯一支持GPU的GPM系统。然而，受BFS顺序的限制，穿山甲只能处理较小的输入图，并且缺乏模式感知
  * 子图匹配系统：只支持GPM问题的一个子集，不是通用的，通常不可编程
    * CPU[Emptyheaded(2017) 2, Graph-flow(2017) 55，Graphzero(2021)73，Graphpi 93，aDFS(2021) 104]
      * Graphzero(文中描述是子图匹配)、Graphpi(文中描述是GPM) 不包含频繁子图的挖掘  
    * GPU[Gpu-accelerated subgraph enumeration on partitioned graphs(SIGMOD ’20) 42， Exploiting reuse for gpu subgraph enumeration(2020) 43，Gsi: Gpu-friendly subgraph isomorphism(2020) 117]