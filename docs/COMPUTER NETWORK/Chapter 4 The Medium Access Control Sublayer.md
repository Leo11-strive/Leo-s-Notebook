![[image-484.png]]
* In any broadcast network, the key issue is how to determine who gets to use the channel when there is competition for it.
* Broadcast channels are sometimes referred to as multiaccess channels or random access channels. 
![[image-485.png]]
![[image-486.png]]
* static
![[image-487.png]]
* 不够动态
![[image-488.png]]
* **一个数据帧（frame）从到达发送队列到成功被发送完毕所经历的**平均时间延迟。
* 它是一个帧的**总平均等待时间 + 平均服务时间**。

![[image-489.png]]
* mean time delay 变成 N 倍

## Dynamic Allocation
![[image-490.png]]


## ALOHA
* Let users transmit whenever they have data to be sent.
> There will be collisions and the colliding frames will be destroyed. 中间可能有 ack
> The sender just waits a random amount of time and sends it again if a frame is destroyed.
* Pure ALOHA：frames are transmitted at completely arbitrary times.
* SLotted：
![[image-491.png]]

![[image-492.png]]
![[image-493.png]]

![[image-508.png]]



![[image-509.png]]    
![[image-494.png]]

* Quiz, 待完成


## CSMA
![[image-495.png]]
![[image-496.png]]
![[image-497.png]]
![[image-498.png]]
![[image-499.png]]

### Non-persistant
* 加个随机数
![[image-500.png]]

* P-persistant
![[image-501.png]]
![[image-502.png]]
## CSMA with CD
* 是一种降低 CD 的方式
* 但是，因为它需要同时发送和收听，因此要能够分辨两者，需要两者强度近似
* 但是显然无线信号很难做到这一点
![[image-510.png]]

![[image-503.png]]
* As soon as stations detect a collision, they abruptly stop their transmissions (rather than finishing them)
* A received signal must not be tiny compared to the transmitted signal (which is difficult for wireless)

![[image-504.png]]
![[image-505.png]]

## Collision Free
![[image-506.png]]
![[image-507.png]]
### Bit-Map protocol
![[image-511.png]]
![[image-512.png]]


## Token passing

![[image-529.png]]
![[image-530.png]]


## Binary countdown
![[image-531.png]]

* 地址作为排序的方式
* 每一位的权重依次降低
* OR，就是看每一位是否存在 1，只有在当前位有 1 的，即更高优先级的留下
![[image-532.png]]

## Limited-Contention protocals
![[image-533.png]]

### Adaptive Tree Walk Protocol
![[image-534.png]]
![[image-535.png]]

## Wireless LAN protocals
![[image-536.png]]
* MACA: Multiple Access with Collision Avoidance
![[image-537.png]]
* 图 a，a 想要发数据给 b
![[image-538.png]]
* C 只听到 RTS 却没有听到 CTS，所以这种情况 indicate D 不在范围内，或者 RTS 被拒绝，所以可以发送


## Ethernet
* classic ethernet
> 经典以太网，Mac 协议，总线（基于总线，会 broadcast）
![[image-596.png]]
* 这个简写，代表“10”速率，10 Gbps
* 交换机的话，没有 collision，不是总线
* 调制解调，数模转换
![[image-608.png]]
* With –1 volts for a 0 bit, with 1 volts for a 1 bit A receiver may sample the signal at a slightly different frequency than the sender used to generate it can be out of synchronization.

* sender 和 receiver 的时钟不同
![[image-597.png]]
* 可以使用曼彻斯特编码
![[image-598.png]]

* mac sublayer protocal
* type 步骤：
> 可以区分 length 或者 type
* preamble: 用来区分是开头还是结束
* 二进制指数退避算法
![[image-645.png]]
* 时间离散时间槽
* 第一次发生 collision
> 退避 0，或者 1
* 第二次碰撞
> 退避更多时间，指数级增长
* collision 时间过长，多次碰撞之后，failure

![[image-599.png]]
å
* Preamble: 8 bytes, containing the bit pattern 10101010, used for synchronization
![[image-600.png]]

