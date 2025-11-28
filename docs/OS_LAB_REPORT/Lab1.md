## 实验步骤
### RV 64 内核引导
#### 完善 Makefile 脚本
```makefile
C_SRC = $(sort $(wildcard *.c)) # All C source files in the current directory

OBJ = $(patsubst %.c,%.o,$(C_SRC)) # Object files corresponding to the C source files

  

all:$(OBJ) # Default target to build all object files

%.o:%.c

${GCC} ${CFLAG} -c $<

# Compile each C source file into an object file

clean:

$(shell rm *.o 2>/dev/null)

# Clean up object files
```
* `lib/Makefile` makefile 子程序编写，把所有的 `.c` 文件编译成 `.o` 
#### 编写 head.S
* 为第一个 C 函数设置程序栈（这里会有一个 timer 函数的 call 和 start kernel 的 call），只需要给 sp 寄存器赋予 stack 开始时的地址即可，这一部分的代码如下：
```RV64
_start:
    la sp, boot_stack_top # initialize sp
```

```RV64
    .section .bss.stack
    .globl boot_stack
    
boot_stack:
    .space 4096 # <-- change to your stack size

    .globl boot_stack_top
boot_stack_top:
```

#### 补充 `sbi.c`
* ecall 函数需要 Debug Console Extension的 ID， FunctionID，以及参数的数值，按照正确的方式，存到对应的寄存器中即可，之后就可以用 sbi_ecall 调用函数：
```c
struct sbiret sbi_ecall(uint64_t eid, uint64_t fid,

uint64_t arg0, uint64_t arg1, uint64_t arg2,

uint64_t arg3, uint64_t arg4, uint64_t arg5) {

struct sbiret return_value; // 用于存储返回值

uint64_t error, value; // 用于存储 ecall 返回的 error 和 value

asm volatile( // ecall 调用，使用asm嵌入的方式

"mv a7 , %[eid] \n"

"mv a6 , %[fid] \n"

"mv a0 , %[arg0] \n"

"mv a1 , %[arg1] \n"

"mv a2 , %[arg2] \n"

"mv a3 , %[arg3] \n" // 传递参数到寄存器

"mv a4 , %[arg4] \n"

"mv a5 , %[arg5] \n"

"ecall \n"

"mv %[error], a0 \n"

"mv %[value], a1 \n"

: [error] "=r" (error), [value] "=r" (value) // 从寄存器获取返回值

: [eid] "r" (eid), [fid] "r" (fid),[arg0] "r" (arg0), [arg1] "r" (arg1), [arg2] "r" (arg2),[arg3] "r" (arg3), [arg4] "r" (arg4), [arg5] "r" (arg5) // 传递参数到寄存器

);

return_value.error = error;

return_value.value = value;

return return_value; // 返回 sbiret 结构体

}
```
* 接下来是之后要调用的即可 M mode 函数的封装实现
```c
struct sbiret sbi_debug_console_write_byte(uint8_t byte) { // byte 是要写入的字节

return sbi_ecall(0x4442434e, 2, byte, 0, 0, 0, 0, 0); // 调用 sbi_ecall，传递参数

}

  

struct sbiret sbi_system_reset(uint32_t reset_type, uint32_t reset_reason) {

return sbi_ecall(0x53525354, 0, reset_type, reset_reason, 0, 0, 0, 0); // 重置系统（关机或重启）

}

  

struct sbiret sbi_set_timer(uint64_t time_constant) {// time_constant 是下一个时钟中断的时间点

return sbi_ecall(0x54494d45, 0, time_constant, 0, 0, 0, 0, 0);

}

  

struct sbiret sbi_debug_console_write(char *input, uint64_t length) { // input 是要写入的字符串，length 是字符串长度

return sbi_ecall(0x4442434e, 0, (uint64_t)input, length, 0, 0, 0, 0);

}

  

struct sbiret sbi_debug_console_read(char *output, uint64_t length) { // output 是读取到的字符串存放的位置，length 是要读取的长度

return sbi_ecall(0x4442434e, 1, (uint64_t)output, length, 0, 0, 0, 0);

}
```

