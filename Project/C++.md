[[program语言]]

##### 0：现存问题

1：for循环大量处理opencv操作如下，读图片、操作图片、写图片；导致写的图片部分损坏，打不开。猜测可能的原因之一是缓存缓冲引起的。

##### 1：语法

1：using使用:

和namespace搭配；using  newclass = myclass，newclass是myclass的别名；

定义函数指针类型

```c
using FunPtrType = int (*)(int, int);    //a
typedef int (*FunPtrType)(int, int);    //b和a意义一样，用using更有可读性
int (*funptr)(int, int) = nullptr;    //funptr是函数指针变量，不是函数指针类型
```

2：const加在函数后面

    只能是加在非静态的类成员函数后面，表示this是const，不能修改成员变量，只读不能写

    const加在函数前面，表示返回的值是const

3：std:function

    函数模板对象，可认为是同一类型函数(相同的输入参数类型和返回参数类型)的父类，类似带虚函数的派生类指向父类

```c++
std::vector<std::function<int(int)>> callbacks// 存储回调函数
```

 4：private

父类A有私有变量i，在公有成员方法fun打印i；子类B公有继承父类，列表初始化B(int i):A(i){}，子类调用fun可以成功打印i。

5：右值引用、std::move类型转换、std::forward类型转换

std::forward<T>(u)有两个参数：T与 u。 a. 当T为左值引用类型时，u将被转换为T类型的左值； b. 否则u将被转换为T类型右值

应用场景：verctor.push_back(std::move(object))、类的移动构造函数

```C++
void change2(int&& ref_r) {
    ref_r = 1;
}

void change3(int& ref_l) {
    ref_l = 1;
}

// change的入参是右值引用
// 有名字的右值引用是 左值，因此ref_r是左值
void change(int&& ref_r) {
    change2(ref_r);  // 错误，change2的入参是右值引用，需要接右值，ref_r是左值，编译失败

    change2(std::move(ref_r)); // ok，std::move把左值转为右值，编译通过
    change2(std::forward<int &&>(ref_r));  // ok，std::forward的T是右值引用类型(int &&)，符合条件b，因此u(ref_r)会被转换为右值，编译通过

    change3(ref_r); // ok，change3的入参是左值引用，需要接左值，ref_r是左值，编译通过
    change3(std::forward<int &>(ref_r)); // ok，std::forward的T是左值引用类型(int &)，符合条件a，因此u(ref_r)会被转换为左值，编译通过
    // 可见，forward可以把值转换为左值或者右值
}
```

6：auto在编译器上确定变量的类型

    对于for(auto a:arr)的arr必须是个有范围的，不能是无范围的指针

7：lambda表达式

8：空指针不同的的定义，NULL=0，nullptr=(void*)0

std::add_point

##### 2：规范

1：一个头文件被多个源文件包含，会出现多从定义的问题，要用宏解决。

c++标准规定变量声明的格式为：extern type variable;，其他形式都是定义。

```c
//head.h头文件
#pragma once    //新方式，和#ifndef head.h ...老方式作用相似；为确保编译器兼容性，都加上
#ifndef HEAD_H
#define HEAD_H
#include<iostream>
void fun();
extern int myvalue;//这里是申明，不是定义，不分配内存；extern表示可以在其他源文件中使用
#define MYSUM(a,b)   (a*b)
#define myTest(param) (int param=321)
#endif//HEAD_H
```

头文件设置的全局变量问题

```c
//head.cpp
#include<iostream>
#include<head.h>
int myvalue =9;//这里是定义
void fun()
{ 
    std::cout<<"this is fun, value="<<MYSUM(1,2)<<std::endl;

}
```

注意：

****头文件不能有循环依赖，应该是有向无环图

2：头文件相互引用，造成循环依赖。方法1：使用接口；方法2：用前置声明

```c
//A.h:
#ifndef A_H
#define A_H
class B
class A{
typedef vector<string>::sizetype size_type;
B* b;
}
#endif

//B.h:
#ifndef B_H
#define B_H
#include "A.h"
class B{
A::size_type num;
}
#endif

//用指针B*,因为编译器无法确定对象的实际大小，而指针的大小在特定机器类型上是固定的
```

3：兼容C和C++

extern "C" void fun();

更通用

```c
#ifdef __cplusplus
extern "C" {
#endif
void display();
#ifdef __cplusplus
}
#endif 
```

##### 3：性能优化

优化方向：利用多核、利用缓存

https://www.jianshu.com/p/5a354746fe2e

## 二：规范



2.接口和实现分离，重点pimpl方式，其次是抽象类的纯虚函数方式

不隐藏具体实现类class Impl，需要两个文件，比如telpo_algsdk.h，telpo_algsdk.cpp

隐藏具体实现类class Impl，需要四个文件，比如algsdk.h, algsdk.cpp, algimpl.h, algimlp.cpp

refrence:

https://blog.csdn.net/qq_20853741/article/details/121244189

微软C++手册，https://learn.microsoft.com/zh-cn/cpp/cpp/pimpl-for-compile-time-encapsulation-modern-cpp?view=msvc-170

3.回调的几种方式

https://blog.csdn.net/yuejisuo1948/article/details/113280498

https://bot-man-jl.github.io/articles/?post=2019/Inside-Cpp-Callback#%e4%b8%ba%e4%bb%80%e4%b9%88%e8%a6%81%e5%8c%ba%e5%88%86%e4%b8%80%e6%ac%a1%e5%92%8c%e5%a4%9a%e6%ac%a1%e5%9b%9e%e8%b0%83

4.异步编程

https://zhuanlan.zhihu.com/p/78612487