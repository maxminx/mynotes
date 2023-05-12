[TOC]

## 一：C++

vscode必备插件C/C++；C/C++ Extension Pack；Code Runner

添加头文件目录：在vscode中按Ctrl+Shift+P 输入configuration 在c_cpp_properties.json中includePath字段中添加

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

## 二：Linux系统函数、posix的API

1：mmap()，提高IO文件效率；多进程共享；驱动程序和用户空间的内存映射

读写文件时需要维护两个内存，即内核态内存和进程用户的内存。通过mmap函数就可以只需要维护一个内核态的内存，再把地址映射到进程用户里面。通过这样的方式，多个进程就可以共享内核态的内存。

\*开始就要确定需要映射的内存大小

https://blog.csdn.net/yangle4695/article/details/52139585 （使用教程）

https://blog.csdn.net/qq_56999918/article/details/127070280

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

## 三：CUDA编程

https://zhuanlan.zhihu.com/p/455866677

## 四：编译和调试

编译器版本和系统已有的标准库版本的对应关系，高版本编译器编译出的程序可能超过系统自带的标准库的能力。

##### 1：用gcc、g++直接编译

主要是找到库和头文件

```bash
g++  -o out.exe -O2 -lthread //默认库位置是/lib、/usr/lib、/usr/local/lib;默认的头文件位置是/usr/local/include
```

现在多用pkg-config命令，去pkgconfig中查找pc文件，里面记录libs库和cflags头文件位置

可用环境变量PKG_CONFIG_PATH指定pc文件的路径

```bash
pkg-config --list-all#打印所有的库名
pkg-config pkgName --libs  --cflags
#例子
#pkg-config log4cpp --libs  --cflags
#-pthread -I/usr/local/include -L/usr/local/lib -llog4cpp
```

```bash
g++ -o testLog4cpp.exe `pkg-config log4cpp --libs  --cflags`
```

如果是arm交叉编译，需要用L和I指定arm库位置和头文件位置，头文件有时可用用x86的。

##### 2：用configure配置，控制makefile文件的生成

交叉编译：理解build、host、target

./configure  --build=在哪编译的  --host=编译出的库在哪平台运行  --target=目标平台

借助编译gcc理解target参数，例如：gcc在x86上编译，运行目标平台是arm，但此gcc编译出的可执行程序在mips平台上运行，如下形式：

./configure  --build=x86  --host=arm  --target=mips

一般会自动确定build参数

其他常见的参数

```bash
预处理
CPP
CXXPP
编译器
CC:
CXX:
选项参数
CFLAGS:
CXXFLAGS:
链接参数
LDFLAGS

./configure --help#查看选项
```

##### 3：cmake替代configure

cmake-gui中CMAKE_BUILD_TYPE设置为Release

##### 4：用make

```bash
$(CC) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) example.c  -o example.exe
```

主要设置的变量

```bash
CPPFLAGS#预处理设置，常用参数：-DNDEBUG，表示ANSI宏不进行调试编译

CFLAGS#C的编译和汇编设置，

CXXFLAGS#作用在c++，赋值CXXFLAGS=$CFLAGS

LDFLAGS#连接器选项
```

CFLAGS和CXXFLAGS参数选项

```bash
-pipe#加快编译速度，编译过程使用管道通信，替代临时文件通信
--sysroot=dir#设置库路径
-isysroot#设置头文件路径

-march=arm#硬件cpu架构
-mfpmath=sse#
-mthreads#


传递给汇编器的选项
形式：-Wa,options
-Wa,-R#合并正文段和数据段，减少地址移动
-Wa,-march=arm#按特定的cpu优化
```

 LDFLAGS参数

```bash
-s#删除可执行程序中的无效东西
大部分形式：-Wl,options
```

##### 5：gdb命令调试

| list—查看文件  | start—进入main函数           | q —退出                        | run                |
| ---------- | ------------------------ | ---------------------------- | ------------------ |
| b 3—设置断点   | info b—查看所以断点信息          | delete 1-3 —基于断点号删除          | clear 100—基于行号删除断点 |
| c—继续运行     | n —行单步运行，不进入函数;n 3连续执行3步 | s—进入函数；finish跳出函数；si执行一条机器指令 | u 100—执行直到100行     |
| p var打印变量值 | watch var—在循环里打印变量       |                              |                    |
|            |                          |                              |                    |

1：undefined symbol错误

引起的原因：忘记cmakelist文件中添加cpp文件

定位undefined symbol类型：ldd -r xxx.so

定位C++文件：c++filt _ZN2cv7waitKeyEi(类型)

refrences:

