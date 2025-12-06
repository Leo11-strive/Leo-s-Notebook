* virtual memory
> 采用虚拟内存
![[image-915.png]]
* 虚拟地址可以比内存物理地址更多

### 局部性原理
![[image-916.png]]

![[image-917.png]]

* virtual-address space
![[image-918.png]]
* 空闲的虚拟地址空间
> 可以把其他的地址映射过来，比如 libraries
* hole 占据的空间很大
> Heap stack，动态连接 routing，对于一个 process，这个 hole 会很大
* 能够带来哪些好处
> 内存共享
* 进程创建
> 加速，因为不需要把进程所需要的所有代码都 load 到内存中
* copy on write
> 写时复制
![[image-919.png]]
* 只有写的时候才会只用复制
![[image-920.png]]

* memory-mapped files
> 文件内容映射到进程的地址空间，可以像访问内存一样访问文件，无需显式的读写操作，提高文件访问性能

* demand paging
![[image-921.png]]
* 到用的时候才会 load 进来
![[image-922.png]]
* 为什么要连续的磁盘空间？
> 从效率上来说，便于读取

* valid-invalid bit
> Valid bit 的意思是：v：在 memory 里，i：并不在 memory 里面，但是出现了 i：到底是没有分页还是在硬盘里
![[image-923.png]]

![[image-924.png]]

* page fault+判断是否是不在内存里面或者是没有做映射（有一个专门的 table in PCB 来做这个）
![[image-925.png]]
* 出现 page fault，那么这个 process 进入 waiting state
* 缺页中断返回之后可能会发生错误（或者说不必要的行为）
> 比如一个 copy 的 instruction，解决缺页中断返回之后所有的都要被重新 copy 一遍了，所以 page fault 中断是可以增加减少某些指令的位置的
![[image-926.png]]

![[image-927.png]]

## Performance of demand paging
![[image-928.png]]

![[image-929.png]]
* EAT：非常大

* what happens if there is no free frame?
> Swap 的时候怎么选 swap 回硬盘的？


![[image-931.png]]
* 不会首选 dirty page，因为 dirty page 要写回，需要时间


