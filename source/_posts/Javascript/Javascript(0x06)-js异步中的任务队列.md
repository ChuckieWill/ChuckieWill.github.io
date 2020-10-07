---
title: Javascript(0x06)-js异步中的任务队列
date: 2020-10-02 18:25:21
tags:
- Javascript
- 异步编程
- 任务队列
- 事件循环
categories:
- [Javascript]
---



##  事件循环与任务队列

###  1 事件处理流程

1. 代码从上往下执行，每段代码先放入**任务队列**中
2. **js引擎**从任务队列中提取代码执行
3. 当js引擎执行遇到异步编程时（DOM、ajax、setTimeout）,将这些异步操作丢给**WebAPI**执行
4. WebAPI执行完异步操作的回调函数继续放入**任务队列**
5. js引擎一直从任务队列中提取代码执行，**回调函数执行的顺序由加入队列的顺序决定**

![image-20201002182822386](Javascript(0x06)-js异步中的任务队列/image-20201002182822386.png)

###  2 异步任务队列相关面试题

####  2.1 问题1

**问题：**

以下代码执行结果输出顺序

```js
setTimeout(() => console.log(1), 0)
console.log(2)
```

**答案：**

```js
2 ,1
```

**解析：**

1. `setTimeout(() => console.log(1), 0)`加入任务队列
2. `console.log(2)`加入任务队列
3. js引擎执行任务队列里的代码：`setTimeout(() => console.log(1), 0)`，`setTimeout()`是异步操作，丢给WebAPI执行
4. js引擎执行任务队列里的代码：`console.log(2)`，输出2
5. WebAPI执行完异步操作，将回调函数`() => console.log(1)`加入任务队列
6. js引擎执行任务队列里的代码：`() => console.log(1)`，输出1

