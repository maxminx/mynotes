

# opencv note

## 1：Mat类

由图像基本信息和图像数据data组成。一般的复制拷贝都是浅拷贝，data都是同一个，由引用计数跟踪。深拷贝使用函数

```text
Mat F = A.clone();
Mat G;
A.copyTo(G);
```