[undefined symbol问题的查找、定位与解决方法_n大橘为重n的博客-CSDN博客](https://blog.csdn.net/buknow/article/details/96130049?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-96130049-blog-108647920.pc_relevant_landingrelevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-96130049-blog-108647920.pc_relevant_landingrelevant&utm_relevant_index=3)

错误：undefined reference to xxx

链接出错，找不到xxx函数的定义

2： Undefined Reference to Typeinfo

```textile
主要原因
1：虚拟函数未实现
2：混合使用了带RTTI信息和不带RTTI信息的代码导致的
https://blog.csdn.net/tanningzhong/article/details/78598836
```

## 

## 五：数据序列化和反序列化

##### 1：protobuf

序列化：把对象、结构数据等转变为二进制内容

解决的问题：

1-方便远程的进程间通信；本地进程间也可以使用

2-方便几十兆M数量级的数据紧凑的存储在文件中.proto

3-对比json等，数据解析快、占空间小

下面默认介绍proto3

| 修饰词      |                                       | type       | to c++     |
| -------- | ------------------------------------- | ---------- | ---------- |
| singular | default                               | double     | double     |
| optional | 类似singular(除了显示确认有没有设置值，如果没有，就不会被序列化) | float      | float      |
| repeated | 使用方法见官网                               | [s]int32   | int32      |
| map      | version3新增                            | [s]int64   | int64      |
|          |                                       | uint32[64] | uint32[64] |
|          |                                       | bool       | bool       |
|          |                                       | string     | string     |
|          |                                       | bytes      | string     |
|          |                                       | enum       |            |

```protobuf
//number 1-15,using one byte; 16-2047 using two bytes
message Test6 {
  map<string, int32> g = 7;
}
//map和下面作用一样
message Test6 {
  message g_Entry {
    optional string key = 1;
    optional int32 value = 2;
  }
  repeated g_Entry g = 7;
}
/////
enum Corpus {
  CORPUS_UNSPECIFIED = 0;//here 0 is value
  CORPUS_UNIVERSAL = 1;
  CORPUS_WEB = 2;
  CORPUS_IMAGES = 3;
}

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  Corpus corpus = 4;
}
```

## 六：shell用法

##### 1：语法

1:echo

单引号和双引号，句中有其一，则需要其二将字符串包围起来

echo "I'm sunyc"

输出内容和后接命令输出内容在同一行

echo -n "current time=";date;

2:变量复制，等号左右不能有空格

3：反引号，将shell命令的输出赋值

```shell
mdate=`date +%y%m%d`#output format likes 230328
```

4:可以输入重定向的命令grep,wc，sort等等

5：数学计算

整数计算：

```bash
var=`expr 2+4`#expr方式
var=$[2+4]#$[]方式
```

浮点数计算

```bash
var=`echo "scale=3; 2+4"|bc`#sacle定义输出几位的小数点
```

```bash
var1=2
var2=3
var=`bc<<EOF
scale=3
$var1+$var2
EOF
`
echo $var
```

6：选择结构化

```bash
if command#只有command推出码是0时，才执行then
then
    commands
fi
```

```bash
#另一种格式
if command;then
    commands
fi
```

```bash
#if-else
if command;then
    commands
else
    commands
fi
```

```bash
#if-elif
if command;then
    commands
elif command2;then
    commands
fi
```

test命令，用来判断条件，其符号形式为[]

```bash
if test condition;then
    commands
fi

if [condition];then
    commands
fi
```

test用来比较数值;不能用来比较浮点数，可以将浮点数保存为字符串的形式来比较

| command         | description | command         | description |
| --------------- | ----------- | --------------- | ----------- |
| [var1 -eq var2] | 是否相等        | [var1 -le var2] |             |
| [var1 -ge var2] | 是否大于等于      | [var1 -lt var2] |             |
| [var1 -gt var2] | 是否大于        | [var1 -ne var2] |             |

(())，复杂的数学表达，内部不需转义

if (($val+1>9))

test用来比较字符串

| command | description | command  | description |
| ------- | ----------- | -------- | ----------- |
| =       |             | !=       |             |
| >       | 前需有\转义      | -n str1  | 是否非空        |
| <       | 前需有\转义      | -z str11 | 是否为空        |

[[]]，可以进行模板匹配

if [[$USR = r*]]

test用来比较文件

| command         | description           | command         | description |
| --------------- | --------------------- | --------------- | ----------- |
| -d file         |                       | -r file         |             |
| -f file         |                       | -w file         |             |
| -e file         |                       | -x file         |             |
| file1 -nt file2 | file1 newer to file2  | file1 -ot file2 |             |
| -s file         | 是否存在并非空。应用：删除文件前判断    |                 |             |
| -d "$file"      | 避免变量file中有空格，双引号会看成整体 |                 |             |

逻辑：与、或

| []\|\|[] | []&&[] |
| -------- | ------ |

case关键字

```bash
case $var in
rich|bara)
    commands;;
pattern3)
    commands;;
*)
    commands;;
esac
```

7：循环结构

```bash
for var in list
do
    commands
done
#or
for var in list;do
    commands
done#  
```

在list中的默认分隔符是：空格、制表符、换行；在双引号内，即使有分隔符，也会把双引号范围看作一个整体。

若list中有单引号，则需要转义字符\，或双引号把单引号包括起来

修改默认的分隔符变量

```bash
IFS.old=$IFS
IFS=$'\n:;"'//分隔符变为：换行符、冒号、分号、双引号
commands;
IFS=$IFS.old
```

C语言风格的for

```bash
for((a=1,b=2; a<10; a++,b--))
do 
    echo "$a-$b"
done
```

while关键字

```bash
var=10
while [$var -gt 1]
do
    echo $var
    var=$[$var-1]
done
```

while包含多个测试命令时，只有最后的测试状态码决定循环的结束

```bash
while echo $var
      [$var -gt 1]//此测试状态码决定循环的结束
do
    commands
done
```

until关键字，满足条件则停止，与while相反

```bash
var=100
until [$var -qe 0]
do
    var=$var-1
done
```

break n终止当前for循环，n默认是1；当n=2，则是终止双层for循环

continue

最后done可以接管道|，或重定向>

```bash
while []
do 
    commands
done|sort#或者done>result.txt
```

8：用户输入

输入参数：$0-程序名；\$1-\$9；\${10} - \${100000}

计量参数\$#

```bash
var=$#
lastVar=$var//1
lastVar=${!#}//与1处同义，花括号内不能用$符号
```

抓取输入参数

```bash
$*//把多个输入参数看着整体一个str
$@//把多个输入参数看着str数组，可以用for遍历
```

移动变量

```bash
shift n//n默认为1;删除参数1到参数n，参数n+1后面的参数迁移n位
```

##### 2：常用工具

## Z：好书

《C++ 编程规范》(2016)，Herb Sutter

《Effective C++》第3版 (2011)，Scott Meyers
