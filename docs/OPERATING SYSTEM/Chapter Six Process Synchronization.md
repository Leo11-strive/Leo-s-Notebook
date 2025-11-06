* Concurrent access to shared data may result in data inconsistency
* Maintaining data consistency requires mechanisms to ensure the orderly execution of cooperating processes
> 进程之间的数据依赖
* Cooperating process can affect or be affected by the execution of another process
![[image-409.png]]
* 可以加 count，但是 count 的问题是也要注意这两个调用的 consistency
![[image-410.png]]
![[image-411.png]]

![[image-412.png]]
* $S3$ 之后又出现了 producer 的写入请求，如何处理呢？
* A race condition is a situation in which a memory location is accessed concurrently, and at least one access is a write.
![[image-413.png]]
* 还是会产生进程中断

## Critical section problem

![[image-513.png]]
* 就像是上锁
![[image-514.png]]
* 全局共享变量是临界资源
* 只读没有写，就是那个值，可以同时读取
* CPU 中的调度部分共用的寄存器也算是临界资源

## Solution to critical-section problem
* 互斥，一个在 execute，另一个进程不能访问
* 空闲让进，If no process is executing in its critical section and there exist some processes that wish to enter their critical section, then the selection of the processes that will enter the critical section next cannot be postponed indefinitely
![[image-515.png]]
![[image-516.png]]
![[image-517.png]]


## 软件实现
* 单标志法
![[image-518.png]]
![[image-519.png]]
* mutual exclusion: 只有一个能够进入这个 critical section
* 为什么 progress 会有问题？
> 因为 $P_j$ 如果把 turn 设置为 i，再一次循环，turn 还是 i（在 $P_i$ 还没有执行完一轮的情况下）
* 单标志法的特点是交替访问
* 当前的代码，一直在 while，没有释放

## 双标志法
![[image-520.png]]

![[image-521.png]]

* 存在一直在循环的状态
* 肯定不满足 bounded waiting（时间上），当两个进程能够执行完一个，另一个也可以，bounded waiting 的判断标准不一
* 同样没有让权

## 双标志先检查法
![[image-522.png]]
* 同时进入 critical section


## Peterson's solution
![[image-523.png]]
* ld, store 原子性，因此硬件解决 turn 和 flag 的写入问题
![[image-524.png]]

![[image-525.png]]
* 针对两个进程
* There are no guarantees that Peterson's solution works correctly on modern computer architectures.
* 当前的计算机架构可能会有编译打乱的情况，而且因为只支持两个进程
![[image-526.png]]

## Bakery algorithm
![[image-527.png]]

![[image-528.png]]

![[image-609.png]]

* for 语句来检查所有的进程，是否有在取号，如果有，等待
* 筛选循环直到进程完成临界区访问


## Synchronization Hardware
![[image-611.png]]

### Disable interrupts
* 直接关闭中断
![[image-612.png]]

![[image-613.png]]

![[image-614.png]]
* 如果都是 true，循环
* 如果是 false，自己设置成 true，然后开始运行
* 不满足 bounded waiting

## Swap instruction
![[image-615.png]]

![[image-616.png]]

![[image-617.png]]

## CAS
![[image-618.png]]

![[image-619.png]]

![[image-620.png]]

![[image-621.png]]
## Mutex Locks
![[image-622.png]]

![[image-623.png]]
* 阻塞唤醒方式
* 自旋锁：自己检查
> 短临界区域，只要让他不断检查即可
> 无上下文切换，不需要调度器


![[image-624.png]]

## Semaphore
![[image-625.png]]

![[image-626.png]]

![[image-627.png]]
* 有序操作

![[image-628.png]]
