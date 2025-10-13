* the base: frame
* design issues
* error detection and correction
* protocals
* flow control

## Design Issues
* 给网络层，作为承载的作用
![[image-138.png]]
* unacknowledged connectionless services
* acknowledged connectionless services
* acknowledge connection-oriented services

### Framing
* no guarantee for error free
* how to break the bit stream into frame
![[image-139.png]]
* 一旦出错，全都错了
![[image-140.png]]
* 如果有效数据中也有 flag 这个 pattern
* 那么需要给有效数据加上修饰，转义字符
![[image-141.png]]
* 混合
![[image-142.png]]
### Error Control
![[image-143.png]]

### Flow Control
* 传输速度要快于接收方接受的速度，如何处理？
![[image-144.png]]





## Error Detection And Correction
![[image-145.png]]
### Hamming Distance
* 不同位的个数
* n=m+r（原始加上冗余）
* 还可以定义在整个编码上，任何两个正确码字之间最短的汉明距离
![[image-146.png]]
* 奇偶校验：一位出错

### How to detect and correct errors?
* 发送的时候要有冗余的
* 接收方分开
* 再进行检测
* 汉明距离，如果出错的位数小于，那么能够 detect
* 改错：需要错误的位数小于汉明距离的二分之一

![[image-181.png]]

* 设计能够纠错一位错误的编码：
> 对**每一个**合法码字，我们必须“专门分配”给它一共 $n+1$ 个接收模式：

> 1. **0 个错误**：码字自身（1 个模式）。
    
> 1. **1 个错误**：在n个位置的任意一个发生单比特翻转（n个模式）。
    

> 合计 n+1个接收模式，都必须**解码回这同一个码字**（这就是“dedicated to it”的含义：这些模式被划给这个码字来纠错）, 这些汉明球不能有任何重叠

> 这样就可以求出校验位的下界


## Error Correcting

![[image-182.png]]

* block codes: 原来的数据流，分成 block，block 的 data bits 计算出对应的 r check bit




