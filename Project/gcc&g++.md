[[compile&debug]]
[[C++]]
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
