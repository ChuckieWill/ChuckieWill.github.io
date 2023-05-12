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