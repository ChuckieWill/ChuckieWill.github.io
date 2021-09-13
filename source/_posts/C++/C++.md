---
title: C++
date: 2021-07-06 10:43:17
tags:
- C++
categories:
- [C++]
---



 

 ##  基础知识点

####  编译、链接的过程

![image-20210816181241344](C++/image-20210816181241344.png)

####  数据类型

* 1byte = 8bits
* short类型  2byte    存储范围[-2^15, 2^15-1]  [ -32768 , 32767]
* int 类型      4byte

![image-20210907142129928](C++/image-20210907142129928.png)



####  安装IDE

> 一下以vs2015为例

* 启动

![image-20210907143322981](C++/image-20210907143322981.png)

* 创建项目

![image-20210907143514046](C++/image-20210907143514046.png)

* 编辑选项

![image-20210907143604498](C++/image-20210907143604498.png)

* 生成选项

![image-20210907143709667](C++/image-20210907143709667.png)

* 项目配置选项

![image-20210907143823901](C++/image-20210907143823901.png)

* 调试选项

![image-20210907143933016](C++/image-20210907143933016.png)

* 如何注册

![image-20210907144051673](C++/image-20210907144051673.png)

![image-20210907144120057](C++/image-20210907144120057.png)

![image-20210907144140147](C++/image-20210907144140147.png)

![image-20210907144220778](C++/image-20210907144220778.png)

####  vs2019快捷键

