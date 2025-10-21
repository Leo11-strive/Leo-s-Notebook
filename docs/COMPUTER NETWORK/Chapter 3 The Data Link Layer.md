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

* Hamming Code 可以实现，就是上面提到的那个 lower bound
* block codes: 原来的数据流，分成 block，block 的 data bits 计算出对应的 r check bit
* **systematic code**: the m data bits are sent directly, along with the check bits
* linear code : the r check bits are computed as a linear function of the m data bits
* **code rate**, or simply rate, is the fraction of the codeword that carries information that is not redundant. 有用的占比

### Hamming Code
* 2 的倍数位置为 check，data 就是其他的
* Each check bit forces the parity of some collection of bits, including itself, to be even (or odd)
* 一个 bit 可以被多个 check bit 检查
![[image-221.png]]
* 对其做类似二进制的拆分
* 所有出错的 check bit 之和就是错误的 bit（只有一位错误的情况，因为，如果是比这个大的 bit 出的错的话，一定还有一个 invalid check bit，矛盾）
![[image-222.png]]


### Convolutional Code
* An encoder processes a sequence of input bits and generates a sequence of output bits.
* The output depends on the current and previous input bits.
* characterized by coding rate and constraint length
* previous input bits is called **constraint length**.
* add all memorized bits and mod 2
![[image-236.png]]


### Reed Solomon Codes
![[image-223.png]]
* why we can know the error position?
> Physical layer information, you can know the incorrect bits referencing 星座图（intuition)
![[image-237.png]]

### LDPC
* 用一小部分的输入位决定的，linear block code
*  practical for large block sizes and have excellent error-correction abilities that outperform many other codes (including the ones we have looked at) in practice.


## Correction
![[image-224.png]]
* less expensive
* 有些时候重新传输会比 correction 更优秀

![[image-225.png]]
![[image-226.png]]
* 一列一列传输，会更容易 detect

### Checksum
![[image-227.png]]
![[image-228.png]]

* 用**一补码加法（one’s‐complement addition）把要校验的比特流分成**16 位字**相加，再对结果**按位取反**作为 checksum。
![[image-238.png]]

### CRC

* 可以用来纠错，但是我们这里考虑检查错误
![[image-229.png]]
* 看成 polynomial
![[image-239.png]]
* 

![[image-230.png]]
![[image-231.png]]
![[image-232.png]]



![[image-233.png]]
* G (x) 的选择很重要
![[image-240.png]]
![[image-241.png]]

## Elementary data link protocols
* 链路层向网络层提供服务
![[image-357.png]]
* 先把 packet 变成 frame，传输这个数据包到另一个 link layer

![[image-358.png]]

![[image-359.png]]

![[image-360.png]]

![[image-361.png]]
* 上述是接口

![[image-362.png]]


* 一个完美的状态
![[image-363.png]]



* Simplex：error free channel assumption
* No flow control

### Protocal 2
* A simplex stop-and-wait protocol
* The communication channel is assumed to **be error free**.
* The sender is always ready. The receiver is NOT always ready or the receiver has limited buffer space.
![[image-364.png]]
![[image-365.png]]

![[image-366.png]]
![[image-367.png]]
* flow control: stop-and-wait


### Protocal 3
* add "The communication channel is NOT free of errors."
* Protocols in which the sender waits for a positive acknowledgement before advancing to the next data item are often called PAR (Positive Acknowledgement with Retransmission) or **ARQ** (Automatic Repeat reQuest)
![[image-368.png]]
![[image-369.png]]
![[image-370.png]]
![[image-371.png]]
* ACK 丢包，那么这样的重新传输不需要被上传到 network 层，所以需要 sequence number
![[image-372.png]]

### Protocal 4
![[image-373.png]]
![[image-374.png]]

![[image-375.png]]
![[image-376.png]]

![[image-377.png]]

### Sliding Window
* RTT：往返时延：$T_{prop}/T_{frame}$
* BD: bandwidth delay product: 在传播时延内，信道中“正在传输但尚未到达”的数据量BD（比特）=B×Tprop​ 
![[image-384.png]]
* ***N = 发送窗口大小（window size）**，也就是**允许在未被确认（unACKed）状态下“在路上”的数据帧个数**。
* Window = Set of sequence numbers permitted to send/receive
![[image-378.png]]

* Unprocessed frames: frames that are received but not deliver to the network layer and their acks are not sent yet.
* sender 发送的和准备发送的加起来没有 ack 的最多有一个 boundery，就是 sliding window
* receiver 没有 ack 的和没有处理的也有一个 boundery



## Protocol 5
![[image-379.png]]
* receiver 的 sliding window 是 1
* 一旦出错，整个扔掉，直到错误的重新传输
![[image-380.png]]
* 序号到 5 的话，最多传 5 个包，0 就是新的，5 就是重新传递（需要有序号作为区分）

![[image-381.png]]
**cumulative acknowledgement**
* Sometimes, ACK number means the next expected frame to receive. So if a ACK number _n_ arrives, it means _n_ − 1, _n_ − 2, … are correctly received while _n_ is the next expected frame to receive.













## Example data link protocols
* Packet over SONET
* PPP 对 packet 进行封装
![[image-382.png]]
* A standard protocol called **PPP (Point to** **Point Protocol)** is used to send packets over the links, including the SONET fiber optic links and ADSL links !
LCP 管理链路（建立连接） 
NCP 管理网络层参数。
![[image-383.png]]

![[image-385.png]]
* LCP && NCP
![[image-386.png]]

![[image-387.png]]


### PPP ADSL
![[image-397.png]]

* payload 包含 PPP 信息