#### 修改 `defs`
* 这个只需要读出来储存就好了
```c
#define csr_read(csr) \

({ \

uint64_t __v; \// 定义一个变量 __v 用于存储读取的值

asm volatile("csrr %0, " #csr : "=r"(__v) : : "memory"); \ // 读取 CSR 寄存器的值到 __v 变量中

__v; \

})
```

### RV 64 时钟中断处理
#### 开启 `trap` 处理
* 这里要在 `head.S` 中，首先把 `stvec` 手动设置到我们编写的中断处理程序，这样发生中断之后硬件就会直接跳转。
* sstatus 的 SIE 设置为 1，这样能够处理中断（当然会有硬件的 SIE 与 SPIE 的交换）
* sie 设置为时钟中断（第六位）
* 再完成第一次时钟中断
```c
la t1, _traps # load the address of _traps
   csrw stvec, t1 # set stvec to _traps， for the trap call of OpenSBI
   li t0, (1<<5)
   csrs sie, t0 # enable the timer interrupt
   rdtime t1 # read the current time
   li t2, 10000000 # set the timer interval
   add t1, t1, t2 # set the next timer interrupt time
   mv a0, t1
   call sbi_set_timer # set the timer
   li t0, (1<<1)
   csrs sstatus, t0 # enable the interrupt
```

#### 实现上下文切换
* 保存上下文（所有寄存器）
* 传入 trap handler 再恢复，然后返回
```RV64
    .extern trap_handler
    .section .text.entry
    .align 2
    .globl _traps 
_traps:
    # 1. save 32 registers and sepc to stack
    # 2. call trap_handler
    # 3. restore sepc and 32 registers (x2(sp) should be restore last) from stack
    # 4. return from trap
    addi sp, sp, -32*8 # make space for 32 registers
    csrr a1, sepc # save sepc to a1
    csrr a0, scause # save scause to a0
    sd x1, 0(sp) # save registers to stack
    sd x3, 8(sp)
    sd x4, 16(sp)
    sd x5, 24(sp)
    sd x6, 32(sp)
    sd x7, 40(sp)
    sd x8, 48(sp)
    sd x9, 56(sp)
    sd x10, 64(sp)
    sd x11, 72(sp)
    sd x12, 80(sp)
    sd x13, 88(sp)
    sd x14, 96(sp)
    sd x15, 104(sp)
    sd x16, 112(sp)
    sd x17, 120(sp)
    sd x18, 128(sp)
    sd x19, 136(sp)
    sd x20, 144(sp)
    sd x21, 152(sp)
    sd x22, 160(sp)
    sd x23, 168(sp)
    sd x24, 176(sp)
    sd x25, 184(sp)
    sd x26, 192(sp)
    sd x27, 200(sp)
    sd x28, 208(sp)
    sd x29, 216(sp)
    sd x30, 224(sp)
    sd x31, 232(sp)
    sd x2, 240(sp)
    sd a1, 248(sp) 
    call trap_handler # call trap_handler
    ld x31, 232(sp)
    ld x30, 224(sp)
    ld x29, 216(sp)
    ld x28, 208(sp)
    ld x27, 200(sp)
    ld x26, 192(sp)
    ld x25, 184(sp)
    ld x24, 176(sp)
    ld x23, 168(sp)
    ld x22, 160(sp)
    ld x21, 152(sp)
    ld x20, 144(sp)
    ld x19, 136(sp)
    ld x18, 128(sp)
    ld x17, 120(sp)
    ld x16, 112(sp)
    ld x15, 104(sp)
    ld x14, 96(sp)
    ld x13, 88(sp)
    ld x12, 80(sp)
    ld x11, 72(sp)
    ld x10, 64(sp)
    ld x9, 56(sp)
    ld x8, 48(sp)
    ld x7, 40(sp)
    ld x6, 32(sp)
    ld x5, 24(sp)
    ld x4, 16(sp)
    ld x3, 8(sp)
    ld x1, 0(sp)
    ld x2, 240(sp)
    ld a1, 248(sp)
    addi sp, sp, 32*8 # restore sp
    csrw sepc, a1 # restore sepc
    sret # return from trap

```
#### 实现 `trap` 处理函数
* trap handler 实际上就是要设置时钟中断到下一个时间点
* 需要分辨 scause 中储存的是异常还是中断
```c
void trap_handler(uint64_t scause, uint64_t sepc){

// 通过 `scause` 判断 trap 类型

// 如果是 interrupt 判断是否是 timer interrupt

// 如果是 timer interrupt 则打印输出相关信息，并通过 `clock_set_next_event()` 设置下一次时钟中断

// `clock_set_next_event()` 见 4.3.4 节

// 其他 interrupt / exception 可以直接忽略，推荐打印出来供以后调试

if(scause>>63 ==0)

{

printk("This is an exception\n");

}

else if(scause==0x8000000000000005) // ai提供的写法，我最开始用的是scause-0x8000000000000000==32

{

printk("This is a timer interrupt\n");

clock_set_next_event(); // 设置下一次时钟中断

}

else

{

// printk("This is an interrupt\n");

}

}
```

