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
![[image-413.png|630x65]]
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
* 信号量处理
![[image-625.png]]

![[image-626.png]]
* 互斥访问
> 全局共享信号量 S，wait 和 signal 在同一个进程中
> 一般初始化为 1
![[image-627.png]]
* 有序操作
> 同步访问，一个运行结束，发送信号，P 2 捕捉，继续运行
![[image-628.png]]

* 假设四个房间，有四个钥匙，每一把都可以打开所有房间
> 一个信号即可，设置为这个房间数量
* 每个房间都有一个独特的钥匙
> 用户需要每个房间都尝试一遍，设置四个信号量来解决问题，每个房间一个。保证当一个用户在对某一个房间试探访问的时候，申请就是不同的 key，所以 1 个 number 无法区分

### **Semaphore Implementation**
* Must guarantee that no two processes can execute wait () and signal () on the same semaphore at the same time 自己也是临界区的问题
![[image-629.png]]
* 自旋锁
* 对 CPU 资源存在浪费

![[image-630.png]]

![[image-631.png]]

### Semaphore Implementation with no Busy waiting
![[image-632.png]]
* S 代表着 waiting 的 process 的数量
![[image-633.png]]
 
 
![[image-634.png]]
 * 会出现无效 wakeup
 * 原子性是处理器必须执行这个程序不允许有中断打扰，scope 仅限当前处理器
 * count 是有效资源数量
 * 有了资源之后再来取用
![[image-635.png]]
* 但是你给 P 加锁，它就在 sleep 阶段无法释放锁，只有有人 wakeup 它，它才会醒来释放锁，但是因为锁住了，没有人能 count+1，所以一直锁住了
![[image-636.png]]
* mark as sleep 之后，先释放锁，再进入 sleep
* 被唤醒之后，是重新执行 P 的方式
![[image-637.png]]

 

![[image-638.png]]

![[image-639.png]]
![[image-640.png]]
* 等待的不是资源空闲

## Bounded-Buffer Problem
* N buffers, each can hold one item
* Mutex
* 顺序访问？
> 前提是 buffer 里面有产品，因此需要顺序
![[image-641.png]]
* 信号量数量，初始化
![[image-642.png]]
* 同步 wait 一定要在互斥之前，防止死锁

## Readers-Writers Problem
![[image-643.png]]
* 共享数据集，read count
* integer read count is 0
* semaphore mutex is initialized to 1 (对 readcount 进行互斥的访问)
* semaphore wrt 则是互斥的写锁
> 写者比较简单，直接用 semaphore 锁即可
![[image-644.png]]
* mutex：对读者数量的计算
* 如果有读者要读取数据集，作为第一个读的读者，无法判断是否在写，所以上锁，是否读由第一个读的人控制

## Dining-Philosophers Problem
![[image-732.png]]
* 设置 5 个信号量
* 五个筷子
![[image-733.png]]
* 所以说需要两个筷子
> 如果 5 个科学家一人一个筷子，如何处理呢？
* 可以只允许 4 个人一块儿吃饭
* 一位反序拿筷子，这样也是不会出现 5 个科学家一人一个筷子
![[image-734.png]]
* 先 wait 再 signal

## Monitor
* Only one process may be active within the monitor at a 
* 它把一组“共享数据 + 操作函数”包起来，并且**一次只允许一个线程在里面运行**（互斥是隐含的）。

* 在监视器外面有一个 **entry queue**（入口队列）：想进来的线程先在这里排队；

* 在监视器内部，每个条件变量（如 x、y）各自有一条**条件等待队列**，只存放“因为等某个条件而暂时让出监视器”的线程。
![[image-735.png]]

![[image-736.png]]

![[image-737.png]]

![[image-738.png]]
* 有可能会 starve
