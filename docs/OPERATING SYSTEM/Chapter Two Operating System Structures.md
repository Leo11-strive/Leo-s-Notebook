* OS kernel, ultilize encapsulation, I/O etc is too complex
* exec: 常见的系统调用，执行程序（替换）
* 如何做装修
## Note
* Read the textbook
* appearance->insider

## Operating System Services
* User interfaces
* Program execution
* I/O operations
* File system manipulations
* Resource allocation
* Accounting
* Protection and Security

## UI
* CLI
* GUI

## System Calls
* API 和 systemcall 的区别：API 是更加 high level 的封装
* trap (软中断)
![[image-54.png]]

> [!note] example: if no api
> ![[image-57.png]]

> [!info] printf
> ![[image-59.png]]




* user are using API

* > abstraction: 便利，可移植性

## System Call Implementation
* a number associated (mature, small)


![[image-55.png]]

![[image-58.png]]

* single CPU


## System Call Parameter Passing
* 参数（已经在 system call 里写好了）
![[image-56.png]]
* regs
* in memory (give it to system call) 可能在不同的内存区域，传递有一定的麻烦
* trapframe
* put into a stack
![[image-60.png]]
* more complex management, yet memory and stack are large

## Types of System Calls
* `pipe()` 和 `fork()`

## System Programs
* System programs provide a convenient environment for program development and execution. (above kernel)

## Operating System Design and Implementation
* Design and Implementation of OS not “solvable”, but some approaches have proven successful
* define goals and specifications
* User goals – operating system should be convenient to use, easy to learn, reliable, safe, and fast
* System goals – operating system should be easy to design, implement, and maintain, as well as flexible, reliable, error-free, and efficient
* principles: Policy:  What will be done? Mechanism: How to do it? 
![[image-61.png]]


## Operating System Structure
* Monolithic Strcuture
![[image-62.png]]
![[image-64.png]]
* 宏内核
* Microkernel Structure
![[image-65.png]]
* > Mac OS: two microkernel 
![[image-66.png]]


* Darwin
> 结合两者的优势
> ![[image-90.png]]

* 微内核接口加上 BSD（丰富的接口），一块儿和 kernel 交互
* 如何做到？对整个系统调用优化（减少系统调用开销），共享内存，消息传递，etc（中断次数减少）
* windows 早期是宏内核，而现在已经有微内核的一些特性
* wsl 1 的时候是有解释层的，wsl 2 类似一个虚拟机，完整的 linux 内核
![[image-91.png]]

* application framework: swift 编程语言，开发框架，sdk

## Layered Approach
* The operating system is divided into a number of layers (levels), each built on top of lower layers.  The bottom layer (layer 0), is the hardware; the highest (layer N) is the user interface.
![[image-92.png]]


* With modularity, layers are selected such that each uses functions (operations) and services of only lower-level layers
* 每一层独立开发，抽象
* 层与层之间的依赖

## Modules
* Solaris Modular Approach
> ![[image-93.png]]
* loadable kernel module `.ko`: 和内核地址打通，内核可以直接调用，on demand 加载
* object oriented

## Other Structures
![[image-94.png]]
* 定制 library
* Unikernel: linked with the OS code needed
  * fast，for specific functions

## Virtual Machines
* A _virtual machine_ takes the layered approach to its logical conclusion. It treats hardware and the operating system kernel as though they were all hardware
* A virtual machine provides an interface _identical_ to the underlying bare hardware
* 本质是一种分层
* The operating system creates the illusion of multiple processes, each executing on its own processor with its own (virtual) memory
* 本质上那个虚拟机软件是一个在用户态下运行的
![[image-95.png]]

* 这里的 virtual-machine implementation 是一个进程（hypervisor）
* ![[image-96.png]]
* 裸机就是直接安装在 hardware 上
* 另一种就是在 host os 之上

## Java Virtual Machine
![[image-97.png]]

* 在任何系统上运行
* Docker 是一个容器
* > 定制化文件系统