#### 实现时钟中断相关函数

* 得到时间 `rdtime`
* 然后根据当前时间设置下一次时钟中断
```c
// QEMU 中时钟的频率是 10MHz，也就是 1 秒钟相当于 10000000 个时钟周期

uint64_t TIMECLOCK = 10000000;

  

uint64_t get_cycles() {

// 编写内联汇编，使用 rdtime 获取 time 寄存器中（也就是 mtime 寄存器）的值并返回

uint64_t result;

asm volatile(

"rdtime %0"

: "=r"(result)

);

return result;

}

  

void clock_set_next_event() {

// 下一次时钟中断的时间点

uint64_t next = get_cycles() + TIMECLOCK;

  

// 使用 sbi_set_timer 来完成对下一次时钟中断的设置

sbi_set_timer(next);

}
```
## 最终结果
```bash
2024 ZJU Operating System
kernel is running!
kernel is running!
kernel is running!
This is a timer interrupt
kernel is running!
kernel is running!
kernel is running!
kernel is running!
kernel is running!
This is a timer interrupt
kernel is running!
kernel is running!
kernel is running!
kernel is running!
This is a timer interrupt
kernel is running!
kernel is running!
kernel is running!
kernel is running!
This is a timer interrupt
kernel is running!
kernel is running!
kernel is running!
```
* 是正确的

## 思考题
* 1. 请总结一下 RISC-V 的 calling convention，并解释 Caller / Callee Saved Register 有什么区别？
> 返回地址：ra， sp 栈指针，gp 全局指针，tp 线程指针，t 0-t 6 临时寄存器，s 0-s 11 寄存器，函数参数/返回值 a 0-a 7，x 0 零寄存器
> Caller saved：在调用之前，主程序保存 `t0–t6` 和 `a0–a7`
> Callee saved: 在调用之后，进入函数，在函数中保存s0–s11
* 2. 编译之后，通过 System.map 查看 vmlinux.lds 中自定义符号的值并截图。
![[image-163.png]]

* 3. 用 `csr_read` 宏读取 `sstatus` 寄存器的值，对照 RISC-V 手册解释其含义并截图。
> 我在 test 中加上了 `int status = csr_read(sstatus); // 读取 sstatus 寄存器的值，防止优化掉循环`, 这样的话，刚开始输出会是 SIE=1， SPIE=0，而第一次中断处理之后，每一次的中断之后应该都是 SIE=1， SPIE=1
![[image-164.png]]
* 4. 用 `csr_write` 宏向 `sscratch` 寄存器写入数据，并验证是否写入成功并截图。
> 我选择把 sstatus 的寄存器数值写入 sscratch，代码如下
```c
#include "printk.h"

#include "defs.h"

void test() {

int i = 0;

while (1) {

if ((++i) % 100000000 == 0) {

printk("kernel is running!\n");

int status = csr_read(sstatus); // 读取 sstatus 寄存器的值，防止优化掉循环

csr_write(sscratch, status); // 写回 sstatus 寄存器的值，防止优化掉循环

int scratch = csr_read(sscratch); // 读取 sscratch 寄存器的值

printk("sscratch: %d\n", scratch);

i = 0;

}

}

}
```

