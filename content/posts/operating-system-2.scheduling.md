---
title: "【操作系统】2.调度scheduling"
date: 2019-02-27T16:37:33+08:00
draft: false
tags: [OS]
categories: [OperatingSystem]
---
<!--more-->

### 调度的前提：限制

为了虚拟化 CPU，操作系统需要将物理 CPU 分享给正在运行的作业，即进程。虚拟化的基础思想是：运行一个进程一会儿，然后再运行另一个，一直下去，按时间来分享 CPU。 然而建立虚拟机有两个难题：性能和控制性，虚拟机的可控性非常重要，他可以让进程在系统中稳定的运行，不会崩溃，所以就有了一个技术: Limited Direct Execution(限制直接执行)。 如果不做限制，一般是当操作系统想运行某个程序的时候，系统就会在进程表里面创建一条记录，然后分配内存给程序，并把程序代码加载到内存里面去，然后开始运行程序代码。 Limited Direct Execution 是说当操作系统想让一个程序运行的时候，不直接让它做上面的操作，需要避免危险操作。

**所以下面我们有两个需要注意的：**
- 1.严格操作 Restricted Operations 系统会给一些危险的操作设置模式，用来保护系统，例如文件系统的读写权限模式。
- 2.上下文切换 Switching Between Processes 上下文切换即可以理解为进程间的切换，上下文切换需要一定的策略，可以根据进程的优先级来判断，切换时会消耗一定的资源。
切换有两种方式：
1) 合作方式：由进程等等系统调用 操作系统有依据的信任进程。
2) 非合作方式：由操作系统控制 操作系统完全控制着进程的停止和开始。

### 调度介绍

操作系统在执行多个作业的时候，总是需要在各个作业之间切换，调度就是用来实现这个的。

**调度有许多种思想，叫做调度策略，大致有如下几种：**
- 1.先来先服务
First In, First Out (FIFO) 或 First Come, First Served (FCFS)
- 2.短作业优先
Shortest Job First (SJF)
- 3.抢占式短作业优先
Shortest Time-to-Completion First (STCF)
- 4.轮循
Round Robin
- 5.多级反馈队列
Multi-level Feedback Queue (MLFQ)

**有一些指标可以用来衡量调度策略的优劣**
- 1.周转时间
turnaround time T\_turnaround = T\_completion − T\_arrival
周转时间 = 完成的时间点 - 开始的时间点
- 2.响应时间
response time T\_response = T\_firstrun − T\_arrival
响应时间 = 每轮完成的时间点 - 每轮开始的时间点

**为了能很好的理解调度策略，我们先来假设几条规则，然后逐渐抛弃这些规则的限制**
- 1.每个作业运行的时间相同
- 2.每个作业开始在相同的时间
- 3.一旦作业开始运行，那么它最后都会执行完成
- 4.所以作业只用 CPU （不考虑 I/O 形式）
- 5.已经知道作业的运行时间

**下面来介绍每个调度策略**
- 1.先来先服务
First In, First Out (FIFO) 或 First Come, First Served (FCFS)
假设作业A, B, C 的到达时间均为 0, 所需执行的时长分别为 100, 10, 10, 且其他假设条件不变 那么我们设计出下图
![FIFO.png](http://cdn.gzhh.tech/2018/02/FIFO.png)
可以很容易得到三个作业的平均周转时间 = (100 + 110 + 120) / 3

- 2.短作业优先 Shortest Job First (SJF)
假设还是和上面一样的，得到下图
![FIFO.png](http://cdn.gzhh.tech/2018/02/SJF.png)
平均周转时间 = (10 + 20 + 120) / 3
根据平均周转时间指标可以得出 SJF 比 FIFO 要优化不少
但是我们改了下假设，让 B, C 的开始时间都为 10，这里作业的执行策略就会像下图这样
![FIFO.png](http://cdn.gzhh.tech/2018/02/SJF_2.png)
平均周转时间 = (100 + 100 + 110) / 3 效果差了很多

- 3.抢占式短作业优先 Shortest Time-to-Completion First (STCF)
我们这里还是按照上面 SJF 的第二个假设来看
抢占式短作业优先的策略就是根据作业的优先权来先后执行作业
下图的作业优先权是依据当前进程的运行时间和余下进程的总时间来比较的，如果前者比后者小，当前进程优先级比后者高；如果前者比后者大，那么余下进程优先级高
![FIFO.png](http://cdn.gzhh.tech/2018/02/STCF.png)
得出平均周转时间 = (120 + 20 + 30) / 3 比 SFJ 的第二种情况好了不少

- 4.轮循 Round Robin
这里我们还是把假设改下，让 A, B, C 的开始时间都为 0，执行时间都为 5
但是当我们考虑到与用户交互的作业时，上面三种调度一般都会让用户等待作业比较长时间，所以这里我们说下 Round Robin 调度的实现原则：
即给每个作业都设置一个时间片(可以是很短)，然后在时间片内不停的切换不同的进程，让每个进程的响应时间都短，提高用户体验 所以，我们可以假设得到下面的图，用 SJF 和 RR 来分别实现这里的假设
![FIFO.png](http://cdn.gzhh.tech/2018/02/RoundRobin.png) 
可以看出如果根据周转时间来衡量，那么 SJF 周转时间 = (5 + 10 + 15) / 3 RR 周转时间 = (13 + 14 + 15) / 3 得出 SJF 比 RR 稍微好点
如果根据响应时间来衡量，那么 SJF 响应时间 = (5 + 10 + 15) / 3 RR 响应时间 = (1 + 2 + 3) / 3 得出 RR 比 SJF 好了很多

- 5.多级反馈队列 Multi-level Feedback Queue (MLFQ)
上面四种调度我们默认是在单个 CPU 下进行的，并且也是在单个任务队列中分发作业任务的，没有考虑到多个任务队列，而现代操作系统大多都采取这种调度机制，现在我们假设有多个作业任务队列。

**MLFQ 基本规则**
- Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t).
- Rule 2: If Priority(A) = Priority(B), A & B run in RR. 改变优先级
- Rule 3: When a job enters the system, it is placed at the highest priority (the topmost queue).
- Rule 4a: If a job uses up an entire time slice while running, its pri- ority is reduced (i.e., it moves down one queue).
- Rule 4b: If a job gives up the CPU before the time slice is up, it stays at the same priority level.
例如键盘输入作业的优先级比较高，而长期占用cpu的作业优先级比较低，短时间作业一般要求很强的交互，响应时间比较重要，而长期占用cpu的作业一般则不需要很强的交互 我们这里给出一个多队列作业图
![FIFO.png](http://cdn.gzhh.tech/2018/02/MLFQ.png)

**根据 MLFQ 的概念，我们基本可以得出以下结论**
- 1.如果任务A的优先级大于任务C的优先级，则先运行任务A
- 2.任务A和B优先级一样，则任务A、B以Round Robin的方式运行
- 3.当有新任务到来的时候，进入优先级最高的队列
- 4.一旦一个任务用完了时间片或者因为I/O放弃了CPU，则将该任务移到下一个优先级队列
- 5.固定周期之后，把所有的任务都迁移到优先级最高的队列中去


### 参考资料

[http://pages.cs.wisc.edu/~remzi/OSTEP/](http://pages.cs.wisc.edu/~remzi/OSTEP/)
