---
 
title: C++重点
date: 2024-01-26 14:43:17
tags:
- C++
categories:
- [C++]

---



#  右值引用

> lvalue 是`loactor value`的缩写，rvalue 是 `read value`的缩写
>
> https://subingwen.cn/cpp/rvalue-reference/

##  右值与左值

- 左值是指存储在内存中、有明确存储地址（可取地址）的数据；
- 右值是指可以提供数据值的数据（不可取地址）
  * `纯右值`：非引用返回的临时变量、运算表达式产生的临时变量、原始字面量和 lambda 表达式等
  * `将亡值`：与右值引用相关的表达式，比如，T&&类型函数的返回值、 std::move 的返回值等。

##  引用初始化

* **右值引用**
  * **只能用右值初始化右值引用**  右值引用不用初始化右值引用
  * **右值引用 本质是一个 左值**
* 左值引用
  * 用左值初始化
* 常量右值引用
  * 只能用右值初始化
* **常量左值引用**  万能引用类型
  * 可以由左值、左值引用、右值引用、常量右值引用初始化  

```c++
// 左值
int num = 9；
// 左值引用
int& a = num;
// 右值
// 右值引用
int&& b = 8;
// 常量右值引用
const int&& d = 6;
// 常量左值引用
const int& c = num;
const int& f = b;
const int& g = d;
const int&h = a;
```





##  T&&  auto&&

- 通过右值推导 T&& 或者 auto&& 得到的是一个右值引用类型
- 通过非右值（右值引用、左值、左值引用、常量右值引用、常量左值引用）推导 T&& 或者 auto&& 得到的是一个左值引用类型

```c++
template<typename T>
void f(T&& param);
void f1(const T&& param);  // 不用推导 传入的就必须是一个右值  这里就是一个常量右值引用
f(10); 	
int x = 10;
f(x); 
f1(x);	// error, x是左值
f1(10); // ok, 10是右值
```



```c++
int&& a1 = 5;
auto&& bb = a1;  // int&
auto&& bb1 = 5;  // int&&

int a2 = 5;
int &a3 = a2;
auto&& cc = a3;  // int&
auto&& cc1 = a2; // int&

const int& s1 = 100;
const int&& s2 = 100;
auto&& dd = s1;  // const int&
auto&& ee = s2;  // const int&

const auto&& x = 5; // 不用推导 就是 const int&& 常量右值引用  必须用右值初始化
```



#  move & forward

##  move

>  将左值转换为一个右值  用于去初始化右值引用

###  move与移动构造的区别

* move是将一个对象的所有权转给另一个对象  即**对象的内容完全重复利用**
* 移动构造  是在内部重写逻辑 复用堆区的内容  只是**复用堆区内容**

```c++
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
    Test(Test&& a) : m_num(a.m_num)  // 复用原对象的堆区
    {
        a.m_num = nullptr;  // 原对象的堆区指针设为nullptr  避免原对象 对堆区的释放
        cout << "move construct: my name is sunny" << endl;
    }

    ~Test()
    {
        delete m_num;
        cout << "destruct Test class ..." << endl;
    }

    int* m_num;
};
```

##  move使用

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
    return t;  // 临时对象 但是可以取地址
}

Test getObj1()
{
    return Test(); // 匿名对象 临时的 且不可取地址
}

int main()
{
  	// 如果对象有移动构造  且右侧是临时对象（getObj()返回的是临时对象）则会调用移动构造
    // 下面两个的情况一样  
    // 如果对象没有移动构造  则会调用拷贝构造
    Test t = getObj();    // 一次构造  一次移动构造
    Test&& t1 = getObj(); // 一次构造  一次移动构造
  
    // 如果没有移动构造 要给右值引用初始化 要求右侧是一个临时的不可取地址的对象
    Test&& t2 = getObj1(); // 一次构造   用右值引用给内部构造的匿名对象续命了
  
  	//
  	Test&& t3 = move (t2) ; // 无构造 t2是左值引用（右值引用使用时就具名话 成为左值引用了）通过move转换为右值  续命
    Test&& t4 = move(t) ;   // 无构造 t是左值  通过move转换为右值  续命
    return 0;
};
```



##  forward

> 右值引用作为值传递时 会成为一个左值引用
>
> https://subingwen.cn/cpp/move-forward/

```c++
// 函数原型
template <class T> T&& forward (typename remove_reference<T>::type& t) noexcept;
template <class T> T&& forward (typename remove_reference<T>::type&& t) noexcept;

// 精简之后的样子
std::forward<T>(t);
```

- `当T为左值引用类型时，t将被转换为T类型的左值`
- `当T不是左值引用类型时，t将被转换为T类型的右值`



