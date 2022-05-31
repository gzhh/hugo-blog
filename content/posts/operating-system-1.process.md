---
title: "【操作系统】1.进程process"
date: 2019-02-25T23:00:39+08:00
draft: false
tags: [OS]
categories: [OperatingSystem]
---
<!--more-->

### 介绍
什么是进程：简单来说就是一个正在运行中的程序 进程是程序的一次执行，包括代码和数据，是CPU分配资源的基本单位，一个进程可以包括多个线程。

进程之间通信方式：管道、SOCKET、信号量（互斥、同步）等。

子进程是父进程的复制品。子进程获得父进程数据空间、堆和栈的复制品。

线程是独立运行和独立调度的基本单位（线程比进程更小，基本上不拥有系统资源，故对它的调度所付出的开销就会小得多，能更
高效的提高系统内多个程序间并发执行的程度），线程之间共享进程的数据空间（借此通信）

### 进程接口
操作系统提供一些 API 给进程调用，例如 create 创建, destroy 销毁, wait 等, suspend 暂停, status 状态等。

**1.fork()**

fork函数复制了当前进程，生成子进程，子进程与父进程不完全相同，子进程有自己的资源，例如：address space (private memory), registers, PC。

子进程不会从 main 函数开始运行，只会从调用 fork 函数处开始运行。

fork调用的一个奇妙之处就是它仅仅被调用一次，却能够返回两次，它可能有三种不同的返回值：

1）在父进程中，fork返回新创建子进程的进程ID；

2）在子进程中，fork返回0；

3）如果出现错误，fork返回一个负值；

    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    
    int main(int argc, char *argv[])
    {
        printf("hello (pid:%d)\n", (int) getpid());
        int rc = fork();
        if (rc < 0) { // fork failed, exit
            fprintf(stderr, "fork failed\n");
            exit(1);
        } else if (rc == 0) { // child (new process)
            printf("hello, i am child (pid:%d)\n", (int) getpid());
        } else { // parent goes down this path (main)
            printf("hello, i am parent of %d (pid:%d)\n", rc, (int) getpid());
        }
    
        return 0;
    }
    
    
    /*
    复制了当前进程的备份，称为子进程，子进程一般在父进程执行完后再执行，子进程不会从 main 函数开始
    执行，而是从 fork 函数开始执行
    
    大致结果：
    hello (pid:10642)
    hello, i am parent of 10643 (pid:10642)
    hello, i am child (pid:10643)
    */
    
    

**2.wait()**

wait函数让当前进程执行等操作，进入wait状态，而操作系统的调度器则会去执行其他的进程，当根据调度策略执行完其他进程，回到该正在等的进程时，继续执行这个进程。

返回值：如果执行成功则返回子进程识别码(PID), 如果有错误发生则返回-1. 失败原因存于errno 中.

    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    #include <sys/wait.h>
    
    int main(int argc, char *argv[])
    {
        printf("hello (pid:%d)\n", (int) getpid());
        int rc = fork();
        if (rc < 0) { // fork failed, exit
            fprintf(stderr, "fork failed\n");
            exit(1);
        } else if (rc == 0) { // child (new process)
            printf("hello, i am child (pid:%d)\n", (int) getpid());
        } else { // parent goes down this path (main)
            int wc = wait(NULL);
            printf("hello, i am parent of %d (wc:%d) (pid:%d)\n", rc, wc, (int) getpid());
        }
    
        return 0;
    }
    
    
    /*
    父进程进程了等操作，所以子进程先走，完了后再继续执行父进程
    
    大致结果：
    hello (pid:10633)
    hello, i am child (pid:10634)
    hello, i am parent of 10634 (wc:10634) (pid:10633)
    */
    

**3.exec() 函数集合**

下面这些函数执行操作系统的命令
- execl(path, arg1, arg2, ...)
- execle(path, arg1, arg2, ...)
- execlp(file, arg1, arg2, ...)
- execv(path, arg\[\])
- execvp(file, arg\[\])

最好用 execve() 内核级系统调用，上面5个都是调用了这个函数

    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>
    #include <unistd.h>
    #include <sys/wait.h>
    
    int main(int argc, char *argv[])
    {
        printf("hello (pid:%d)\n", (int) getpid());
        int rc = fork();
        if (rc < 0) { // fork failed, exit
            fprintf(stderr, "fork failed\n");
            exit(1);
        } else if (rc == 0) { // child (new process)
            printf("hello, i am child (pid:%d)\n", (int) getpid());
            char *myargs[3];
            myargs[0] = strdup("wc");   // program: "wc" (word count)
            myargs[1] = strdup("p3.c"); // argument: file to count
            myargs[2] = NULL;           // marks end of array
            execvp(myargs[0], myargs);  // runs word count
            printf("this shouldn't print out\n");
        } else { // parent goes down this path (main)
            int wc = wait(NULL);
            printf("hello, i am parent of %d (wc:%d) (pid:%d)\n", rc, wc, (int) getpid());
        }
    
        return 0;
    }
    
    
    /*
    strdup() 复制字符串的备份并返回
    execvp() 执行成功直接退出，执行失败则继续往下走
    
    大致结果：
    hello (pid:10700)
    hello, i am child (pid:10701)
     28 121 917 p3.c
    hello, i am parent of 10701 (wc:10701) (pid:10700)
    */
    

**4.总结**

fork/exec 编程模式很流行

### 进程的创建

从程序到进程创建
![program_to_process](http://uploads.gzhh.tech/2018/02/program_to_process.png)
- 1.进程加载一个程序和静态数据到内存，要求操作系统从磁盘读取数据并把它们放到内存中去。
- 2.为程序的运行栈分配内存 Ｃ语言用栈来处理局部变量、函数形参和返回地址。
用堆来动态分配数据：例如调用 malloc() 分配内存，释放内存调用 free()， 堆的数据结构可以是链表、hash 表、树等。

### 进程的状态

进程状态转换图
![process_state_transitions](http://uploads.gzhh.tech/2018/02/process_state_transitions.png)
Running 运行中 Ready 等待 Blocked 阻塞 Zombie 僵尸

### 进程的数据结构

进程也有自己的数据结构，一般是进程数据块 Process Control Block(PCB)

### 参考

[http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf)
