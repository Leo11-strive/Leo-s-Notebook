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
![[image-146.png]]
* 奇偶校验：一位出错

### How to detect and correct errors?
* 发送的时候要有冗余的
* 接收方分开
* 再进行检测
* 汉明距离，如果出错的位数小于，那么能够 detect
* 改错：错误位数小于最小汉明距离的一半
