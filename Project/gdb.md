[[compile&debug]]

##### 1：gdb命令调试

| list—查看文件  | start—进入main函数           | q —退出                        | run                |
| ---------- | ------------------------ | ---------------------------- | ------------------ |
| b 3—设置断点   | info b—查看所以断点信息          | delete 1-3 —基于断点号删除          | clear 100—基于行号删除断点 |
| c—继续运行     | n —行单步运行，不进入函数;n 3连续执行3步 | s—进入函数；finish跳出函数；si执行一条机器指令 | u 100—执行直到100行     |
| p var打印变量值 | watch var—在循环里打印变量       |                              |                    |
|            |                          |                              |                    |

1：undefined symbol错误

引起的原因：忘记cmakelist文件中添加cpp文件

定位undefined symbol类型：ldd -r xxx.so

定位C++文件：c++filt   _ZN2cv7waitKeyEi(类型)

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

3：segmentation fault
https://blog.csdn.net/chen1415886044/article/details/108175581

4：