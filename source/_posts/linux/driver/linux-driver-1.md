---
title: linux driver-1
date: 2019-11-19 15:32:25
tags:
- linux
- driver
---

说真的，想写linux driver，先把linux操作系统原理看一遍吧。。。
我真的是傻，想在一天之内就学会这个。。。
就当是一次教训吧。记录下

<!--more-->

```makefile
obj-m := hello.o
KERNELBUILD :=/lib/modules/$(shell uname -r)/build
default:
	make -C $(KERNELBUILD) M=$(shell pwd) modules
clean:
	rm -rf *.o *.ko *.mod.c .*.cmd *.markers *.order *.symvers .tmp_versions
```

注意，Makefile必须首字母大写，否则编译失败。

```c++
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>
MODULE_LICENSE("Dual BSD/GPL");

static int __init hello_init(void)
{
    printk(KERN_EMERG "Hello, world\n");
    int t = 2333;
    return 0;
}

static void __exit hello_exit(void)
{
    printk(KERN_EMERG "Goodbye, cruel world\n");
}

module_init(hello_init);
module_exit(hello_exit);
```

至于驱动程序的代码，就是用module_init和module_exit在内核中注册驱动。
注意printk里的宏没有逗号，否则内核无法输出信息。
