## 实验步骤
### 准备工作
* 首先是要把用户段代码放入内存，给链接器 guidence
```lds
.data : ALIGN(0x1000) {
        _sdata = .;

        *(.sdata .sdata*)
        *(.data .data.*)

        _edata = .;

        . = ALIGN(0x1000);
        _sramdisk = .; 
        *(.uapp .uapp*)
        _eramdisk = .;
        . = ALIGN(0x1000);
    } >ramv AT>ram
```
* makefile 加入 user
```makefile
all: clean

$(MAKE) -C lib all

$(MAKE) -C init all

$(MAKE) -C user all

$(MAKE) -C arch/riscv all

@echo -e '\n'Build Finished OK

///只需注意arch/ariscv放在最后链接即可

clean:

$(MAKE) -C lib clean

$(MAKE) -C init clean

$(MAKE) -C user clean

$(MAKE) -C arch/riscv clean

$(shell test -f vmlinux && rm vmlinux)

$(shell test -f vmlinux.asm && rm vmlinux.asm)

$(shell test -f System.map && rm System.map)

@echo -e '\n'Clean Finished
```

### 创建用户态进程
#### 结构体更新
* 首先更新 `thread_struct`
```c
/* 线程状态段数据结构 */

struct thread_struct {

uint64_t ra;

uint64_t sp;

uint64_t s[12];

uint64_t sepc, sstatus, sscratch;

};

  

/* 线程数据结构 */

struct task_struct {

uint64_t state; // 线程状态

uint64_t counter; // 运行剩余时间

uint64_t priority; // 运行优先级 1 最低 10 最高

uint64_t pid; // 线程 id

  

struct thread_struct thread;

uint64_t *pgd; // 用户态页表

};
```
> 加入 `uint64_t sepc, sstatus, sscratch;` 保存中断权限，中断返回和用户态 sp，加入每个进程单独的页表


#### 修改 task_init
* 加入新的寄存器设置，而且还要分别创建页表
* sret 之后直接跳出中断，回到 user mode 所以 SPP 需要设置成 0
```c
uint64_t SPP=~(1UL<<8);

assist&=SPP;
```
* `sscratch` 保存 u mode 的 sp，我会用它来判断是否是用户进程
```c
//user process
task[i]->thread.sscratch=sp;

//kernel process
idle->thread.sscratch=0;
```
* 页表项：先是 kernel 的虚拟地址映射，我们为了在异常时无需切换页表，复制内核内存页表
```c
uint64_t *pgdir = (uint64_t *)kalloc();

for(int j=0;j<512;j++)

{

pgdir[j]=swapper_pg_dir[j]; // 将页表根目录复制到每个线程的页表中

}
```
* 接下来是 user code 的页表映射
```c
Elf64_Ehdr *elf_header = (Elf64_Ehdr *)&_sramdisk;

uint8_t *elf = (uint8_t *)&_sramdisk; // 注意：按字节访问

Elf64_Phdr *phdrs = (Elf64_Phdr *)(elf + elf_header->e_phoff);

uint64_t assist_mem=USER_START;

// for (int i = 0; i < elf_header->e_phnum; ++i)

// {

// Elf64_Phdr *phdr = phdrs + i;

// if (phdr->p_type == PT_LOAD)

// {

// uint64_t *segment_start=phdr->p_vaddr;

// uint64_t *segment_end=(uint64_t *)(phdr->p_vaddr)+phdr->p_memsz;

// uint64_t offset=phdr->p_offset;

// uint64_t filesz=phdr->p_filesz;

// uint64_t memsz=phdr->p_memsz;

// uint64_t *new_page=(uint64_t *)alloc_pages(1);

// uint64_t *whether_end=segment_start;

// uint64_t *assist2=new_page;

// while(1)

// {

// *assist2=whether_end;

// assist2++;

// whether_end++;

// if(whether_end>segment_end)

// break;

// }

// create_mapping(pgdir, assist_mem, new_page,phdr->p_memsz, 0x1F);

// assist_mem+=phdr->p_memsz;

// }
```
> 我们先复制一份，然后用这个复制的用户代码做映射即可


* 还需要设置一个用户态的栈，给虚拟内存尾部设置映射即可
```c
uint64_t *stack_pa=(uint64_t *)alloc_pages(1);

create_mapping(pgdir,USER_END-PGSIZE,(uint64_t)stack_pa-PA2VA_OFFSET,PGSIZE,0x17);

task[i]->pgd=pgdir; // 设置页表根目录
```

