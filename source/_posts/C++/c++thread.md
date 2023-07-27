---
 
title: 多线程(c/c++)和线程池
date: 2022-01-26 14:43:17
tags:
- 多线程
- 线程池
categories:
- [C++]


---

#  thread

##  c多线程

###  c线程基本使用

> https://subingwen.cn/linux/thread/

创建线程

```c
#include <pthread.h>
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);
```

退出线程

> 默认情况下主线程退出后，子线程也就结束了，无论子线程任务是否执行完，因为进程资源被释放了

* 退出线程传出的参数，通过pthread_join()函数回收时可以获取返回的参数
* 线程必须通过这种方式才能返回参数，且需要通过pthread_join才能获取返回的参数
* 子线程调用pthread_exit返回参数后，就释放了子线程栈区资源
  * 所以返回的参数不能是子线程的局部变量
* 实现子线程返回数据
  * 可以通过pthread_create将主线程的需要的数据通过传输**传入参数**传入
  * 也可以定义全局变量，子线程修改全局变量即可
  * 也可以子线程定义堆区数据，返回堆区的指针

```c
void pthread_exit(void *retval);
```

线程回收

* retval接收子线程通过pthread_exit返回的数据

```c
int pthread_join(pthread_t thread, void **retval);
```

线程分离

* 默认情况下主线程退出后，子线程也就结束了，无论子线程任务是否执行完，因为进程资源被释放了
* 线程分离后，只是主线程不负责回收子线程了，但是主线程结束时，子线程也会随之结束
* 要实现线程分离后，主线程结束后子线程仍然可以正常执行，则需要在主线程中调用` pthread_exit(NULL);`
  * 即主线程自己退出，但是进程资源不会释放

```c
int pthread_detach(pthread_t thread);
```

###  线程同步

> https://subingwen.cn/linux/thread-sync/



##  c++多线程

###  c++线程基本使用

> https://subingwen.cn/cpp/thread/

## 无锁队列

> github: https://github.com/gongyiling/cpp_lecture/tree/91ec4233a8b8175db94f7430604ace82fa51cefb/lockfree
>
> bilibili: https://www.bilibili.com/video/BV1sy4y1t73N/?spm_id_from=333.337.search-card.all.click&vd_source=7230a052308bbb41976f248d2c778e3a

##  线程池

难点

* waitalldone
  * 在worker函数中需要判断同时`shutdown = false && queue.size() == 0 `时，线程才阻塞
    * 如果队列中还有任务，即使`shutdown = true`也不能发生关闭，因为线程不会进入阻塞，还会继续往下执行
    * 只有队列中任务为空了，且`shutdown = false`才会阻塞，这种情况在`shutdown = true`时，会唤醒线程，线程执行到下面的判断就直接退出了
      * 也就说被唤醒退出的情况，只发生在`shutdown = true`之前就已经因为队列为空阻塞了
      * 若`shutdown = true`之前队列还不为空，这些线程将永远进入不了阻塞，因为`shutdown != false`，对于这种情况会一直执行任务队列中的任务，直到任务执行完成，执行到下面的判断，然后退出
  * 在worker函数中需要判断同时`shutdown = true && queue.size() == 0 `时线程才退出
  * waitdone中
    * 标记线程池退出，如果已经标记则直接return(说明之前已经唤醒所有线程并等待所有线程退出)
    * 阻塞回收线程
  * 构造线程池，添加任务
  * 析构函数， 调用waitdone， 然后释放资源
* lambda表达式作为任务函数
  * 将任务函数的类型定义为可调用函数

```c++
#include <functional>
using callback = function<unsigned long long(void*)>; // unsigned long long是返回值类型，void*是函数参数
struct Task
{
  callback func;
  void* arg;
  Task() : func(nullptr), arg(nullptr) {}
  Task(callback f, void* arg) : func(f), arg(arg) {}
};
```

* 任务函数的返回值求和
  * 线程池中定义一个原子变量
  * 任务函数的返回值累加到原子变量上
  * 通过`pool->getRet()`获取原子变量的累加结果
    * getRet()中需要先等待所有任务执行完(调用waitalldone())，再返回

#  协程

> **协程是一种函数对象，可以设置锚点做暂停，然后再该锚点恢复继续运行**
>
> 一个特殊的函数——可以在某个地方挂起，并且可以重新在挂起处继续运行
>
> 协程就是可暂停和恢复的函数
>
> [使用场景](https://zhuanlan.zhihu.com/p/446797833)
>
> [协程实现ovoice-github](https://github.com/wangbojing/NtyCo)
>
> 协程池github : https://github.com/fengyoulin/ef    对应视频：https://www.bilibili.com/video/BV1a5411b7aZ/?spm_id_from=333.788.recommend_more_video.0&vd_source=7230a052308bbb41976f248d2c778e3a
>
> 