> 结果正确

![[image-165.png]]

* 5. 详细描述你可以通过什么步骤来得到 `arch/arm64/kernel/sys.i`，给出过程以及截图。
> 先利用 make 生成基于 arch 64 的 config 文件
![[image-166.png]]
> 然后在编译时 specify 要生成的文件以及路径

![[image-167.png]]
> 验证：

![[image-168.png]]

* 6. 寻找 Linux v6.0 中 ARM32 RV32 RV64 x86_64 架构的系统调用表；
    - 请列出源代码文件，展示完整的系统调用表（宏展开后），每一步都需要截图。
> 展示初始状态下的 `tbl` 文件以及 c 数组
> `x86_64`

![[image-169.png]]
![[image-171.png]]
> 方法就是生成编译文件 `syscall_64.i` `make ARCH=x86 CROSS_COMPILE=x86_64-linux-gnu- arch/x86/entry/syscall_64.i `
> `ARM32`

![[image-170.png]]
![[image-173.png]]
![[image-174.png]]
> `RV32`, `RV64` 因为我使用的是 linux 6.16.8, 所以它们使用统一系统调用符号表
![[image-172.png]]


* 7. 阐述什么是 ELF 文件？尝试使用 readelf 和 objdump 来查看 ELF 文件，并给出解释和截图。
    - 运行一个 ELF 文件，然后通过 `cat /proc/PID/maps` 来给出其内存布局并截图。
> ELF 是一个包含文件基本信息的可执行与可链接格式，而非单纯的二进制执行文件
![[image-175.png]]
> 观察 header，包含魔数，架构类型，entry point address 等等基本信息

![[image-176.png]]
> 内存映射信息

![[image-177.png]]
> 链接时的 header

![[image-178.png]]
> 符号表

> 还可以观察详细的反汇编

![[image-179.png]]

> 运行一个 ELF 文件

![[image-180.png]]
> 内存映射，xp 是可执行文件 `.text` 字段，而 `rw--p` 是 `data` 字段

* 8. 通过查看 [RISC-V Privileged Spec](https://github.com/riscv/riscv-isa-manual/releases/download/20240411/priv-isa-asciidoc.pdf) 中的 `medeleg` 和 `mideleg` 部分，解释上面 `MIDELEG` 和 `MEDELEG` 值的含义。
> 这是两个 csr（machine mode），所有的中断，异常，本身产生于 M
> 而部分异常，可以让 s mode 处理
> 0x222 = 0b10_0010_0010： M 模式委托了所有 U/S 模式的软件中断、时钟中断、外部中断。  
但 M 模式自身的中断（MSI/MTI/MEI）仍由 OpenSBI 自己处理。
> 0xB109 = 0b1011_0001_0000_1001
> 常见的用户空间异常、页错误、系统调用都被委托给 S 模式。而硬件故障类（访问错误、M 模式异常）仍由 OpenSBI（M 模式）处理

## 总结
* 真的是获益匪浅的一个 lab，知识量非常大
* ai 提供补写，查找知识点的作用
* 遇到的问题，实验中，我把所有的寄存器都在 caller 保存了，后来才查找 riscv 的 convention
* 思考题中，arm 架构的 table 是 calls-eabi. S, 因此单独写了一个转换宏（c 语言，借助 ai），完成宏展开的展示
* 对 kernel，trap 的基本流程有了理解