### 修改 `__switch_to`
* 这个主要是寄存器的储存多了 sepc，sscratch 和 sstatus
> sscratch 在 trap 的内部都保持 0
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

csrr t0, sepc

sd t0, 144(a0)

csrr t1, sstatus

sd t1, 152(a0) // save caller's sstatus， 152

// csrr t2, sscratch

// sd t2, 160(a0) // save caller's sscratch， 160

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

ld t0, 144(a1)

csrw sepc, t0

ld t1, 152(a1)

csrw sstatus, t1

ld t2, 160(a1)

csrw sscratch, t2
```
* 接下来是切换页表, 和 lab 3 的 satp 处理方法一致
```c
ld t3, 168(a1) // 加载 pgd（虚拟地址）

li t4, 0xffffffdf80000000 // PA2VA_OFFSET = VM_START - PHY_START

sub t3, t3, t4 // 转换为物理地址

srli t3, t3, 12 // 右移12位得到 PPN

li t4, (8 << 60) // Sv39 模式

or t3, t3, t4 // 组合成 satp 值

csrw satp, t3

sfence.vma zero, zero

ret // jump back to the thread about to run
```

### 更新中断处理逻辑
#### 修改 `__dummy`
* 因为__dummy 是新 switch 的进程，所以没有上下文，而且既然 sepc 已经设置成 user，所以可以直接跳转即可，但是我需要先把 sscratch 改成 sp
```c
__dummy:

csrr sp, sscratch

sret
```

#### 修改 `_traps`
* 有一个切换内核栈的过程，以及一个条件跳转过程
> 暂时通过 sscratch 储存 t0，这样可以保证原本的 t0 保存下来
* 同时保存用户态的 sp
* 但是 sscratch 为 0 的没有这些过程
* 而在从中断返回时，我检查当前的 sstatus 中的第 8 位是否为 1 来判断是否是内核态进程
```c
_traps:

# 1. save 32 registers and sepc to stack

# 2. call trap_handler

# 3. restore sepc and 32 registers (x2(sp) should be restore last) from stack

# 4. return from trap

# addi sp, sp, -16

# sd t0, 0(sp)

# sd t1, 8(sp)

# la t0, current

# ld t0, 0(t0)

# ld t1, 160(t0)

csrrw t0, sscratch, t0

beq t0, zero, _from_kernel

la t0, current

ld t0, 0(t0)

sd sp, 160(t0)

ld sp, 40(t0)

csrr t0, sscratch

csrw sscratch, zero

_from_kernel:

addi sp, sp, -34*8 # make space on stack

sd x1, 0(sp)

sd x2, 8(sp)

sd x3, 16(sp)

sd x4, 24(sp)

sd x5, 32(sp)

sd x6, 40(sp)

sd x7, 48(sp)

sd x8, 56(sp)

sd x9, 64(sp)

sd x10, 72(sp)

sd x11, 80(sp)

sd x12, 88(sp)

sd x13, 96(sp)

sd x14, 104(sp)

sd x15, 112(sp)

sd x16, 120(sp)

sd x17, 128(sp)

sd x18, 136(sp)

sd x19, 144(sp)

sd x20, 152(sp)

sd x21, 160(sp)

sd x22, 168(sp)

sd x23, 176(sp)

sd x24, 184(sp)

sd x25, 192(sp)

sd x26, 200(sp)

sd x27, 208(sp)

sd x28, 216(sp)

sd x29, 224(sp)

sd x30, 232(sp)

sd x31, 240(sp)

csrr t0, sepc

sd t0, 248(sp) # save sepc to stack

csrr a1, sepc

csrr a0, scause # get scause

mv a2, sp

call trap_handler # call trap_handler(a0=scause)

# restore registers

ld t0, 248(sp) # restore sepc to t0

csrw sepc, t0 # write back sepc

ld x1, 0(sp)

ld x3, 16(sp)

ld x4, 24(sp)

ld x5, 32(sp)

ld x6, 40(sp)

ld x7, 48(sp)

ld x8, 56(sp)

ld x9, 64(sp)

ld x10, 72(sp)

ld x11, 80(sp)

ld x12, 88(sp)

ld x13, 96(sp)

ld x14, 104(sp)

ld x15, 112(sp)

ld x16, 120(sp)

ld x17, 128(sp)

ld x18, 136(sp)

ld x19, 144(sp)

ld x20, 152(sp)

ld x21, 160(sp)

ld x22, 168(sp)

ld x23, 176(sp)

ld x24, 184(sp)

