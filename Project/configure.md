[[compile&debug]]
##### 1：用configure配置，控制makefile文件的生成

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

##### 2：cmake替代configure，仅使用configure生成配置头文件.h