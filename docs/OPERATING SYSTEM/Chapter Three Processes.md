## Process Concepts
* Batch system – a batch is a sequence of jobs
![[image-98.png]]

* Time-sharing systems – user programs or tasks
  * Multitasking
  * Less turnaround, less CPU idle, user interaction
* **Process** – a program in execution; process execution must progress in sequential fashion
  * text section
  * data section
  * stack（这些都是特定功能区域）
  * heap
  * program counter
![[image-99.png]]

* 堆和栈会变化，对向而生，灵活区域，充分利用

![[image-100.png]]

* ready: 提交到操作系统时，ready 就是进程需要等一下，因为 os 在执行其他的 process
* waiting：等待除了 cpu 之外的东西
* 如果大家都是 cooperative process，都可以使用 cpu
* 如果遇到不配合的，如何处理呢？操作系统调度
* 这个 running 的中断如何产生？定时器，I/O

## PCB: Process control block

![[image-149.png]]
* Linux: task struck, Windows: e process
* 储存进程信息
> State，number, counter, reg, memory, list of open file
![[image-147.png]]

* PCB 里的信息不是最新的，中断之后，kernel 运行，更新

## Process Scheduling
* job queue: a set of all processes in the system
* ready queue
> 包含所有 ready 状态的进程，等待使用 CPU 的队列
> 特殊的 device queue

* 每个 device 维护一个queue，在这个设备空闲之后，就可以选一个继续运行
* 进程在这些队列里面转
* 还有一张表，需要收集所有 PCB（linked list）

![[image-148.png]]


## Schedulers
![[image-150.png]]
*UNIX and Windows do not use long-term scheduling*
* the kernel decide which process to run
> *Longterm scheduling*: which program to run (程序)
> *Short-term scheduling*: in the queue, find one to run
> 中期调度：swap in, swap out：多个进程运行，内存稀缺，把其中某个进程暂时不运行，把它先放在硬盘里
![[image-151.png]]
* 一些放在内存上，一些放在硬盘上，动态处理
* CPU-bound, I/O-bound
> 统计，在 ready queue 待的时间长，说明需要 CPU
> 在 I/O queue 中待的时间多，经常出现，说明 I/O bound

* Short-term scheduler is invoked very frequently (milliseconds) ⇒ (must be fast)
* Long-term scheduler is invoked very infrequently (seconds, minutes) ⇒ (may be slow)
* The long-term scheduler controls the _degree of multiprogramming_
* Need to select good combination of CPU bound and I/O bound processes.
* "Autoregression": I/O bound
* "Diffusion": computing bound
![[image-152.png]]
* 可以把寄存器分组，在一个 process 被拿下来之后，PCB 的储存可以先放在一块寄存器里不动

## Process Creation
* start a creation from a creation
* 父进程，子进程
* a tree of processers
* scheduler（CPU） 只会从 ready queue 中选择运行的进程
* fork (): create child process
* **exec** system call used after a **fork** to replace the process’ memory space with a new program
![[image-153.png]]
* pid 为 0：子进程
* ps 命令查看所有进程，or top
> `ps -f -u shou`
* `wait()` 等待子进程结束

## Process Termination
* 进程结束之后，操作系统会回收进程资源
![[image-154.png]]

* 如果父进程结束，会给 init，即 PID=1 的进程
* fork () 之后所有的父子进程都在 ready queue 中
* 在 fork（）的过程中，parent 在 waiting 状态，等待创建子进程完毕

## Cooperating Processes
![[image-242.png]]

### Producer-Consumer Problem
* 信息的生产方和消费方
> 即使时互换信息，互为生产者与消费者
> *unbounded buffer*: no practical limit in the size of the buffer (think of it a bounded buffer with large buffer)
> *bounded buffer*: fixed buffer size, producer must wait if buffer is full
* shared data
![[image-245.png]]
![[image-246.png]]

* intuition
> In: producer input pointer
> Out: consumer output pointer
![[image-247.png]]
![[image-248.png]]




* `wsl`：的系统进程是 6962
* 程序在 shell 中执行时，如果没有父进程，就会被挂在系统进程中

## Interprocess Communication
* Two models for IPC: *message passing* and *shared memory*
> Shared memory: 都当成自己的，都在内存中，立马就能看到改变，加读写锁
> 消息传递：管道，socket，好处是实现简单，但是复制内容有 overhead

* communication link：
![[image-249.png]]
![[image-250.png]]
* questions
![[image-251.png]]

### Direct Communication
* name each other explicit
* one pair, automatically established
![[image-252.png]]


### Indirect Communication
* use ports
> Each mailbox has a unique id
> Processes can communicate only if they share a mailbox

* create, send/recv, destroy mailboxes instead of PID
![[image-253.png]]
* Mailbox sharing is good (multiple process use one mailbox), yet who gets the message?

![[image-254.png]]
### Synchronization
* 同步
![[image-255.png]]
* sender 和 receiver 到底需不需要等待