ld x25, 192(sp)

ld x26, 200(sp)

ld x27, 208(sp)

ld x28, 216(sp)

ld x29, 224(sp)

ld x30, 232(sp)

ld x31, 240(sp)

ld x2, 8(sp)

addi sp, sp, 34*8

csrrw t0, sscratch, t0

csrr t0, sstatus

andi t0, t0, (1 << 8)

bnez t0, _from_kernel_2

la t0, current

ld t0, 0(t0)

sd sp, 40(t0)

ld sp, 160(t0)

csrr t0, sscratch

csrw sscratch, sp

sret

_from_kernel_2:

csrr t0, sscratch

csrw sscratch, zero

sret
```

#### 修改 trap_handler
* 因为在 user mode 的 ecall 作用时产生中断，而内核态的 ecall 会直接进入 M 态执行，所以我们需要修改 trap_handler
* 我们通过之前保存的参数来确定调用 ecall 时的参数，从而确定调用的是什么函数
```c
void trap_handler(uint64_t scause, uint64_t sepc, struct pt_regs *regs){

// 通过 `scause` 判断 trap 类型

// 如果是 interrupt 判断是否是 timer interrupt

// 如果是 timer interrupt 则打印输出相关信息，并通过 `clock_set_next_event()` 设置下一次时钟中断

// `clock_set_next_event()` 见 4.3.4 节

// 其他 interrupt / exception 可以直接忽略，推荐打印出来供以后调试

if(scause == 0x8000000000000005) // timer interrupt (最高位1表示中断，低位5表示S-mode timer)

{

// printk("This is a timer interrupt\n");

clock_set_next_event(); // 设置下一次时钟中断

do_timer();

}

else if(scause == 8) // Environment call from U-mode (ecall)

{

if(regs->regs[16] == 172)

{

sy_getpid(regs);

}

else if(regs->regs[16] == 64)

{

sy_write(regs);

}

regs->sepc += 4; // ecall 指令长度为4，需要跳过

}
```
* 但是实现的时候要注意，我选择之前传入具体的数值而不是地址
```c
#include "syscall.h"

#include "proc.h"

#include "sbi.h"

#include "defs.h"

extern struct task_struct* current;

void sy_getpid(struct pt_regs *regs)

{

int pid = current->pid;

regs->regs[9] = pid;

}

  

void sy_write(struct pt_regs *regs)

{

if(regs->regs[9]==1)

{

// char* assist_pointer=kalloc();

// char* assist1=assist_pointer;

char* assist2=regs->regs[10];

int num=regs->regs[11];

while(num-- >0)

{

// if(*assist2=='\0') // 如果遇到字符串结束符

// {

// *assist1='\0'; // 在 assist_pointer 处添加字符串结束符

// break; // 退出循环

// }

// *assist1=*assist2; // 将 assist2 的内容复制到 assist_pointer

// assist1++; // 移动 assist_pointer 到下一个位置

// assist2++; // 移动 assist2 到下一个位置

if(*assist2=='\0')

break;

sbi_debug_console_write_byte((uint8_t)*assist2);

assist2++;

}

// sbi_debug_console_write(assist_pointer, assist1 - assist_pointer); // 调用 sbi_debug_console_write 输出字符串 regs->regs[9] = (uint64_t)regs->regs[11];

regs->regs[9] = (uint64_t)regs->regs[11];

}

}
```

### 调整时钟中断
* “这里保持 sstatus.SIE 为 0 可以在 S 态禁用中断来防止被时钟中断打断”，我们把 sstatus. SIE 设置为 0 从而禁止嵌套中断的问题

### 二进制文件测试
* 结果正确
```c
Boot HART MEDELEG         : 0x0000000000f0b509
...buddy_init done!
...mm_init done!
...task_init done!
2024 ZJU Operating System
schedule: next pid = 2, counter = 10
[U-MODE] pid: 2, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 2, sp is 0x3fffffffe0, this is print No.2
schedule: next pid = 8, counter = 10
[U-MODE] pid: 8, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 8, sp is 0x3fffffffe0, this is print No.2
[U-MODE] pid: 8, sp is 0x3fffffffe0, this is print No.3
schedule: next pid = 30, counter = 10
[U-MODE] pid: 30, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 30, sp is 0x3fffffffe0, this is print No.2
[U-MODE] pid: 30, sp is 0x3fffffffe0, this is print No.3
schedule: next pid = 10, counter = 9
[U-MODE] pid: 10, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 10, sp is 0x3fffffffe0, this is print No.2
schedule: next pid = 12, counter = 9
[U-MODE] pid: 12, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 12, sp is 0x3fffffffe0, this is print No.2
schedule: next pid = 27, counter = 9
[U-MODE] pid: 27, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 27, sp is 0x3fffffffe0, this is print No.2
[U-MODE] pid: 27, sp is 0x3fffffffe0, this is print No.3
schedule: next pid = 29, counter = 9
[U-MODE] pid: 29, sp is 0x3fffffffe0, this is print No.1
[U-MODE] pid: 29, sp is 0x3fffffffe0, this is print No.2
[U-MODE] pid: 29, sp is 0x3fffffffe0, this is print No.3
```

### 添加 ELF 解析与加载
* ELF 的文件中包含了每个 segment 所要放置的虚拟地址
* 我们需要按照要求，把 segment 复制到正确物理位置并且按照 ELF 文件的要求做好虚拟地址映射
* 要注意的是地址的变量，到底是按照字节访问还是按照 bit 访问
```c
Elf64_Ehdr *elf_header = (Elf64_Ehdr *)&_sramdisk;

uint8_t *elf = (uint8_t *)&_sramdisk; // 注意：按字节访问

Elf64_Phdr *phdrs = (Elf64_Phdr *)(elf + elf_header->e_phoff);
```

```c
for (int i = 0; i < elf_header->e_phnum; ++i)

{

Elf64_Phdr *ph = &phdrs[i];

if (ph->p_type != PT_LOAD)

continue;

  

uint64_t vaddr = ph->p_vaddr; // 目标虚拟地址

uint64_t offset = ph->p_offset; // 文件内偏移

uint64_t filesz = ph->p_filesz; // 文件中这段的大小

uint64_t memsz = ph->p_memsz; // 内存中需要的总大小（含bss）

  

// 1) 计算需要分配的页数

uint64_t va_start = PGROUNDDOWN(vaddr);

uint64_t va_end = PGROUNDUP(vaddr + memsz);

uint64_t seg_len = va_end - va_start;

int pages = seg_len / PGSIZE;

  

// 2) 分配物理页并清零

uint8_t *pa = (uint8_t *)alloc_pages(pages);

memset(pa, 0, seg_len); // 先全零，保证 bss 为 0

// uint64_t *address=ph->p_vaddr;

for (uint64_t i = 0; i < filesz; ++i)

{

pa[(vaddr - va_start) + i] = elf[offset+ i];

}

// 4) 把这块物理页映射到用户页表上对应的虚拟地址范围

create_mapping(pgdir,va_start, // 段的起始虚拟地址（页对齐）

(uint64_t)pa - PA2VA_OFFSET, // 物理地址

seg_len,

0x1F); // R/W/X/U

}

task[i]->thread.sepc = elf_header->e_entry;
```


* 同样可以正确运行

## 思考题
1. 我们在实验中使用的用户态线程和内核态线程的对应关系是怎样的？（一对一，一对多，多对一还是多对多，言之有理即可）
> 一对一，因为我们的每个用户态线程进入内核态处理中断的时候，都成为了一个单独的内核态线程，即便是地址映射一致但是内核态的 sp 也完全不一致，而且同时作为同一个单元进行调度

2. 系统调用返回为什么不能直接修改寄存器？
> 因为这些寄存器会直接被异常返回覆盖

3. 针对系统调用，为什么要手动将 sepc + 4？
> 因为已经完成了这个 systemcall，可以进行下一个指令了，防止中断循环

4. 为什么 Phdr 中，`p_filesz` 和 `p_memsz` 是不一样大的，它们分别表示什么？
> `.data` 的那一部分，需要在文件中实际保存内容，这部分计入 `p_filesz` 和 `p_memsz`。`.bss` 的那一部分，在 ELF 文件中是 **不占空间** 的（只知道“要有这块内存，而且初始要是 0”）这部分 **只计入 `p_memsz`，不计入 `p_filesz`**。

5. 为什么多个进程的栈虚拟地址可以相同？用户态程序有没有方法知道栈所在的物理地址？
> 因为栈空间在初始化的时候已经正确分配，虚拟地址相同也不会有冲突。我认为是没有办法的


## 总结
* 对与用户态进程的调度过程有了一定的理解，真的是非常充实的 lab
* 遇到了不少问题，uint 64_t 和 uint 8_t 的使用问题，system call 传入参数的问题，以及保护用户 context 一定要精细
* ai 使用上，用于学习知识和代码补写