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

1. 代码从上往下执行，一行一行放在**js引擎**的 Stack（栈）中 ，若是同步代码则直接执行
2. 当js引擎执行遇到异步代码时（DOM、ajax、setTimeout）,将这些异步操作丢给**WebAPI**执行
3. WebAPI执行完异步操作的回调函数放入**任务队列**
4. 如Stack为空（即同步代码执行完）**Event Loop**开始工作
5. **Event Loop**轮询查找Callback Queue ，如有回调函数则移动到Stack执行，**回调函数执行的顺序由加入队列的顺序决定**
6. 然后**Event Loop**继续轮询查找(永动机一样)

![image-20201002182822386](Javascript(0x06)-js异步中的任务队列/image-20201002182822386.png)

###  2 宏任务和微任务执行流程

1. 代码在Call  Stack中执行，遇到同步代码则直接执行，遇到遇到微任务则加入micro task queue ,遇到宏任务则交给我Web APIs 处理，Web APIs处理完成后将回调函数加入宏任务队列（Callback Queue）
2. 代码都执行完，Call Stack空闲后，从micro task queue 提取微任务的回调函数执行
3. Call Stack再次空闲，且micro task queue也为空后，开始DOM渲染
4. DOM渲染完后，触发Event Loop ,从宏任务队列提取回调函数执行



![image-20210120183029005](Javascript(0x06)-js异步中的任务队列/image-20210120183029005.png)

###  3 异步任务队列相关面试题

####  3.1 问题1

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

