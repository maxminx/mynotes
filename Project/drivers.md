[[Linux]]
[[hardware]]

内核头文件/usr/src/linux/include

##### 一：驱动相关命令

| name          | func           |
| ------------- | -------------- |
| lsmod         | 查看驱动列表         |
| modinfo  name | 查看驱动name模块具体信息 |
| insmod name   | 加载驱动模块         |
| rmmod name    | 卸载驱动模块         |

相关文件：

| cat /proc/devices       | 查看字符、块设备的主设备号       | 通过insmod加载到内核        |
| ----------------------- | ------------------- | -------------------- |
| ls -hal  /sys/dev/block | 主设备号：次设备号-->连接到真实设备 | 指向/sys/devices目录下的设备 |
| /dev/\*                 | 设备文件目录，通过mknod添加    | 供应用程序访问和控制实际设备       |
| /sys/devices            | 设备按总线分类的目录结构        |                      |
|                         |                     |                      |

根据主设备号，绑定对应的驱动

目录分析/sys/

https://www.cnblogs.com/aozhejin/p/15874504.html


二：模块编译、加载、卸载

```c
#include<linux/init.h>
#include<linux/module.h>

//#include<linux/Kernel.h>


MODULE_LICENSE("Dual BSD/GPL");

static int hello_init(void)
{
	printk(KERN_ALERT "hello world\n");
	return 0;
}

static void hello_exit(void)
{
	printk(KERN_ALERT "Goodbye,cruel world\n");
}

module_init(hello_init);
module_exit(hello_exit);

```

```make
ifneq ($(KERNELRELEASE),)
obj-m :=hello.o
else
KDIR :=/lib/modules/$(shell uname -r)/build
all:
	make -C $(KDIR)  M=/home/sun/temp/kernel  modules
clean:
	rm -f *.ko *.o *.mod.o *.mod.c *.symvers *.order  *.mod
endif

```

编译sudo make
加载insmod hello.ko
卸载rmmod hello.ko
查看printk信息 dmesg


三：字符设备驱动开发