![[image-601.png]]
* 数据帧发送完之前，一定能判定
* The **Binary Exponential Backoff Algorithm**
![[image-602.png]]
* frame 长度增加，efficiency 增加，平均每个 bit 的 overhead 以及等待时间都变少
* 同时 station 越多越拥挤，collision 越多
![[image-603.png]]
* 交换机
> Switched ethernet
![[image-646.png]]
* 集线器；交换机：上下不在一个冲突域
![[image-647.png]]
* 核心就是存在高速背板，连接所有端口，向指定端口传递数据帧，不需要 mac 协议
* 都是可以同时发，只是如果同时传输给一个端口，需要有缓存
> outputs frames to the ports for which those frames are destined. None of the other ports even knows the frame exists.
![[image-648.png]]
* ethernet 的速度
* ethernet 的数字命名
> Number speed -介质

![[image-604.png]]
* 计算机网络需要商业化，与标准化
![[image-649.png]]
* 子层，在数据链路层的子层

![[image-605.png]]
* 802.11:
> 都有一个无线的基础设施
* 802.11 b:
> 速度慢
* DSSS
> 有点类似 CDMA，每发一个数据，多加入多个 bit，加入编码机制
* 802.11 g;
> 使用不同的频段
* n：
> 速度更快
![[image-606.png|302x192]]
* ready, 占用，随机数，等，先会有一个等待，时钟会出现冻结
* CSMA-CA
> 都有 channel sense 能力，no collision detection
* DCF
> 分布协调模式：是否要发言是自己决定的，AP 不会决定你什么时候发言（802.11）
* PCF
> 所有的统一管理
* 802.11 非常谨慎，听到 rts 也不能发送
![[image-651.png]]
* 无法解决暴露中端问题
* NAV：持续的时间，对预留的预判
* 之前讲的 MACA 协议看起来很好，但是实际使用的时候很少会用
![[image-650.png]]

> 中央管理
![[image-607.png]]

### DCF
![[image-652.png]]
* 分片传输
* 但是分片希望连续
* 定义帧间隔
> SIFS，DIFS
![[image-653.png]]
* 时间越短，抢占越容易

## Frame Structure
![[image-654.png]]

![[image-655.png]]
* 为什么有两个 address？
* duration 就是之前讲的 NAV
* address 1: 接收端
* address 2: 发送端
> 为什么三个地址？：
![[image-656.png]]
* 所以 address 多出一个是为了 AP 这个转发地址
* intracell：在... 里面
* intercell：跨区域 service

## Data Link Layer Switching
* 多个局域网连起来，更大的网桥
![[image-657.png]]
* 能不能把两个不同的 link layer 连接起来？
> 效果不好
* 连接两个同构的局域网
![[image-658.png]]
* b 图中 A，B，C 是传输时互不干扰的
![[image-659.png]]
* 统计意义上的哈希表
![[image-660.png]]



* Learning bridges
> 地下也有路由规则
![[image-661.png]]


* Spanning Tree Bridges
> 加入两个桥，是否合适？
> 形成环路，对数据包转发无力
> 当需要 flooding 时，只关心 F 1，F 2 这两条，同样 B 2 也要 flood，这里就形成环路了，大量带宽资源消耗，所以需要改造拓扑
![[image-662.png]]

![[image-663.png]]

### Virtual LANs
* 很大的局域网
![[image-664.png]]
* 一个很大的 physical LAN，虚拟成多个 virtual LAN
* 个数，属于哪个局域网，需要分块，switch，bridge 的配置
![[image-665.png]]
* 虚拟成两个局域网
* 每个 port 都给一个 color/每个 mac 地址/每个 network 地址给一个 color
* 如何决定属于哪个颜色？
![[image-666.png]]
* IEEE 802.1 Q
> 对以太网帧的改进
![[image-667.png]]
* 加入一个 VLAN protocal
> 重要字段：vlan identifier
![[image-668.png]]
* 如果说网络中的协议不同呢？
> B 6 不是 VLAN aware 的话，需要做 frame 转化
![[image-669.png]]

* 数据帧越到下面越长
* Repeater：中器物：放大一个 signal
![[image-670.png]]

![[image-671.png]]

![[image-672.png]]

![[image-673.png]]
* a: 一个冲突域
* bridge 启用连接作用
* switch 是交换机，更加现代的 bridge
> 端口号更多，更加 general
![[image-674.png]]

![[image-675.png]]
* collosion domain：
> 冲突域：同一时刻一个在发送
* broadcast domain:
> 其他人都能听到，一个发（数据链路层）
![[image-676.png]]

![[image-677.png]]

* bridge is pretty similar to switches
































































