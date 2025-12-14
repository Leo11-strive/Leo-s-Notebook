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

* 页面置换算法
![[image-951.png]]
* 降低 page fault
> 尽可能的低
* module
> 对算法输入的建模
> 输入是一系列逻辑地址的组合
> Offset+page number
> 作简化，保留页号，只关心页号

![[image-952.png]]
* 因为前后访问同一个页面（中间没有间隔），可以缩减成一个
* 给一个 process 一定数量的 frame，个数越多，缺页的概率越低
![[image-953.png]]

## FIFO
* 最早放入 frame，最早被换出来
![[image-954.png]]
* 为什么会出现 frame 越多，page fault 反而更多的问题？
> 主要是根据 fifo，没有充分利用 page 的使用情况（n 个 frame，n+1 个数据 page）

## Optimal Algorithm
* page fault 产生最少
![[image-955.png]]
* 这个用来判断一个置换算法是否好，作为下界

## LRU
![[image-956.png]]
* 选择页面哪个是最久未使用的
* 记录历史
![[image-957.png]]
* 最近访问时间，clock 最小
* 通过一个 stack
![[image-958.png]]
* 访问之后提到栈顶，需要改变的指针量是很多的
![[image-959.png]]
* LRU approximation algorithms
![[image-960.png]]
* 增加访问位
* 近似的 LRU
> 0：相对而言，上个周期内没有被访问过

## Enhanced Second Chance Algorithm
![[image-961.png]]
* 加入了一个修改位
* 这样的循环只需要两个循环


## Count-based Alogorithms
![[image-962.png]]
* LFU：缺少 localization
* MFU

## Page Buffering Algorithm
![[image-963.png]]
* windows，linux 的页面置换算法是基于这个的


## Allocation of Frames
![[image-964.png]]
* how to allocate?
* fixed allocation
> 平均分配
> 按比例分配：根据 process 占据的比例
* 进程大小是指？
> 虚拟地址空间的大小，在执行文件中存在
![[image-965.png]]

* 优先级高的，需要更快结束，因此分配更多 frame
* 可以从优先级更低的进程置换
## Global vs Local Allocation
![[image-966.png]]
* 局部 replace：只能从分配给你的 frame 置换
> 不够动态，但是可控，实现更 simple
* global：可以从其他进程的页面中置换
> 对资源的利用率更高（利用空闲 frame），但是会影响其他进程，但是用的更多
![[image-967.png]]

## Thrashing
* 抖动
![[image-968.png]]
* 需要的页面
* 增加物理内存
* 优化页面置换算法
* 工作集算法
* etc
* Demand Paging and Thrashing
![[image-969.png]]
* threshing occur
![[image-970.png]]

## Working-Set Model
* Δ ≡ working-set window ≡ a fixed number of page references
![[image-1006.png]]
* 是 reference 的集合
* 集合的元素大小作为 working set size
![[image-1007.png]]

![[image-1008.png]]
* 需要做停止
* 做工作集追踪
![[image-1009.png]]

![[image-1010.png]]

![[image-1011.png]]

![[image-1012.png]]

## Memory-Mapped Files
![[image-1014.png]]

![[image-1013.png]]

![[image-1015.png]]

![[image-1016.png]]

![[image-1017.png]]

## Buddy System
![[image-1018.png]]

![[image-1019.png]]

![[image-1020.png]]

![[image-1021.png]]

![[image-1022.png]]

![[image-1023.png]]

![[image-1024.png]]

![[image-1025.png]]

![[image-1026.png]]






