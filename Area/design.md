## 10种软件架构模式

1. 分层模式
2. 客户端-服务器模式
3. 主从设备模式
4. 管道-过滤器模式 https://blog.csdn.net/qq_34179431/article/details/116655251
5. 代理模式
6. 点对点模式
7. 事件总线模式
8. 模型-视图-控制器模式
9. 黑板模式
10. 解释器模式

https://www.cnblogs.com/IcanFixIt/p/7518146.html

高并发模式

1：消息传递模型：actor和CSP

2：IO并发模式：阻塞式IO，非阻塞式IO，多路复用IO，信号驱动IO和异步IO

https://cloud.tencent.com/developer/article/1170510?from=article.detail.1968977&areaSource=106000.9&traceId=MDCR2-WRpFsaZVqauadRR

## 六大设计原则solid

1：单一原则 single responsibility

2：开闭原则open closed principle

3：里氏替换原则Liskov substitution

4：最小知道原则law of demeter

5：接口隔离原则 interface segregation

接口：事物的顶层抽象，具体形式有：抽象类、模板、函数签名(函数指针、callable object）、某些设计模式(迭代器、观察者、订阅发布等)。目的是隔离具体的实现。

6：依赖倒置原则dependence inversion

## 设计模式

1：工厂模式，从简单工厂--->基于模板的自动注册的工厂模式

https://zhuanlan.zhihu.com/p/268462046

2：单例模式

单例的双锁检验，在某些平台或编译器会失效。

简单无误的局部静态变量模式

```c++
class Singleton
{
public:
    ~Singleton(){
        std::cout<<"destructor called!"<<std::endl;
    }
    Singleton(const Singleton&)=delete;
    Singleton& operator=(const Singleton&)=delete;
    static Singleton& get_instance(){
        static Singleton instance;
        return instance;

    }
private:
    Singleton(){
        std::cout<<"constructor called!"<<std::endl;
    }
};
```

[C++ 单例模式和可继承的单例基类模板_c++ 单例 基类_panamera12的博客-CSDN博客](https://blog.csdn.net/wteruiycbqqvwt/article/details/124852912)
