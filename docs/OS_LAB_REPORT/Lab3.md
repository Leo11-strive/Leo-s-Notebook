## 实验步骤
### 实验准备
* `defs.h` 中添加的内容，定义了虚拟地址的启始和结束，以及 offset
```c
#define OPENSBI_SIZE (0x200000)

#define VM_START (0xffffffe000000000)
#define VM_END (0xffffffff00000000)
#define VM_SIZE (VM_END - VM_START)

#define PA2VA_OFFSET (VM_START - PHY_START)
```
* makefile 修改，因为编译器的优化，在 PIE设置虚拟地址的代码中，GOT 读出的始终是虚拟地址，为了防止 PIE 接管，所以要加上 `-fno-pie` 阻止编译 PIE 代码


### 开启虚拟内存映射
* `setup_vm`
> 因为有 9 位数，而且我们只进行 1 GB 的映射，虚拟地址是从 `0x80000000` 做一个映射，对应那九位数是 `2`, 而 `0xffffffe000000000` 对应的那九位数字是 `384`，因此做映射即可，主要我出错的地方就是格式，因为映射的格式是留出 10 位作为权限位。
```c
void setup_vm() {

/*

* 1. 由于是进行 1GiB 的映射，这里不需要使用多级页表

* 2. 将 va 的 64bit 作为如下划分： | high bit | 9 bit | 30 bit |

* high bit 可以忽略

* 中间 9 bit 作为 early_pgtbl 的 index

* 低 30 bit 作为页内偏移，这里注意到 30 = 9 + 9 + 12，即我们只使用根页表，根页表的每个 entry 都对应 1GiB 的区域

* 3. Page Table Entry 的权限 V | R | W | X 位设置为 1

**/

uint64_t va_start = VM_START;

uint64_t pa_start = PHY_START;

// for(int i=0;i<256;i++)

// {

// uint64_t pa=pa_start+(i*0x40000000);

// early_pgtbl[i]=((pa>>12)<<10)| 0xF; // V R W X

// early_pgtbl[i+256]=((pa>>12)<<10)| 0xF; // V R W X

// }

uint64_t pa=pa_start;

early_pgtbl[2]=((pa>>12)<<10)| 0xF; // V R W X，映射虚拟到物理地址同时留出10位

early_pgtbl[384]=((pa>>12)<<10)| 0xF; // V R W X

}
```
* 接下来需要 relocate 函数完成 satp 的设置
```riscv
_start:

la sp, boot_stack_top # initialize sp

la t1, _traps # load the address of _traps

csrw stvec, t1 # set stvec to _traps， for the trap call of OpenSBI

call setup_vm

call relocate
```
* 我在 stack 初始化之后调用虚拟地址初始化，是因为我需要调用函数
* 因为 mm_init 之前已经初始化了虚拟地址，因此，它访问的地址是虚拟地址，因此要做修改
```c
void mm_init(void) {

kfreerange(_ekernel, (char *)(VM_START +

PHY_SIZE));// 因为我们映射了 1GiB 的物理内存，所以这里的结束地址是 VM_START + PHY_SIZE

printk("...mm_init done!\n");

}
```

