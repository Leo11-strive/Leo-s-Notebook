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
