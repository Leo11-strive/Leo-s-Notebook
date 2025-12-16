![[image-972.png]]

![[image-973.png]]
* 实体：传输实现
![[image-974.png]]
* 传输层的代码是在 host 上运行的
* network 大多在路由器中实现

![[image-975.png]]

![[image-976.png]]
* 提供了 5 个源语
* listen，connect，receive，disconnect

## Segment
![[image-977.png]]
* 使用有限状态机
![[image-978.png]]

![[image-979.png]]

![[image-980.png]]
* 比较传输层和数字链路层
> 可靠数据传输
![[image-981.png]]

![[image-982.png]]
* Transport service access point（TSAP）
![[image-983.png]]
* 有些端口约定俗成
![[image-984.png]]
* 用到之后再 fork 一个
![[image-985.png]]
* 发了两次

* solution 1: 加入编号即可

![[image-986.png]]
* 三个比特不够用了
![[image-987.png]]

![[image-988.png]]

![[image-989.png]]

![[image-990.png]]
* 设置一个阈值

![[image-991.png]]

![[image-992.png]]

![[image-993.png]]

* 接下来如何建立连接，是否是 duplicate？
* three way handshake
> CR，ACK，OATA
![[image-994.png]]
* 异常情况
> Duplicated，x 太早之前的就不建立连接了，利用三次握手的 ack 一致性，可以完成 reject

![[image-995.png]]

![[image-996.png]]

![[image-997.png]]
* 同步释放还是异步释放

![[image-998.png]]

![[image-999.png]]
* 决定什么时候停下是个问题

![[image-1000.png]]

![[image-1001.png]]

![[image-1002.png]]

![[image-1003.png]]

![[image-1004.png]]

![[image-1005.png]]

* 链路层的 check 不能说所有的链路正确就一定正确，但是中间的主机可能会出现 crash
* sliding window
> 一般链路层只需要把 sliding window 设置为 1
> 传输层的 sliding window 会控制
* 缓存机制
![[image-1027.png]]
* 发送端缓存，receiver 缓存，可以处理 out of order 数据包
![[image-1028.png]]
* chained fixed size 会有内部碎片没有充分利用的问题
![[image-1029.png]]

![[image-1030.png]]
* 通过 receiver 的数值做匹配
* 通信子网的传输数据的能力
![[image-1031.png]]

![[image-1032.png]]

## Multiplexing
* 多路复用
* demultiplexing
> 解码复用
![[image-1033.png]]

## Crash Recovery
* Host Crash
![[image-1034.png]]
* stop and wait protocal
> S 1: 现在所有的信息都已经收到 ack 了
> S 0: 存在没有收到的 ack
![[image-1035.png]]
* 出现 duplicated 的问题可能不在缓存里面了，所以会存在问题
> 因为 crash 了，没有缓存
* 第一种情况，都重新传输或者都不传输，要么分情况
![[image-1036.png]]
* 仅仅在传输层是很困难的

## Congestion Control
* 臃塞控制
![[image-1037.png]]

![[image-1038.png]]

![[image-1039.png]]

* 有线环境下，丢包很多，很大概率是拥塞了
![[image-1040.png]]

![[image-1041.png]]

![[image-1042.png]]

![[image-1043.png]]
* 如何分配带宽
> Additive increase and decrease: 共同增加，共同减少
> Multiplicative increase：等比增加
* 增加的时候是 additive，减少的时候是等比减少
![[image-1044.png]]

![[image-1045.png]]

![[image-1046.png]]
![[image-1047.png]]

![[image-1048.png]]

## The Internet Transport Protocols: UDP, RPC, RTP
![[image-1049.png]]

![[image-1050.png]]

* 8 个字节的包头

![[image-1051.png]]
* 函数在另一个主机上执行
* 对于编程开发者而言是抽象掉的

![[image-1052.png]]

* RTP
> Real time transport
> 用户态协议
> 在 udp 基础上的协议
![[image-1053.png]]

![[image-1054.png]]
* RTP 协议是一个实现在用户态的传输层协议，是把很多应用 mutliplex 到一个 single stream
![[image-1055.png]]

* 带时间戳
![[image-1056.png]]
* RTCP
![[image-1057.png]]
> 控制平面的事务

* timestap：减少抖动
![[image-1058.png]]
* 先保存
![[image-1059.png]]
* 看当前的 glitter，就是 delay 变化很大的问题

## TCP 协议
![[image-1060.png]]
* 可靠，端到端，socket to socket
![[image-1061.png|630x359]]
* 一个进程监听所有的端口号，然后请求来了再 fork

* 点到点
* byte stream：没有消息边界
![[image-1062.png]]

![[image-1063.png]]

![[image-1064.png]]

* 20 个字节
* TCP header length：32 位的 words
![[image-1065.png]]
* ack piggy back
* CWR: sender 已经开始做拥塞控制了
* ECE：发生拥塞之后的，ECN 发过来
* URG：urgent data
* psh：立刻发送
* rst：重制连接
* syn：connection request
* fin：释放
* window size：窗口的大小

![[image-1066.png|630x427]]
* 连接的建立
* 三次握手。Ack 是下一个期待的信息的 seq
![[image-1067.png]]

* 连接是双向的，每个方向的连接可以单独释放
* fin 被 ack 之后就会被释放
![[image-1068.png]]

![[image-1069.png]]

![[image-1070.png]]

![[image-1071.png]]

![[image-1072.png]]

![[image-1073.png]]

![[image-1074.png]]

![[image-1075.png]]
* smooth rtt：上一个 srtt 再加上当前的 rtt
* rttvar：看看这个方差是否大
> 也是迭代 R：当前的 rtt
* RTO：就是 retransmission timeout
![[image-1076.png]]
* persistence timer：主动发送询问包的 buffer 剩下多少，防止 deadlock
* keepalive：是否存活

![[image-1139.png]]

![[image-1140.png]]

* AIMD = Additive Increase, Multiplicative Decrease（加性增大、乘性减小）