* `setup_vm_final` 实现
> 多级页表的实现
> 不需要等值映射，但是现在因为已经使用虚拟地址了，所以要注意转换，所有代码访问的内存，必须都是虚拟地址。
> 还有就是 other memory 不可以漏掉
```c
uint64_t swapper_pg_dir[512] __attribute__((__aligned__(0x1000)));

  
  

void setup_vm_final() {

memset(swapper_pg_dir, 0x0, PGSIZE);

  

// No OpenSBI mapping required

  

// mapping kernel text X|-|R|V

uint64_t kernal_text_size = (uint64_t)(_etext - _stext); // 计算内核文本段大小

uint64_t kernal_text_pa = (uint64_t)_stext - PA2VA_OFFSET; // 计算内核文本段物理地址

create_mapping(swapper_pg_dir, (uint64_t)_stext, kernal_text_pa, kernal_text_size, 0xB); // X R V

  

// mapping kernel rodata -|-|R|V

uint64_t kernal_rodata_size = (uint64_t)(_erodata - _srodata);// 计算内核只读数据段大小

uint64_t kernal_rodata_pa = (uint64_t)_srodata - PA2VA_OFFSET;// 计算内核只读数据段物理地址

create_mapping(swapper_pg_dir, (uint64_t)_srodata, kernal_rodata_pa, kernal_rodata_size, 0x3); // R V

// mapping kernel data and bss -|W|R|V

uint64_t kernal_data_size = (uint64_t)(_ebss - _sdata);// 计算内核数据段大小

uint64_t kernal_data_pa = (uint64_t)_sdata - PA2VA_OFFSET;// 计算内核数据段物理地址

create_mapping(swapper_pg_dir, (uint64_t)_sdata, kernal_data_pa, kernal_data_size, 0x7); // W R V

  

// mapping other memory -|W|R|V

uint64_t other_memory_pa = (uint64_t)_ebss - PA2VA_OFFSET; // 计算其他内存的起始物理地址

uint64_t other_memory_size = PHY_END - other_memory_pa;// 计算其他内存的大小

create_mapping(swapper_pg_dir, (uint64_t)_ebss, other_memory_pa, other_memory_size, 0x7); // W R V

  

// set satp with swapper_pg_dir

uint64_t pgtbl_physical_address = ((uint64_t)swapper_pg_dir) - PA2VA_OFFSET; // 根页表的物理地址

uint64_t satp_value = (8ULL << 60) | (pgtbl_physical_address >> 12);// MODE=8 for Sv39， 设置 satp 寄存器的值

csr_write(satp, satp_value);

// YOUR CODE HERE

  

// flush TLB

asm volatile("sfence.vma zero, zero");

return;

}
```
> 不存在页表的时候就要创建，而且要加入 round up/down 加强鲁棒性，每一个最底层的 page 都要进行映射，保证没有缺漏
```c
/* 创建多级页表映射关系 */

/* 不要修改该接口的参数和返回值 */

void create_mapping(uint64_t *pgtbl, uint64_t va, uint64_t pa, uint64_t sz, uint64_t perm) {

/*

* pgtbl 为根页表的基地址

* va, pa 为需要映射的虚拟地址、物理地址

* sz 为映射的大小，单位为字节

* perm 为映射的权限（即页表项的低 8 位）

*

* 创建多级页表的时候可以使用 kalloc() 来获取一页作为页表目录

* 可以使用 V bit 来判断页表项是否存在

**/

uint64_t va_assist = va & (~(PGSIZE - 1)); // 页对齐

uint64_t pa_assist = pa & (~(PGSIZE - 1));

sz=PGROUNDUP(pa + sz) - pa_assist; // 调整 sz，确保覆盖所有需要映射的物理地址

uint64_t levels[3];

  

for(uint64_t offset = 0; offset < sz; offset += PGSIZE)

{

uint64_t current_va = va_assist + offset;

uint64_t current_pa = pa_assist + offset;

// 计算三级页表的索引

levels[2] = (current_va >> 30) & 0x1FF; // 9 bits for level 2

levels[1] = (current_va >> 21) & 0x1FF; // 9 bits for level 1

levels[0] = (current_va >> 12) & 0x1FF; // 9 bits for level 0

// 处理 level 2

if ((pgtbl[levels[2]] & 0x1) == 0) // 如果映射不存在，检查 V bit

{

uint64_t *new = (uint64_t*)kalloc();

memset(new, 0, PGSIZE);

pgtbl[levels[2]] = (((uint64_t)new-PA2VA_OFFSET) >> 12 << 10) | 0x1; // V bit，同时放置新页表的物理地址

}

// 处理 level 1

uint64_t *level1 = (uint64_t*)(((pgtbl[levels[2]] >> 10) << 12)+PA2VA_OFFSET);// 得到 level1 页表的虚拟地址

if ((level1[levels[1]] & 0x1) == 0) // 如果映射不存在，检查 V bit

{

uint64_t *new = (uint64_t*)kalloc();// 分配新页表

memset(new, 0, PGSIZE);

level1[levels[1]] = (((uint64_t)new-PA2VA_OFFSET) >> 12 << 10) | 0x1; // V bit

}

// 处理 level 0

uint64_t *level0 = (uint64_t*)(((level1[levels[1]] >> 10) << 12)+PA2VA_OFFSET); // 得到 level0 页表的虚拟地址

level0[levels[0]] = ((current_pa >> 12) << 10) | perm; // 设置 PTE

}

}
```
> 具体实现注释应该比较清晰

