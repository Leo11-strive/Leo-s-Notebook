* 如何从 ready queue 当中选择一个合适的进程运行
* 涉及到程序的同步，异步

## Basic Concept
![[image-276.png]]
* 尽量让 cpu 一直在使用
* 一般情况下，I/O burst 占据大多数

![[image-277.png]]
* 算法速度快

## CPU Scheduler
* 选择一个 ready queue 中的进程进行运行

![[image-278.png]]
* 主动放弃：非强占
> 进程运行结束，`wait()`，`I/O` request (需要等待其他资源)
* 被动放弃：强占
> 分配时间片中断，从 waiting 到 ready queue 有了新的 ready 进程之后，需要 OS 中断，OS 做调度
* 一般都是使用强占式（如果是非强占式，可能会被一个进程一直霸占 CPU）
* 对应的进程/线程，所以是 short-term 调度

## Dispatcher
* Dispatcher module gives control of the CPU to the process selected by the short-term scheduler（命令发出者）
![[image-279.png]]
> [!note]
> ![[image-280.png]]

## Scheduling Criteria
* 标准
> *CPU 利用率*
> *进程吞吐率：完成进程的数量（单位时间）*
> *周转时间，就是一个进程从 new 到 terminate*
> *带权重的周转时间*：加入 CPU 运行时间作为权重，周转时间除以运行时间（了解）
> *等待时间：在 ready queue 的等待时间*
> *响应时间：进程提交到首次执行的时间*



### First Come, First Serve
![[image-281.png]]
* 一些到达的顺序变化，就会有很大的性能变化
> 对于短进程，一旦排在后面，很不利
![[image-282.png]]

* 先来先服务显然不是一种很好的算法，因为顺序的改变会极大的影响结果，不过选择的方法很简单
* 产生护航效应
> 长进程后有小进程，那么这些小进程其实要等很长时间，超过 100%的等待时间
* 对于长进程有利（CPU burst long），如果长进程放在短进程之后，等待时间相对值比较少
* I/O-bound processes wait for the CPU-bound one


## Shortest-Job-First (SJF) Scheduling
* 对于每个进程，预先知道下一次 CPU burst 的时间
* Associate with each process the length of its next CPU burst.  Use these lengths to schedule the process with the shortest time
![[image-388.png]]

### Non-preemptive
* 非强占式，所以在 process 创建之后，直接排序
![[image-389.png]]
* 等运行一个 process 结束之后再调度
* 一个 process 来就调度
![[image-390.png]]
* 调度次数会比较多
> 调度会花费时间


*no way to know the length of the next burst*
* Can be done by using the length of previous CPU bursts, using exponential averaging (指数平均法)
![[image-391.png]]
* $\alpha$ 的意义
![[image-392.png]]
* 事实上，时间越长，越是这些进程之前内容信息的整合

* SJF is optimal – gives minimum average waiting time for a given set of processes

* starvation（部分 process 会一直得不到 CPU），not good for long process

## Priority scheduling
* The CPU is allocated to the process with the highest priority (smallest integer ≡ highest priority)
* Static Priority: determine when processes is created; do not change.

## Highest response ratio next
![[image-393.png]]
* weighting time 指的是到达到 schedule 的时间
![[image-394.png]]
* 当调度 priority 相同时，可以采用其他的调度方法

* HRRN is a compromise between FCFS and SJF


## Round robin
* Each process gets a small unit of CPU time (_time quantum_), usually 10-100 milliseconds.  After this time has elapsed, the process is preempted and added to the end of the ready queue.
* 每个人平等运行
![[image-395.png]]
* 快速开始运行
![[image-396.png]]

* sweet spot：时间长度要比 context switch 时间长很多
* 要保证在这个时间片里面 CPU burst 要完成
* no starvation
* SJF turnaround time faster than RR

## Multilevel queue
* 把一个队列分成多个队列
* 分别调度
* 前端，后端（分离）
> 前段有大量的交互（RR），而后端会有 batch 大量处理，长时间运行，要有最高的资源利用率（FCFS）
![[image-399.png]]
* 本身 queue 与 queue 之间也要进行调度
![[image-401.png]]
* 但是问题是，它的调度比较静态（队列之间没有交换），进程的 CPU consume 会发生变化

## Multilevel feedback queue
* A process can move between the various queues; aging can be implemented this way
![[image-402.png]]
![[image-403.png]]
* 多级反馈队列
![[image-404.png]]

* 也有可能 starve，长进程

## Multiple-processor scheduling
* 应该让进程在哪个处理器上跑？
* 我们需要让所有的 CPU 忙起来吗？
* 处理器之间的信息交流？
![[image-405.png]]
* 进程亲和度，一个进程尽量在一个 processor 上跑，以提高缓存命中率
* load balancing，每个 CPU 的 workload 差不多

## Real-Time scheduling
![[image-406.png]]
* 评价标准是可预测性（可以预测结束时间），可靠性（正确执行）
* 最早截止时间优先
* 最低松弛度优先
![[image-407.png]]
* Rate Monotonic Scheduling（速率单调调度）
* 是一种**实时调度算法（Real-Time Scheduling Algorithm）**，专门用于周期性任务（periodic tasks）的场景。
* 每个任务的优先级一开始就根据它的周期（period）决定了，运行过程中不会改变。
* 基于任务的周期来分配优先级，周期越短的任务优先级越高
* 抢占式


## Thread Scheduling
![[image-408.png]]
* 由**用户态线程库**决定：在**同一进程内部**，哪些用户线程（U-threads）能被放到**可用的 LWP**（LightWeight Process，内核线程/可调度实体）上运行。
* 由**内核调度器**决定：在全系统范围，哪个内核线程（K-thread/LWP)拿到某个 CPU 的下一次时间片。


