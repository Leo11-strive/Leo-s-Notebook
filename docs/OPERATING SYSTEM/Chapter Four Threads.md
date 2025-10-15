* when some code in the process need to wait
* set time limit (what if we lower the time bound?)
> Faster response
* what if we check every time to switch between different task in a single process? (Whether there is data)
> Lots of check, not efficient
![[image-256.png]]

## Thread
![[image-257.png]]
* 并发执行
* 进程中有自己的资源和共享的资源
![[image-258.png]]

* 同样也会有 content switch
* benefits：
> 一个核心可以配备多组寄存器，就是所谓超线程（20 核心 24 线程）
![[image-259.png]]
> 可交互性变强
> 相比进程，overhead 小，进一步增加并发能力
> 异步，同样需要线程

> [!info] Comparison: Concurrency vs Parallelism
> ![[image-260.png]]
> 单核，多个任务执行充分利用, concurrency
> 多核，多任务利用，parallel, 如果 parellel 一定 concurrency



## User Threads
* 开辟指令流
![[image-261.png]]
* User Level Thread（逻辑载体）
* 最开始的时候，没有 os based thread manipulation，指令流一旦出现 exception，直接进程中断进入 waiting
* 有了 kernel 态的线程处理之后，CPU 开始调度线程（物理载体）

![[image-262.png]]

## Multithreading Models
### Many-to-one
* 一个 kernel thread，一个 process
![[image-263.png]]
* 意义不大

### One-to-One
![[image-264.png]]
* 一一绑定
![[image-265.png]]
* 当线程过于多之后，KLT 会很多，这个进程的压力就很大

### Many-to-Many Model
* 多对多
![[image-266.png]]
* 用的多的，trap 多的，一对一，用的少的，多对一
* Linux
![[image-267.png]]


## Two-level Model
![[image-268.png]]
![[image-269.png]]


## Thread Issues
* `fork()`， `exec`:
> 跟 OS 的实现有关，但是细节会发生变化
![[image-270.png]]
* 意为着 `exec` 会删除所有当前进程的内容，包括所有线程

## Thread Cancellation
![[image-271.png]]

## Signal Handling
![[image-272.png]]

## Thread Pooling

![[image-273.png]]
* 提前创建线程
* 方便 many to many 调度

## Thread Specific Data
![[image-274.png]]
* local 信息

## Scheduler Activation
* Both M: M and Two-level models require communication to maintain the appropriate number of kernel threads allocated to the application
![[image-275.png]]