### 编译测试
![[image-754.png]]
* 是正确的


## 思考题
1. 验证 `.text`，`.rodata` 段的属性是否成功设置，给出截图。
* text 段：
![[image-755.png]]

* 分配位置（正确）：
![[image-756.png]]

2. 为什么我们在 `setup_vm` 中需要做等值映射？在 Linux 中，是不需要做等值映射的，请探索一下不在 `setup_vm` 中做等值映射的方法。
   * 本次实验中如果不做等值映射，会出现什么问题，原因是什么  
   > 在写 satp 前，PC 还是物理地址，而在写了 satp 之后，下一条指令取址时，或者说前几行代码，PC 就被当成了虚拟地址，走页表，因此需要两个映射，要不然直接会出现（只有执行改变控制流的指令时，才会改变 PC，如中断，跳转）
      instruction page fault

   * 简要分析 [Linux v5.2.21](https://elixir.bootlin.com/linux/v5.2.21/source) 或之后的版本中的内核启动部分（直至 `init/main.c` 中 `start_kernel` 开始之前），特别是设置 satp 切换页表附近的逻辑；
     > 首先设置虚拟地址，物理基址，大小，偏移。初始化一些早期页表（trampoline_pg_dir），回到 relocate，在返回地址上加上偏移，保证后续 ret 使用虚拟地址，把 stvec 设置为 satp 之后的下一条指令的虚拟地址
     > ```c
     > la   a2, 1f        
     > add  a2, a2, a1    
     > csrw stvec, a2
     > ```
     > 先计算真正的内核页表 satp 数值，但是不写入，而是先写一个 trampoline，在这个时候，下一条取指令的时候，还是没变过的 PC 会使用这个 trampoline 来索引物理地址，就会触发中断，跳转到 stvec，也就是正确的虚拟地址指引的下一条指令。接下来就是写入真正的 satp，然后运行。 %%
     


   * <font color="#000000">回答 Linux 为什么可以不进行等值映射，它是如何在无等值</font><font color="#000000">映射的情况下让 pc 从物理地址跳到虚拟地址</font>
     > 通过 trampoline 的这个辅助映射，中断调节 PC 到虚拟地址
     


   * Linux v5.2.21 中的 `trampoline_pg_dir` 和 `swapper_pg_dir` 有什么区别，它们分别是在哪里通过 satp 设为所使用的页表的
    > 就像上面写的那样，trampoline_pg_dir 本身就是为了把 CPU 迁移到虚拟地址，swapper_pg_dir 才是长期使用的页表，在 `arch/riscv/mm/init.c` 里有这两个全局变量，在 relocate 里面，trampoline 被写入。而 swapper_pg_dir 在 `arch/riscv/mm/init.c` 里，`setup_vm_final()` 的最后写入
    

   * 尝试修改你的 kernel，使得其可以像 Linux 一样不需要等值映射。
> 	    同样先设置一个类似的 trampoline_pg_dir，写一个简单的映射，也就是我们的 setup_vm, 然后先设置好映射之后，stvec 设置为写完 satp 之后的下一行虚拟地址，触发中断，这样在触发一次异常 / 跳转之后，PC 才会变成正确的高虚拟地址。PC 就是正确的虚拟地址了。然后再调用 setup_vm_final 即可；或者拆开 setup_vm，写一个简单的最小映射，足够让 stvec 跳转回来之后走到 setup_vm 我们写的初始化过程即可，然后最后跳出中断. 我的实现是在 lab 2_copy 代码中，是第一种实现，不确定对不对，但是确实能正确运行。


## 总结
* 非常好的一个 lab，对虚拟地址的设置过程有了深入的了解。
* 遇到的问题：之前 mm_init 的上界写错，导致内存分配错误；La 和 li 没分清；在设置页表的时候数字的移动问题；在设置映射的时候 2 和 384 两个数字算错...
* ai 使用在了提问和补写上