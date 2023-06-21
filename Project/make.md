[[compile&debug]]

##### 1：语法参数

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

