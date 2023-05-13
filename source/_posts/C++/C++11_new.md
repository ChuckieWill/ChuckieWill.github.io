---
title: C++11新特性
date: 2022-08-06 10:43:17
tags:
- C++11
categories:
- [C++]
---

#  c++11_new

> [爱编程的大丙B站-c++11学习笔记](https://blog.csdn.net/Jiangtagong/article/details/119698713)
>
> [爱编程的大丙个人博客-c++11](https://subingwen.cn/cplusplus/)

###  原始字面量

* 使用场景
  * windows下的路径
  * 换行字符串

```c++
string str1 = R"(D:\hello\world\test.text)";
```

###  nullptr

* C++ 程序 NULL 就是 0
* C 程序 NULL 表示 `(void*)0`
* C++ 中，`void * `类型无法隐式转换为其他类型的指针
* nullptr 无法隐式转换为整形，但是可以隐式匹配指针类型

###  constexpr

* const 语义
  * 变量只读，起始也是可变的，不能视为常量，不是在编译阶段计算
  * 修饰常量，编译阶段计算

* 常量表达式的计算往往发生在程序的**编译阶段**
* 修饰函数
  * 整个函数的函数体中，不能出现非常量表达式之外的语句（using 指令、typedef 语句以及 static_assert 断言、return 语句除外）
  * 本质就是函数可以在编译阶段计算出结果，如果函数内部存在需要执行才能计算出结果的情况，则不能定义为常量表达式函数
* 修饰模板函数
  * 根据传入和传出的值判断是否用这个关键字，编译阶段就判断传入的是不是常量
* 修饰构造函数
  * 构造函数体需要为空，且只能通过初始化列表初始化
* 总结：就是告诉编译器可以在编译阶段就计算出结果

###  自动类型推导

* auto
  * 使用场景：
    * 迭代器
    * 泛型编程，返回值类型是根据传入的泛型决定的
* decltype
  * 使用场景：模板类中，如果传入的是STL容器，获取对应容器的迭代器
    * 在定义迭代器的时候不能通过`T::iterator`来定义，因为T是未知的，必须通过一个已知的类型才能定义一个变量
* 返回值类型后置
  * 使用decltype推导返回结果的数据类型

###  final、override

* final
  * 注意修饰位置在函数和类后面
  * 修饰函数表示，这个方法不能在在子类中被重写了
  * 修饰类表示，这个类不能被继承
* override
  * 用于确保重写的函数函数名不会出错，写的位置是在重写函数名后面

###   模板优化

* 模板的右尖括号
  * c++11以前，`Base<vector<int>> b;`这种写法的`>>`会被解析为右移

* 默认模板参数
  * 类和函数都可，函数没有传入实参时，会使用默认类型和默认值

```c++
template <typename T = int, typename U = char>
void func(T arg1 = 100, U arg2 = 100)
{
    cout << "arg1: " << arg1 << ", arg2: " << arg2 << endl;
}

int main()
{
    // 模板函数调用
    func('a');
    func(97, 'a');
    func();    // 打印：100， d 
    return 0;
}
```

###  using的使用

> using 语法和 typedef 一样，并不会创建出新的类型，它们只是给某些类型定义了新的别名

* 定义别名
  * using 相较于 typedef 的优势在于定义函数指针别名时看起来更加直观
* 模板的别名
  * using 给模板定义别名更加简洁直观



###  委托构造函数和继承构造函数

* 委托构造函数
  * 在构造函数中可以条用另外的构造函数，以此可精简代码
* 继承构造函数
  * c++11之前继承是没有继承构造函数的，要初始化，只能在子类中再调用父类的构造函数
  * 继承构造函数就是可以允许子类继承父类的构造函数，直接可以初始化父类的成员变量
  * 使用`using Base::Base 、using Base::func`可直接使用父类的构造函数和被隐藏的函数func

###  列表初始化

* 聚合体和非聚合体的初始化
  * 只要有构造函数，都直接用就行
* initializer_list ？？？

###  基于范围的for循环

* 原始for循环可以一遍循环一遍修改循环的内容，因为每次都会对结束判断
* 基于范围的for循环是一开始就确定了循环的范围，所以循环过程中是不能修改循环内容的，但是不用每次都判断是否结束了，效率更高
* set的值和map的key在循环时本身就是不可变的

###  可调用对象 ？？？

* 包装器
* 绑定器

###  Lambda 表达式

###  右值引用

>  **引用本质是别名**
>
> 核心： 用右值引用接收一个将亡值就是给这个将亡值续命，相当于通过这个右值引用可以继续使用这个将亡值，完全继承了将亡值的所有权，这个将网亡值可以是函数的返回值，本来返回后应该就析构了，但是因为有右值引用接收，所以并不会析构，直到右值引用不使用时才析构

####  初始化

* 初始化

  * 右值引用和常量右值引用**只能通过右值**初始化

  * 左值引用不能通过右值初始化
  * 常量左值引用可以通过很多种方式初始化，如下

```c++
#include<iostream>
using namespace std;

int main()
{
  // 左值
  int num = 5;
  int num2 = 6;
  // 左值引用
  int& a = num; // a是num的左值引用
  int& y = 10; // error 10是右值
  // 右值引用
  int&& b = 10;
    // 常量右值引用
  const int && g = 10; // 用右值初始化
  const int && h = g; // 用常量右值引用初始化 error 右值引用只能用右值初始化
  const int && i = b; // 用右值引用初始化 error 右值引用只能用右值初始化
  int && j = b; // 用右值引用初始化 error 右值引用只能用右值初始化
  // 常量左值引用
  const int & x = num; // 用左值初始化
  const int & c = 10;  // 用右值初始化  临时变量也可以 
  const int & e = a;   // 用左值引用初始化
  const int & f = b;   // 用右值引用初始化
  const int & d = c;   // 用常量左值引用初始化
  const int & k = g;   // 用常量右值引用初始化
  return 0;
}

```

* 常量左值引用的理解

```c++
#include<iostream>
using namespace std;

int main()
{
  int num = 5;
  int num2 = 6;
  const int & a = num;  // a是num的常量引用 a不能修改 a的值可以修改,但不能通过a修改num的值，num的值可以通过其他方式修改
  const int & b = a;
  // a = num2; // error 不能修改常量引用的值
  // a = 10; // error 不能修改常量引用的值
  num = 10; // 可以修改a的值
  cout<<a<<endl; // 10
  cout<<b<<endl; // 10
	return 0;
}

```

####  函数返回的临时对象处理

函数返回临时对象，外部接收时，直接是使用了函数内部构造的对象，地址都完全一样，这个过程没有调用拷贝构造和移动构造

* 出现这种请求可能时编译器自动把外部的变量看作了一个右值引用接收了这函数返回的将亡值，续命了将亡值，所以没有拷贝动作

反而是对返回的临时对象，使用move后再构造，会调用移动构造

```c++
#include <iostream>
using namespace std;

class Test
{
public:
    Test() : m_num(new int(100))
    {
        cout << "construct: my name is jerry" << endl;
    }

    Test(const Test& a) : m_num(new int(*a.m_num))
    {
        cout << "copy construct: my name is tom" << endl;
    }

    // 添加移动构造函数
    Test(Test&& a) : m_num(a.m_num)
    {
        a.m_num = nullptr;
        cout << "move construct: my name is sunny" << endl;
    }

    ~Test()
    {
        delete m_num;
        cout << "destruct Test class ..." << endl;
    }

    int* m_num;
};

Test getObj()
{
    Test t;
    cout << "&t: " << &t << endl; 
    cout << "t.m_num: " << t.m_num << endl;
    return t;
}

Test getObj1(){
  return Test();
}

int main()
{
    Test t1 = getObj();   // 和t5用右值接收续命将亡值效果完全一样，不会调用构造函数
    cout << "&t1: " << &t1 << endl; 
    cout << "t1.m_num: " << t1.m_num << endl;
    cout<<"-------------------"<<endl;
    Test t2 = getObj1(); // 和t5用右值接收续命将亡值效果完全一样，不会调用构造函数
    cout << "t2.m_num: " << t2.m_num << endl;
    cout<<"-------------------"<<endl;
    Test t3(getObj()); // 和t5用右值接收续命将亡值效果完全一样，不会调用构造函数
    cout << "&t3: " << &t3 << endl;
    cout << "t3.m_num: " << t3.m_num << endl;
    cout<<"-------------------"<<endl;
    //  使用move函数
    Test t4 = move(getObj()); // move返回的是右值， 通过移动构造函数来构造t4
    cout << "&t4: " << &t4 << endl;
    cout << "t4.m_num: " << t4.m_num << endl;
    cout<<"-------------------"<<endl;
    Test&& t5 = getObj(); 
    cout << "&t5: " << &t5 << endl;
    cout << "t5.m_num: " << t5.m_num << endl;
    cout<<"-------------------"<<endl;
    Test&& t6 = getObj1(); 
    cout << "&t6: " << &t6 << endl;
    cout << "t6.m_num: " << t6.m_num << endl;
    return 0;
};
// 打印
construct: my name is jerry
&t: 0x7fffd883a718
t.m_num: 0x5570d17a7e70
&t1: 0x7fffd883a718
t1.m_num: 0x5570d17a7e70
-------------------
construct: my name is jerry
t2.m_num: 0x5570d17a82a0
-------------------
construct: my name is jerry
&t: 0x7fffd883a728
t.m_num: 0x5570d17a82c0
&t3: 0x7fffd883a728
t3.m_num: 0x5570d17a82c0
-------------------
construct: my name is jerry
&t: 0x7fffd883a740
t.m_num: 0x5570d17a82e0
move construct: my name is sunny
destruct Test class ...
&t4: 0x7fffd883a730
t4.m_num: 0x5570d17a82e0
-------------------
construct: my name is jerry
&t: 0x7fffd883a738
t.m_num: 0x5570d17a8300
&t5: 0x7fffd883a738
t5.m_num: 0x5570d17a8300
-------------------
construct: my name is jerry
&t6: 0x7fffd883a740
t6.m_num: 0x5570d17a8320
destruct Test class ...
destruct Test class ...
destruct Test class ...
destruct Test class ...
destruct Test class ...
destruct Test class ...
```

####  疑问？？？

```c++
#include <iostream>
using namespace std;

class Test
{
public:
    Test() : m_num(new int(100))
    {
        cout << "construct: my name is jerry" << endl;
    }

    Test(const Test& a) : m_num(new int(*a.m_num))
    {
        cout << "copy construct: my name is tom" << endl;
    }

    // 添加移动构造函数
    Test(Test&& a) : m_num(a.m_num)
    {
        a.m_num = nullptr;
        cout << "move construct: my name is sunny" << endl;
    }

    ~Test()
    {
        if(m_num == nullptr){
            cout<<"m_num is nullptr"<<endl;
        }
        delete m_num;
        m_num = nullptr; // 防止野指针
        cout << "destruct Test class ..." << endl;
    }

    int* m_num;
};

Test getObj()
{
    Test t;
    cout << "&t: " << &t << endl; 
    cout << "t.m_num: " << t.m_num << endl;
    return t;
}

Test getObj1(){
  return Test();
}

int main()
{
    Test&& t7 = move(getObj());
    cout << "&t7: " << &t7 << endl;
    cout << "t7.m_num: " << t7.m_num << endl;
    cout<<"-------------------"<<endl;
    Test&& t8 = move(getObj1());
    cout << "&t8: " << &t8 << endl;
    cout << "t8.m_num: " << t8.m_num << endl;
    return 0;
};
// 打印
construct: my name is jerry
&t: 0x7ffc4e831420
t.m_num: 0x5638e2442e70
destruct Test class ...   // 先析构  后面没有再析构 ？？？？？
&t7: 0x7ffc4e831420
t7.m_num: 0
-------------------
construct: my name is jerry
destruct Test class ...   // 先析构  后面没有再析构 ？？？？？
&t8: 0x7ffc4e831420
t8.m_num: 0
```



###  移动和完美转发

####  move

> 核心：move返回的是一个右值，用这个值初始化某个对象，就会调用这个对象的移动构造函数构造，如果没有移动构造函数则会调用拷贝构造函数构造

* move本质是将一个左值转化为一个右值

下面是自定义移动构造的情况

```c++
#include <iostream>
using namespace std;

class Test
{
public:
    Test() : m_num(new int(100))
    {
        cout << "construct: my name is jerry" << endl;
    }

    Test(const Test& a) : m_num(new int(*a.m_num))
    {
        cout << "copy construct: my name is tom" << endl;
    }

    // 添加移动构造函数
    Test(Test&& a) : m_num(a.m_num)
    {
        a.m_num = nullptr;
        cout << "move construct: my name is sunny" << endl;
    }

    ~Test()
    {
        delete m_num;
        cout << "destruct Test class ..." << endl;
    }

    int* m_num;
};


// 直接用右值引用接收右值
int main()
{
    Test t;
    cout<<"==================="<<endl;
    Test&& t3 = move(t);  // 将t转化为一个右值  此时t3就只是t的一个别名  这个赋值过程没有调用任何构造函数
    cout << "t.m_num: " << *t.m_num << endl;   // t还存在
    cout<<"t3.m_num: " << *t3.m_num << endl;
    cout<<"==================="<<endl;
    return 0;
};

// 打印
construct: my name is jerry
===================
t.m_num: 100
t3.m_num: 100
===================
destruct Test class ...

 
// 用对象的左值接收右值，本质是用右值调用移动构造，构造了这个左值对象
int main()
{
    Test t;
    cout<<"==================="<<endl;
    Test t2 = move(t);  // 将t转化为一个右值，用这右值使得调用移动构造函数来构造t2
    cout<<"==================="<<endl;
    cout << "t2.m_num: " << *t2.m_num << endl;
    cout << "t.m_num: " << *t.m_num << endl;  // t.m_num在构造t2调用移动构造时置为nullptr了，所以这里报错，但是此时t还是存在的
    cout<<"==================="<<endl;
    return 0;
};

// 打印
construct: my name is jerry
===================
move construct: my name is sunny  // 移动构造t2
===================
t2.m_num: 100
Segmentation fault (core dumped)
    
    

int main()
{
    Test t;
    cout<<"==================="<<endl;
    Test t2 = move(t);  // 将t转化为一个右值，用这右值使得调用移动构造函数来构造t2
    cout<<"==================="<<endl;
    cout << "t2.m_num: " << *t2.m_num << endl;
    // cout << "t.m_num: " << *t.m_num << endl;  不使用t.m_num则不会出错
    cout<<"==================="<<endl;
    return 0;
};

// 打印
construct: my name is jerry
===================
move construct: my name is sunny
===================
t2.m_num: 100
===================
destruct Test class ...
destruct Test class ...
```

下面是没有自定义移动构造的情况

* 没有自定义移动构造就是用一个右值，调用拷贝构造去构造了一个新对象，和直接=赋值效果是一样的

```c++
#include <iostream>
using namespace std;

class Test
{
public:
    Test() : m_num(new int(100))
    {
        cout << "construct: my name is jerry" << endl;
    }

    Test(const Test& a) : m_num(new int(*a.m_num))
    {
        cout << "copy construct: my name is tom" << endl;
    }

    // 添加移动构造函数
    // Test(Test&& a) : m_num(a.m_num)
    // {
    //     a.m_num = nullptr;
    //     cout << "move construct: my name is sunny" << endl;
    // }

    ~Test()
    {
        delete m_num;
        cout << "destruct Test class ..." << endl;
    }

    int* m_num;
};

Test getObj()
{
    Test t;
    return t;
}

int main()
{
    Test t;
    cout<<"==================="<<endl;
    Test t2 = move(t);  // 将t转化为一个右值，用这右值使得调用移动构造函数来构造t2
    Test t3 = t;       // 调用拷贝构造函数
    cout<<"==================="<<endl;
    cout << "t3.m_num: " << *t3.m_num << endl;
    cout << "t2.m_num: " << *t2.m_num << endl;
    cout << "t.m_num: " << *t.m_num << endl;  // 不使用t.m_num则不会出错
    cout<<"==================="<<endl;
    return 0;
};
// 打印
construct: my name is jerry
===================
copy construct: my name is tom
copy construct: my name is tom
===================
t3.m_num: 100
t2.m_num: 100
t.m_num: 100
===================
destruct Test class ...
destruct Test class ...
destruct Test class ...
```

####  forward

* 当T为左值引用类型时，t将被转换为T类型的左值
* 当T不是左值引用类型时，t将被转换为T类型的右值

```c++
std::forward<T>(t);
```



###  智能指针

####  auto_ptr

* c++17后移除
* 问题：独占指针，但是可以赋值，**赋值后再使用原来的指针则会报错**
* 由new 创建堆区内容，auto_ptr指向这个堆区内容，在auto_ptr指针销毁时，他所指向的堆区也会自动被delete 掉。
* 所有权转移:不小心把它传递给另外的智能指针，原来的指针就不再拥有这个对象了。在拷贝/赋值过程中，会直接剥夺指针对原对象对内存的控制权，
  转交给新对象，然后再将原对象指针置为nullptr

```c++
#include <memory>
#include <iostream>
using namespace std;
int main()
{
  // auto_ptr
  auto_ptr<int> p1(new int(10));
  auto_ptr<int> p2 = p1;
  // p2 = p1;
  // cout << *p1 << endl; // error
  cout << *p2 << endl;  // 正常
	return 0;
}
```

#### unique_ptr

> [使用踩坑](https://blog.csdn.net/qq_42518956/article/details/107490356)

* unique_ptr是专属所有权所以unique_ptr管理的内存，只能被一个对象持有，不支持复制和赋值。
* 移动语义: unique_ptr禁止了拷贝语义，但有时我们也需要能够转移所有权于是提供了移动语义，即可以使用std.move()进行控制所有权的转移。

**初始化**

```c++
#include <iostream>
#include <memory>
using namespace std;

unique_ptr<int> func()
{
    return unique_ptr<int>(new int(520));
}

int main()
{
    // 通过构造函数初始化
    unique_ptr<int> ptr1(new int(10));
    cout << "ptr1: "<< *ptr1 << endl;
    // unique_ptr<int> ptr4 = ptr1; // error  编译器禁止拷贝

    // 通过转移所有权的方式初始化
    unique_ptr<int> ptr2 = move(ptr1);
    cout << "ptr2: "<< *ptr2 << endl;
    // cout << "ptr1: "<< *ptr1 << endl;  // error  执行报错，ptr1已经没有了    也会报错？？？？
    // 通过函数返回值初始化  同 move 都是用右值初始化，右值初始化后，右值就没有了
    unique_ptr<int> ptr3 = func();
    cout << "ptr3: "<< *ptr3 << endl;
    // 通过reset函数重置
    ptr3.reset(new int(1024));
    cout << "ptr3: "<< *ptr3 << endl;
    return 0;
}
```

**踩坑**

* move后，用原来的指针也会报错，感觉和auto_ptr一样
* 用一个原指针通过move初始化两个指针，编译不出错，但执行出错
  * ptr1已经转移所有权给ptr2  ptr1为空指针  再转移所有权给ptr3  ptr3为空指针  执行出错

```c++
#include <iostream>
#include <memory>
using namespace std;

class Test
{
private:
  int* a_;
  int b_;
public:
  // 默认构造函数
  Test():a_(new int(0)), b_(0){
    cout<<this<<" :Test default constructor"<<endl;
  }
  // 有参构造函数
  Test(int a, int b):a_(new int(a)), b_(b){
    cout<<this<<" :Test constructor"<<endl;
  }
  // 拷贝构造函数
  Test(const Test& t):a_(new int(*t.a_)), b_(t.b_){
    cout<<this<<" :Test copy constructor"<<endl;
  }
  // 移动构造函数
  Test(Test&& t):a_(t.a_), b_(t.b_){
    cout<<this<<" :Test move constructor"<<endl;
    t.a_ = nullptr;
    t.b_ = 0;
  }
  // 析构函数
  ~Test(){
    cout<<this<<" :Test destructor"<<endl;
    delete a_;
  }
  void print(){
    cout<<"print: "<<"a: "<<*a_<<" b: "<<b_<<endl;
  }

};

int main()
{
    // 通过构造函数初始化
    unique_ptr<Test> ptr1(new Test(1,2));
    cout<<"ptr1: ";
    ptr1->print();
    // unique_ptr<int> ptr4 = ptr1; // error  编译器禁止拷贝

    // 通过转移所有权的方式初始化
    unique_ptr<Test> ptr2 = move(ptr1);
    unique_ptr<Test> ptr3 = move(ptr1);
    cout<<"ptr2: ";
    ptr2->print();
    // cout<<"ptr3: ";
    // ptr3->print();  // 执行出错  ptr1已经转移所有权给ptr2  ptr1为空指针  再转移所有权给ptr3  ptr3为空指针  执行出错
    // cout<<"ptr1: ";
    // ptr1->print();  // error ptr1已经转移所有权 不能再使用  ??? 也报错
    
    return 0;
}

// 打印
0x55812a6afe70 :Test constructor
ptr1: print: a: 1 b: 2
ptr2: print: a: 1 b: 2
0x55812a6afe70 :Test destructor
```





####  shared_ptr





#  c++11 STL

##  emplace

* `vec.push_back(Test()) 和 vec.emplace_back(Test())`  一样， 都是临时对象一次构造一次析构，一次移动构造
* `vec.push_back(t) 和 vec.emplace_back(t)` 一样，都是一次拷贝构造(不算已有对象的构造和析构)，t是已经存在的对象
* `vec.emplace_back(8, 108)` 的方式传入临时对象`Test(8,108)`时， 才只调用一次有参构造， 这种方式才不同于`push_back`， 效率更高
* 总结：只有传入临时对象，且是以构建对象的参数传入时，`emplace_back`才会效率高于`push_back`

```c++
#include <iostream>
#include <vector>
using namespace std;


class Test
{
private:
  int* a_;
  int b_;
public:
  // 默认构造函数
  Test():a_(new int(0)), b_(0){
    cout<<this<<" :Test default constructor"<<endl;
  }
  // 有参构造函数
  Test(int a, int b):a_(new int(a)), b_(b){
    cout<<this<<" :Test constructor"<<endl;
  }
  // 拷贝构造函数
  Test(const Test& t):a_(new int(*t.a_)), b_(t.b_){
    cout<<this<<" :Test copy constructor"<<endl;
  }
  // 移动构造函数
  Test(Test&& t):a_(t.a_), b_(t.b_){
    cout<<this<<" :Test move constructor"<<endl;
    t.a_ = nullptr;
    t.b_ = 0;
  }
  // 析构函数
  ~Test(){
    cout<<this<<" :Test destructor"<<endl;
    delete a_;
  }
  void print(){
    cout<<"print: "<<"a: "<<*a_<<" b: "<<b_<<endl;
  }

};

int main(){
  vector<Test> vec;
  for(int i = 0; i < 3; i++){
    vec.push_back(Test(i, i + 100));
  }
  cout<<"vec.capacity: "<<vec.capacity()<<endl;
  cout<<"vec.size: "<<vec.size()<<endl;

  // cout<<"--------push_back----------"<<endl;
  // 1 临时对象
  // vec.push_back(Test(4, 104));  // 调用移动构造函数       临时对象一次构造一次析构，一次移动构造
  // 2 已有对象
  // Test t(5, 105);
  // vec.push_back(t);  // 调用拷贝构造函数                  一次拷贝构造(不算已有对象的构造和析构)

  cout<<"--------emplace_back----------"<<endl;
  // 1 临时对象 
  // vec.emplace_back(Test(6, 106));  // 调用移动构造函数    临时对象一次构造一次析构，一次移动构造   同 push_back
  // 2 已有对象
  // Test t(7, 107);
  // vec.emplace_back(t);  // 调用拷贝构造函数               一次拷贝构造(不算已有对象的构造和析构)  同 push_back
  // 3 临时对象直接参数传递
  vec.emplace_back(8, 108);  // 调用有参构造函数             一次有参构造                          不同   push_back  高效    
}
```

