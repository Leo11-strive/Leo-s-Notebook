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