[vs2019快捷键](https://blog.csdn.net/qq_41979507/article/details/106188486)

##  基础容器

###  数组

* 计算数组长度

```c++
int a[] = {1,2,3,4};
int len = sizeof(a) / szieof(a[0]);   //sizeof是c++自带函数 sizeof计算的是字节（byte）数，int是4字节，所以sizeof(a)：16  szieof(a[0]：4
//数组名:a指向数组第一个元素，不可更改
```

####  vector

> vector是面向对象方式的动态数组
>
> 可以实现动态扩容插入元素

* `vec.push_back()`：在尾部添加元素
* `vec.capacity()`: 获取数组课容纳的元素个数， 会随着扩容发生变化
* `vec.size()`: 获取数组当前元素个数
* `vec.end()`: 获取数组结束位置的下标（即数组长度）
* `vec.insert(插入的位置, 插入的元素)`: 在中间位置插入元素
* `vec.pop_back()`: 删除最后一个元素
* `vec.erase(vec.end()-2)`: 删除倒数第二个元素

```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{   
    vector <int> vec = { 1,2,3,4 };
    vec.push_back(5);
    cout << "capacity:" << vec.capacity() << endl;
    for (int i = 0; i < vec.size(); i++) 
    {
        cout << vec[i] << endl;
    }
    vec.insert(--vec.end(), 6)//在倒数第二个位置插入元素
    return 0;
}
```



###  字符串

* 字符串本质是字符数组
  * `''`单引号是字符
  * `""`双引号是字符串

```c++
char c1 = 0;   //地址：Ox00
char c2 = '\0';//地址：Ox00
char c3 = '0'; //地址：0x30
char item[] = {"helloword"}
```

* [ASCII](https://baike.baidu.com/item/ASCII/309296?fr=aladdin)

####  字符串的指针

```c++
char strHelloWorld1[11]= { "helloworld" }; //这里的"helloworld"是变量
//strHelloWorld 不可变，strHelloWorld[index]的值可变;
char* pStrHelloWrold = "helloworld"; //这里的"helloworld"是常量
//pStrHelloWrold可变，但是pStrHelloWrold[index]的值可变不可变取决于所指区间的存储区域是否可变;
pStrHelloWrold = strHelloWorld1//可行
pStrHelloWrold[index] = 'w'//修改指针指向的位置的值，通常不可行
```

####  字符串string

> [c标准库`<string.h>`](https://www.runoob.com/cprogramming/c-standard-library-string-h.html)

* `strlen()`：返回字符有效长度，不包括'\0'
* `strcmp(s1,s2)`: 以ASCII比较字符串大小，例：`"A"<"B ";"A"<"AB";"Apple" <"Banana"`
  * 如果s1和s2是相同的，则返回0
  * 如果s1<s2则返回值小于0;
  * 如果s1>s2则返回值大于0
* `strcpy(s1,s2)`: 将s2复制到s1中
* 复制指定长度字符串:`strncpy(s1, s2,n)`
  * 将字符串s2中前n个字符拷贝到s1的前n个位置中;
* 字符串拼接:`strcat(s1, s2)`
  * 将字符串s2接到s1后面;
* 查找字符: `strchr(s1, ch)`
  * 指向字符串s1中字符ch的第一次出现的位置;
* 查找字符串: `strstr(s1, s2)`
  * 指向字符串s1中字符串s2的第一次出现的位置;

```c++
#include <iostream>
#include <string.h>

using namespace std;

int main()
{
    int b[] = { 1,2,3,4 };
    char a[] = { "heloworld" };
    cout << "sizeof(b):" << sizeof(b) << endl;// 16
    cout << "strlen:" << strlen(a) << endl;   // 10 计算的是有效长度
    cout << "sizeof(a):" << sizeof(a) << endl;// 11 有效长度加尾部的'\0'

    return 0;
}
```

```c++
#include <iostream>
#include <string.h>

using namespace std;

const int MAX_LEN = 30;

int main()
{
    char a[] = { "hello" };
    char b[] = { "world" };
    char c[MAX_LEN] = { 0 };
    cout << "a:" << a << endl;//a:hello
    cout << "b:" << b << endl;//b:world
    cout << "c:" << c << endl;//c:
    strcpy(c, a);
    cout << "strcpy:c:" << c << endl;//strcpy:c:hello
    strncpy(c, b, 2);
    cout << "strncpy:c:" << c << endl;//strncpy:c:wollo
    strcat(c, b);
    cout << "strcat:c:" << c << endl;//strcat:c:wolloworld
    return 0;
}
```



**PS：**

* 在使用上面的`<string.h>`中的函数时，需要在编译器（vs2019）中做设置
* 项目根目录右击-点击属性-选择c/c++-选择预处理-选择右侧预处理器定义-点击编辑-在编辑内容的最下面添加`_CRT_SECURE_NO_WARNINGS`
* 以上配置可以避免警告提示



##  多线程

**为什么要用多线程**

* 任务分解
  * 耗时的操作，任务分解，实时响应
* 数据分解
  * 充分利用多核CPU处理数据
* 数据流分解
  * 读写分离，解耦合设计

###  基本概念

* 并行： 多核
* 并发： 单核
* 可执行程序
  * windows: 扩展名为.exe的文件
  * Linux: ls -la  后缀为rwx（x执行权限）
* 进程： 运行起来的可执行程序
* 线程
  * 每个进程（执行起来的可执行程序），都有一个主线程，这个主线程是唯一的。
    * 当执行可执行程序，产生一个进程后，这个主线程就随着这个进程的启动起来了。
    * ctrl+f5运行这个程序的时候，实际上是进程的主线程来执行（调用)这个main函数中的代码。
  * 线程:用来执行代码的;
    * 线程可以理解成一条代码的执行通路(道路）;
    * 除了主线程之外，可以通过自己写代码来创建其他线程，其他线程走的是别的道路，甚至去不同的地方
    * 每创建一个新线程，可以在同一个时刻，多干一个不同的事(多走一条不同的代码执行路径）;
    * 多线程(并发),线程并不是越多越好
      * 每个线程，都需要一个独立的堆栈空间（1M)，线程之间的切换要保存很多中间状态;
      * 切换会耗费本该属于程序运行的时间;
      * 建议不要超过200-300个

**并发的实现方法**

* 多个进程实现并发
  * 进程之间通信
    * 同一个电脑上:管道，文件，消息对列，共享内存) ; 
    * 不同电脑上:socket通信技术;
* 在单个进程中，创建多个线程实现并发
  * 线程象轻量级的进程。每个线程都有自己独立的运行路径，但是一个进程中的所有线程共享地址空间(共享内存)
    * 全局变量，指针，引用都可以在线程之间传递，
    * 使用多线程开销远远小于多进程。
    * 共享内存带来新问题，数据一致性问题。

###  基本使用

####  c++11多线程API

* thread
* join
* detach
* joinable

````c++
// demo2.cpp : 此文件包含 "main" 函数。程序执行将在此处开始并结束。
//

#include <iostream>
#include <thread>

using namespace std;

// 一、用函数作为可调用对象创建线程

void myprint()
{
    cout << "我的线程开始执行了" << endl;
    cout << "我的线程执行完毕了1" << endl;
    cout << "我的线程执行完毕了2" << endl;
    cout << "我的线程执行完毕了3" << endl;
    cout << "我的线程执行完毕了4" << endl;
    cout << "我的线程执行完毕了5" << endl;
    cout << "我的线程执行完毕了6" << endl;
    cout << "我的线程执行完毕了7" << endl;
    cout << "我的线程执行完毕了8" << endl;
    cout << "我的线程执行完毕了9" << endl;

}

int main()
{
    cout << "Hello World!\n";
    // 1 thread: 创建子线程并执行子线程
    thread myobj(myprint);
    // 2 joinable: 判断join和detach是否用过，用过则为false，没用则为true
    if (myobj.joinable())
    {
        cout << "1joinable===true" << endl;
    }
    else
    {
        cout << "1joinable===false" << endl;
    }
    // 3 join: 阻塞主线程，并等待子线程执行完成
    myobj.join();
    // 4 detach: 主线程不等子线程，主线程和子线程分开执行，子线程驻留后台运行
    //此时子线程相当于被C++时库接管，当子线程执行完成后，由运行时库负责清理该线程相关资源
    //myobj.detach();
    
    if (myobj.joinable())
    {
        cout << "2joinable===true" << endl;
    }
    else
    {
        cout << "2joinable===false" << endl;
    }
    cout << "I love China" << endl;
    cout << "I love China1" << endl;
    cout << "I love China2" << endl;
    cout << "I love China3" << endl;
    cout << "I love China4" << endl;
    cout << "I love China5" << endl;
    cout << "I love China6" << endl;
    cout << "I love China7" << endl;

    return 0;
}

// 二、其它创建线程的手法

// 2.1 用类对象创建线程

class TA
{
public:
	int& m_i;
	TA(int& i) :m_i(i)
	{
		cout << "TA()构造函数被执行" << endl;
	}
	TA(const TA& ta) :m_i(ta.m_i)
	{
		cout << "TA()拷贝构造函数被执行" << endl;
	}
	~TA()
	{
		cout << "~TA()析构函数被执行" << endl;
	}
	void operator()()
	{
		cout << "我的线程operator()开始执行" << endl;
		cout << "我的线程operator()执行结束" << endl;

	}
};

int main()
{
	int myi = 6;
	TA ta(myi);
	//注意使用detach时，ta是复制到子线程中的，所以即使主线程销毁，
	//子线程中的ta也不会销毁，不会影响子线程的执行
	thread myobj(ta);
    //thread myobj(std::ref(ta));//传入真引用时，下面就不能用detach()了
	myobj.join();
	// 注意使用detach时，传入ta的参数不能是指向主线程中变量的引用或者指针，
	// 因为主线程先执行完时，引用和指针指向的变量就销毁了
	// 所以容易造成错误
	//myobj.detach();
	cout << "I love China" << endl;
	return 0;
}

// 2.2 用lambda表达式创建线程

auto mylamthrad = [] {
	cout << "我的线程mylamthrad()开始执行" << endl;
	cout << "我的线程mylamthrad()执行结束" << endl;
};

int main()
{
	thread myobj(mylamthrad);
	//myobj.join();
	myobj.detach();
	cout << "I love China" << endl;
	return 0;
}
````

####  线程传参详解，detach()大坑

#####  引用和指针做参数

* 引用： 虽然是引用，但是其实还是值传递，主线程的销毁不影响子线程（不推荐使用）
* 指针：指向主线程的地址，主线程的销毁影响子线程 （不能使用指针做线程参数）

```c++
#include <iostream>
#include <thread>

using namespace std;


void myprint(const int &i, char *pmybuf)
{
    cout << "子线程" << endl;
    cout << i << endl;//此时的引用i并不是指向主线程的中的mvar，而是值传递过来的，即使主线程执行结束，也不影响子线程的执行
    cout << pmybuf << endl;//此时指针pmybuf指向主线程中的mybuf, 主线程销毁则会影响子线程
    return;

}

int main()
{
    int mvar = 1;
    int& mvary = mvar;
    char mybuf[] = "this is a test!";
    thread mytobj(myprint, mvar, mybuf);
    //mytobj.join();
    mytobj.detach();
    cout << "I love China!" << endl;

    return 0;
}

```

#####  传递临时对象作为线程参数

* 值类型传参，直接用值传递，不用引用
* 对象作为参数传递，不要用指针，而是用对象传递，而且有隐式类型转换时，在传递时要**先构造临时对象**
  * 如果直接传入对象，可能在主线程销毁前，对象还没有传入子线程完成构造
  * 先构造临时对象再传入线程，则可以保证在主线程销毁前，对象已经传入子线程
    * 子线程通过引用接临时对象，如果不用引用则会多拷贝一次，浪费资源

```c++

#include <iostream>
#include <thread>

using namespace std;


void myprint(const int i, const string &pmybuf)
{
    cout << "子线程" << endl;
    cout << i << endl;//此时的引用i并不是指向主线程的中的mvar，而是值传递过来的，即使主线程执行结束，也不影响子线程的执行
    cout << pmybuf << endl;//此时指针pmybuf指向主线程中的mybuf, 主线程销毁则会影响子线程
    return;

}

int main()
{
    int mvar = 1;
    int& mvary = mvar;
    char mybuf[] = "this is a test!";
    //string(mybuf): 在传递参数时构造临时对象, 这样可以确保，在主函数销毁前，就完成了临时对象的构造
    thread mytobj(myprint, mvar, string(mybuf));//char转成string属于隐式类型转换
    //mytobj.join();
    mytobj.detach();
    cout << "I love China!" << endl;

    return 0;
}


```

#####  线程ID

```c++
std::this_thread::get_id();
cout << "子线程id:" << std::this_thread::get_id()<<endl;
```

####  传递类对象作为线程参数

```c++
#include <iostream>
#include <thread>

using namespace std;


class A
{
public:
    mutable int m_i;// const A &pmybuf 因为引用时const的，所以在这里要设置为可修改
    A(int a) :m_i(a) { cout << "[A::A(int a)构造函数执行]" <<"thread id:"<<std::this_thread::get_id() << endl; }
    A(const A &a) :m_i(a.m_i) { cout << "[A::A(const A)拷贝构造函数执行]" << "thread id:" << std::this_thread::get_id() << endl; }
    ~A(){ cout << "[A::~A()析构函数执行]" << "thread id:" << std::this_thread::get_id() << endl; }

};

void myprint(const A &pmybuf)//注意接受要是引用，避免多次拷贝
{   //虽然此处是pmybuf是引用，但是它并不指向主线中的对象，而是从主线程拷贝到子线程中的对象
    pmybuf.m_i = 199;//所以此处的修改，并不会影响主线的数据，不会有联动的效果
    cout << "子线程id:" << std::this_thread::get_id()<<endl;
    return;

}

int main()
{
    A myobj(10);
    //传递对象作为线程参数
    thread mytobj(myprint, myobj);
    mytobj.join();
    //mytobj.detach();
    cout << "I love China!" << endl;

    return 0;
}
```

#####  std::ref()

* 不用std::ref()时，对象作为参数传入线程时，虽然是用引用接受对象，但是这个引用并不是真正的引用，通过这个引用在子线程中修改数据后，主线程并不会随之修改，所以并没有真正体现引用的所用
* 使用std::ref()后，可以使得子线程中的引用是指向主线程中的对象，是真正的引用，在子线程或主线程修改数据后可以实现联动。

```c++
#include <iostream>
#include <thread>

using namespace std;


class A
{
public:
    int m_i;// 可以去掉mutable
    A(int a) :m_i(a) { cout << "[A::A(int a)构造函数执行]" <<"thread id:"<<std::this_thread::get_id() << endl; }
    A(const A &a) :m_i(a.m_i) { cout << "[A::A(const A)拷贝构造函数执行]" << "thread id:" << std::this_thread::get_id() << endl; }
    ~A(){ cout << "[A::~A()析构函数执行]" << "thread id:" << std::this_thread::get_id() << endl; }

};

void myprint( A &pmybuf)//可以去掉const
{    pmybuf.m_i = 199;
    cout << "子线程id:" << std::this_thread::get_id()<<endl;
    return;

}

int main()
{
    A myobj(10);//子线程修改为199后，这里也会修改为199
    thread mytobj(myprint, std::ref(myobj));//使用std::ref
    mytobj.join();
    //mytobj.detach();
    cout << "I love China!" << endl;

    return 0;
}
```





####  传递智能指针作为线程参数

```c++
#include <iostream>
#include <thread>

using namespace std;

void myprint(unique_ptr<int> pzn)
{   //pzn和myp指向同一片地址
    //所以主线程销毁，pzn指向的地址就回收了，就会出问题
    //所以在使用智能指针作为线程参数时，不能使用detach()

}

int main()
{   
    unique_ptr<int> myp(new int(100));//创建整型独占是指针
    thread mytobj(myprint, std::move(myp));
    mytobj.join();
    //mytobj.detach();//不能用detach
    cout << "I love China!" << endl;

    return 0;
}

```



0x00a50580

####  成员函数做线程函数

* 使用

```c++
//thread mytobj(&类名::成员函数名,实例对象, 成员函数参数);
thread mytobj(&A::thread_work,myobj, 15);
```

* 案例

```c++
#include <iostream>
#include <thread>

using namespace std;

class A
{
public:
    int m_i;// 可以去掉mutable
    A(int a) :m_i(a) { cout << "[A::A(int a)构造函数执行]" << this << " thread id:"<<std::this_thread::get_id() << endl; }
    A(const A &a) :m_i(a.m_i) { cout << "[A::A(const A)拷贝构造函数执行]" << this << " thread id:" << std::this_thread::get_id() << endl; }
    ~A(){ cout << "[A::~A()析构函数执行]" << this << " thread id:" << std::this_thread::get_id() << endl; }

    void thread_work(int num)
    {
        cout << "子线程thread_work执行" << this << " thread id:" << std::this_thread::get_id() << endl;
    }
};



int main()
{   
    A myobj(10);
    //thread mytobj(&类名::成员函数名,实例对象, 成员函数参数);
    thread mytobj(&A::thread_work,myobj, 15);
    mytobj.join();
    //mytobj.detach();//此时传入的myobj会调用拷贝构造函数，即子线程会构造一个属于自己的对象，所以此时可以用detach
    cout << "I love China!" << endl;

    return 0;
}


```

* 不能用detach（）的情况
  * 使用了std::ref()，则子线程不会拷贝对象，而是使用主线程的对象，所以主线程销毁则不能正常访问了

```c++
int main()
{   
    A myobj(10);
    thread mytobj(&A::thread_work, &myobj, 15);//&myobj 等价于 std::ref(myobj)
    mytobj.join();
    cout << "I love China!" << endl;

    return 0;
}
```

