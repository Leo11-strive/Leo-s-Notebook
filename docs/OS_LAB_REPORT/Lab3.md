## 实验步骤
### 准备工程
* 内存管理系统初始化
```c
after_set_sie:

rdtime t1 # read the current time

li t2, 10000000 # set the timer interval

add t1, t1, t2 # set the next timer interrupt time

mv a0, t1

after_set_set_timer:

call sbi_set_timer # set the timer

li t0, (1<<1)

call mm_init

call task_init

csrs sstatus, t0 # enable the interrupt
```
> 这样设置，保证 mm_init 一定会在线程调度之前创建好

* 初始化自定义宏
```c
#define PHY_START 0x0000000080000000
#define PHY_SIZE 128 * 1024 * 1024 // 128 MiB，QEMU 默认内存大小
#define PHY_END (PHY_START + PHY_SIZE)

#define PGSIZE 0x1000 // 4 KiB
#define PGROUNDUP(addr) ((addr + PGSIZE - 1) & (~(PGSIZE - 1)))
#define PGROUNDDOWN(addr) (addr & (~(PGSIZE - 1)))
```
> 这里的 `pagesize` 我们分配了 `4` kb
> `pageroundup` 就是用来计算下一个 page 的开始地址，也就是十六进制中，保留后四位进一
> `pagerounddown` 就是把后四位至 0

### 线程调度功能实现
* 初始化线程
```c
idle = (struct task_struct *)kalloc(); // 分配一个物理页

idle->state=TASK_RUNNING; // 设置状态为运行中

idle->counter=0; // 设置计数器为 0

idle->priority=0; // 设置优先级为 0

idle->pid=0; // 设置进程 ID 为 0

current=idle; // 将当前进程指针指向 idle

task[0]=idle; // 将任务数组的第一个元素指向 idle
```
> 这个是操作系统内核初始化线程，因为它不参与调度
```c
for(int i=1;i<NR_TASKS;i++)

{

task[i]=(struct task_struct *)kalloc(); // 为每个线程分配一个物理页

task[i]->state=TASK_RUNNING; // 设置状态为运行中

task[i]->priority=rand()%(PRIORITY_MAX-PRIORITY_MIN+1)+PRIORITY_MIN; // 设置优先级为随机数

task[i]->counter=0; // 设置计数器为 0

task[i]->pid=i; // 设置进程 ID 为 i

task[i]->thread.ra=(uint64_t)&__dummy; // 设置返回地址为 __dummy 的地址

task[i]->thread.sp=PGROUNDDOWN((uint64_t)task[i])+PGSIZE; // 设置栈指针为物理页的高地址

}
```
> 这里我们随机设置 priority，这里其实是设置 CPU burst time
> 然后使用 long burst first 算法
> 接下来我们需要设置第一个 ra，作为第一次从 trap 跳转的地址


### `__dummy` 和 `dummy` 的实现
* dummy 的实现，主要的点是，之所以要在 1 的时候减去 1, 是因为，如果这个一留到中断的时候剪掉，那么问题就是不会 printk 了，所以提前剪掉
* “当我们创建一个新的线程，此时线程的栈为空，当这个线程被调度时，是没有上下文需要被恢复的，所以我们需要为线程**第一次调度**提供一个特殊的返回函数 `__dummy`”
> 这里就是说，我们的返回直接到完全新的线程中
* 所以需要更换 `sepc` ，跳转为一个特殊的地址
```c
__dummy:

la t1, dummy

csrw sepc, t1

sret
```


### 实现线程切换
* 在我们的调度执行之后，我们需要切换上下文
* 先保存，再恢复
```c
__switch_to:

sd ra, 32(a0) // save caller's ra， 32 bytes offset

sd sp, 40(a0) // save caller's sp， 40 bytes offset

sd s0, 48(a0) // save caller's s0， 48 bytes offset

sd s1, 56(a0) // save caller's s1， 56 bytes offset

sd s2, 64(a0) // save caller's s2， 64 bytes offset

sd s3, 72(a0) // save caller's s3， 72 bytes offset

sd s4, 80(a0)

sd s5, 88(a0)

sd s6, 96(a0)

sd s7, 104(a0)

sd s8, 112(a0)

sd s9, 120(a0)

sd s10, 128(a0)

sd s11, 136(a0)

ld ra, 32(a1) // restore callee's ra

  

ld sp, 40(a1)

ld s0, 48(a1)

ld s1, 56(a1)

ld s2, 64(a1)

ld s3, 72(a1)

ld s4, 80(a1)

ld s5, 88(a1)

ld s6, 96(a1)

ld s7, 104(a1)

ld s8, 112(a1)

ld s9, 120(a1)

ld s10, 128(a1)

ld s11, 136(a1) // restore callee's s11

  

ret // jump back to the thread about to run
```

### 实现调度入口函数
* 我们等到这个正在运行的线程结束再进行调度
```c
void do_timer()

{

  

if(current==idle||current->counter==0) // 当前是 idle 进程或者当前进程的 counter 已经为 0

{

schedule(); // 调度下一个进程

return;

}

current->counter--;

if(current->counter==0) // 当前进程的 counter 变为 0

{

schedule();

}

  

return;

}
```

线程调度算法实现
* 我们需要调度的是，当前 counter 最大的情况
```c
void schedule()

{

struct task_struct *next; // 下一个要运行的线程

int assist_max=0; // 辅助变量，记录最大的 counter 值

while(1)

{

for(int i=1;i<NR_TASKS;i++)

{

if(task[i]->counter>assist_max) // 找到 counter 最大的线程

{

assist_max=task[i]->counter; // 更新辅助变量

next=task[i]; // 更新下一个要运行的线程

}

}

if(assist_max>0) // 找到 counter 大于 0 的线程

break;

for(int i=1;i<NR_TASKS;i++) // 所有线程的 counter 都为 0，重新分配 counter

{

task[i]->counter=task[i]->priority;

}

}

switch_to(next);

}
```

## 编译与测试
* 运行测试代码之后，输出是正确的
```c
This is a timer interrupt
[PID = 2] is running. auto_inc_local_var = 19
This is a timer interrupt
[PID = 2] is running. auto_inc_local_var = 20
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 8
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 9
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 10
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 11
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 12
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 13
This is a timer interrupt
[PID = 1] is running. auto_inc_local_var = 14
This is a timer interrupt
[PID = 3] is running. auto_inc_local_var = 5
Test passed!
    Output: 2222222222111111133334222222222211111113

```

## 思考题
* 在 RV64 中一共有 32 个通用寄存器，为什么 `__switch_to` 中只保存了 14 个？
> 因为我们在 trap 进入的时候已经保存了所有的寄存器，因此，我们的__switch_to 的目的就是跳转到那个我们接下来要运行的进程的 trap 恢复部分，因此只要 store 和 load 当前 switch 函数的环境即可
* 阅读并理解 `arch/riscv/kernel/mm.c` 代码，尝试说明 `mm_init` 函数都做了什么，以及在 `kalloc` 和 `kfree` 的时候内存是如何被管理的
> 