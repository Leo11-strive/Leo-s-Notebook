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
* 
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
> 经典以太网，Mac 协议，总线
* 交换机的话，没有 collision，不是总线



































































## Collision-Free protocals


