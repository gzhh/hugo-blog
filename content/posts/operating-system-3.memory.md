---
title: "【操作系统】3.内存memory(抽象的地址空间)"
date: 2019-03-11T01:08:00+08:00
draft: false
tags: [OS]
categories: [OperatingSystem]
---
<!--more-->

### 介绍

早期的操作系统实现功能起来很简单，一般看来，物理内存的结构就一般分为俩快：一块用来存os的代码和数据，另一块给当前程序的代码和数据。就像下图： 

![early_memory](/img/2018/03/early_memory.png) 

但是随着多程序的诞生，为了避免在进程间频繁的切换损失性能，所以重新设计了内存的结构，来增加效率，就像下图这样，每个进程都有自己的私有空间，保护了进程的安全，不受其他进程影响

![three_process_sharing_memory](/img/2018/03/three_process_sharing_memory.png) 

为了用起来方便，我们让操作系统创建了物理内存的抽象，叫做地址空间 address space，它是运行中程序的视图，地址空间是内存虚拟化的关键，进程的地址空间包含所有在运行中程序的记忆状态；用地址空间的顶部来存静态代码，提高空间利用率；用栈区来保存函数调用链，局部变量，传递形参，返回值等；用堆区来动态分配内存，用户管理内存，例如c语言里的malloc()和c++里面的new，其他的还有静态区 我们抽象的假设code区在顶部，它的下面是heap区，堆动态向下增长，堆的下面是free区，free区的下面是stack区，stack会动态向上增长。

![a_example_address_space](/img/2018/03/a_example_address_space.png) 

然而事实上进程在内存中的地址是随机

![three_process_sharing_memory](/img/2018/03/three_process_sharing_memory.png) 

所以就出现了虚拟化内存virtualizing memory，即操作系统为正在运行的多个进程分配私有、潜在、独立的地址空间，所有的虚拟地址都会被操作系统翻译成真实物理地址，然后就可以获取到各种值和信息

可以写段代码测试下内存中的虚拟地址

    #include <stdio.h>
    #include <stdlib.h>
    
    /*打印code, heap, stack三个区的虚拟地址，十六进制表示*/
    int main(int argc, char *argv[])
    {
        printf("location of code : %p\n", (void *) main); // main函数开始地址
        printf("location of heap : %p\n", (void *) malloc(1));
        int x = 3;
        printf("location of stack : %p\n", (void *) &x);
        return x;
    }
    /*
    大致结果：
    location of code : 0x4005d6
    location of heap : 0xcc1420
    location of stack : 0x7ffde8e29124
    */
    

### 分配和管理内存

操作系统提供了一些api来给用户管理内存
1.types of memory 一般的程序，都有编译器暗中为你在stack区中allocate和deallocate内存， 而heap区中的分配和释放内存则由程序员手动来操作
2.malloc() call 属于libary call，不是system call

    #include <stdlib.h>
    ...
    void *malloc(size_t size);
    

3.free() call 释放内存例子

    int *x = malloc(10 * sizeof(int));
    ...
    free(x);
    

4.管理内存时易犯错误
1)忘记分配内存

    // Allocate Memory 
    char \*src = "hello"; char \*dst; // oops! unallocated
    strcpy(dst, src); // segfault and die

2)未分配足够内存

    (char *) malloc(strlen(src)); // too small (char *)
    malloc(strlen(src) + 1); // properly

3)Forgetting to Initialize Allocated Memory 忘记填分配的数据类型，则可能会导致uninitialized read

4).忘记free内存 忘记释放内存会导致内存泄露leaking memory 有点语言会自动garbage-collected，但是有的资源gc是不会释放的

5).free 调用错误 free()错误，参数需要指针类型 测试例子：

    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>
    int main(int argc, char *argv[])
    {
        /*
        int *x = (int *) malloc(sizeof(int));
        printf("%p\n", x);
        */
    
        /* 错误，执行时错误
        char *src = "hello";
        char *dst; // oops! unallocated
        strcpy(dst, src); // segfault and die
        */
    
        /* 正确 */
        char *src = "hello";
        char *dst = (char *) malloc(strlen(src) + 1);
        strcpy(dst, src); // work properly
    
        return 0;
    }
    
    /*
    编译过程：
    看到 int *x 的时候，为整型指针创建一个临时空间
    看到 malloc 的时候，把整型指针heap中的一个地址
    最后函数返回地址存在stack上
    */
    

### 地址翻译 address translation

问题：给一个测试代码，问os如何从内存中加载一个值

    void func() {
    int x = 3000; // thanks, Perry.
    x = x + 3; // this is the line of code we are interested in
    

首先c编译器会将代码按行转化成汇编，像下面这样

    128: movl 0x0(%ebx), %eax ;load 0+ebx into eax
    132: addl $0x03, %eax ;add 3 to eax register
    135: movl %eax, 0x0(%ebx) ;store eax back to mem
    

这段汇编代码大意就是：首先x的地址被替换为寄存器ebx，下一步用 move 指令从寄存器ebx中加载出x的值，然后给ebx加上3，最后返回寄存器ebx的在内存中的某个地址 通过地址翻译，操作系统能控制进程的每次访问
