* 我使用的环境是 Ubuntu 24.04 原生环境
## 实验内容
### 环境搭建
* 因为是 ubuntu 原生环境，所以直接在上面完成 qemu 的安装。我使用的是 `linux 6.16.8`
### 获取 Linux 源码和已经编译好的文件系统
* `linux` 直接下载 .tar 文件，解压
* `.img` 文件按照要求正常克隆
### 编译 Linux 内核
```bash
make ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu- defconfig    # 使用默认配置
make ARCH=riscv CROSS_COMPILE=riscv64-linux-gnu- -j$(nproc)   # 编译
```

* 编译时，我的每个 CPU 核几乎占用率几乎都达到了 100%

### 使用 QEMU 运行内核
```bash
qemu-system-riscv64 -nographic -machine virt \                             
  -kernel ./arch/riscv/boot/Image \
  -device virtio-blk-device,drive=hd0 \
  -append "root=/dev/vda ro console=ttyS0 nokaslr" \
  -bios default \
  -drive file=../os-25fall/src/lab0/rootfs.img,format=raw,id=hd0 \

```
* `virt`: 虚拟开发板
* `kernel`: 未压缩扁平二进制
* `-device`: 接 `virt` ，作为硬盘文件系统的前端

### 使用 GDB 对内核进行调试
```bash
qemu-system-riscv64 -nographic -machine virt \                             ─╯
  -kernel ./arch/riscv/boot/Image \
  -device virtio-blk-device,drive=hd0 \
  -append "root=/dev/vda ro console=ttyS0 nokaslr" \
  -bios default \
  -drive file=../os-25fall/src/lab0/rootfs.img,format=raw,id=hd0 \
  -S -s

```
* `-S`: 停在初始 PC 处
* `-s`: 打开 1234 port，方便 gdb 远程调试
#### 具体步骤
```bash
gdb-multiarch ./vmlinux                                                    ─╯
GNU gdb (Ubuntu 15.0.50.20240403-0ubuntu1) 15.0.50.20240403-git
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./vmlinux...

This GDB supports auto-downloading debuginfo from the following URLs:
  <https://debuginfod.ubuntu.com>
Enable debuginfod for this session? (y or [n]) y
Debuginfod has been enabled.
To make this setting permanent, add 'set debuginfod enabled on' to .gdbinit.
Downloading separate debug info for /home/leo/Documents/Operating_System/linux-6.16.8/vmlinux
--Type <RET> for more, q to quit, c to continue without paging--c
(No debugging symbols found in ./vmlinux)                                       
(gdb) target remote :1234
Remote debugging using :1234
0x0000000000001000 in ?? ()
(gdb) b start_kernel
Breakpoint 1 at 0xffffffff80c006b0
(gdb) c
Continuing.

Breakpoint 1, 0xffffffff80c006b0 in start_kernel ()
(gdb) quit
A debugging session is active.

	Inferior 1 [process 1] will be detached.

Quit anyway? (y or n) y
Detaching from program: /home/leo/Documents/Operating_System/linux-6.16.8/vmlinux, process 1
Ending remote debugging.
[Inferior 1 (process 1) detached]

```
* 就是设置了远程连接，设置断点，continue 过去，再退出

## 思考题
### 1, 2. 使用 `riscv64-linux-gnu-gcc` 编译单个 `.c` 文件 && 使用 `riscv64-linux-gnu-objdump` 反汇编 1 中得到的编译产物
> [!note] 具体操作
> ![[image-13.png]]

* `Wall`: 打开常用 warning
* `O2`: 编译优化
* `-d`: 编译可执行段
* `-S`: 源码

> [!note] 编译结果
> ![[image-14.png]]


### 3. 调试 Linux 
* 所有提到的命令都在以下过程中出现了
> [!note] 查看汇编代码
>  ![[image-3.png]]
>  `layout asm`

> [!note] 在 `0x80000000` 处下断点 && 查看断点
> ![[image-5.png]]

> [!note] 在 `0x80200000` 处下断点
> ![[image-6.png]]

> [!note] 清除 `0x80000000` 处的断点
> ![[image-7.png]]


> [!note] 用 display 查看 pc 的位置并且一直展示下一个要执行的指令
> ![[image-4.png]]




> [!note] 继续运行直到触发 `0x80200000` 处的断点
> ![[image-8.png]]


> [!note] Info Reg
> ![[image-10.png]]


> [!note] bt, frame
> ![[image-11.png]]

> [!note] 单步调试
> ![[image-9.png]]


* `finish`: 持续执行这个函数直到返回，在 `start_kernel` 使用 ` finish kernel ` 就正常运行了

* `next`: 同样是单步调试，但是会直接运行完一整个函数


> [!note] 使用 make 工具清除 Linux 的构建产物
> ![[image-12.png]]

* `make clean`: 删除编译产物，但是保留配置文件
* `make mrproper`: 在删除编译产物的基础上，删除配置文件
* vmlinux 和 image 之间的区别：
>  Vmlinux 和 image 之间的关系与区别：vmlinux 是链接产物，包含了调试信息，而 image 就是扁平二进制，用来执行而没有任何调试信息的文件。而因为各种调试信息与地址都在 vmlinux 中，所以 gdb 会阅读其中的内容，然后通过远端连接，地址一致时，gdb 通过远程协议添加指令（断点，etc）


## 讨论与心得
* 这次的实验真的收获非常大。对于 qemu 的基本操作，gdb 的基本操作，在实验过程中，通过查找资料，询问 ai，逐渐学习
* gdb 的调试刚开始有点生涩，但是也同样感受到它的强大
* vmlinux 和 image 的区别，在使用 gdb 连接调试的时候就有这个疑问，询问 ai 得到解释，恰好有这一道思考题
* 遇到的问题是，layout asm 是没问题的，但是无法和 src 代码一起看
* 后来发现应该是编译的时候没有开启 `CONFIG_DEBUG_INFO=y`






## AI 使用
* 查阅资料
* 带着我逐渐学习 gdb





 
