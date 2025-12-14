* 物理层负责把比特流转换成电信号，考虑点对点通信
* 数据调制：信号转换，比特流装载到物理信号
* 物理层需要考虑复用（多个用户共同使用）

## Guided Transmission Media
* Persistant storage (储存好人带过去)
> 花费不高，也是可以考虑的，但是大多数情况需要实时信息传递
* Twisted pair
> 双绞线，两根线交织在一块儿，分为 3 UTP 和 5 UTP（更紧一些），降低彼此之间的电磁干扰
> 越紧，速度越快：百兆/千兆/...
> *Duplex*：媒介在任何一个时刻 a->b, b->a（同时 full，不同时 half）
* Coaxial cable（同轴电缆）
> 一般速率达到 5 utp 以上（相比 twisted pair）
* Power lines
> 波
* Fiber optics
> 光纤，全反射，多模，单模
> 为什么单模好？几乎可以近似一条直线
> 使用什么光？看在光纤中的衰减程度，衰减平稳，传播稳定
> 安全性，etc，贵



## Wireless Transmission
* 电磁波谱
* 无线电传输
* 微波传输
* 红外线和毫米波
* 光波传输

*The wider the band, the higher the data rate（相对变化快的信息）*
*most transmission use a narrow part of the frequency*
![[image-80.png]]

* spread frequency over a wide frequency band
* > frequency hop
* 离散扩频
* UWB

### Radio Transmission
* easy to generate radio waves, travel long, 穿透性（WI-FI）
* once long distance, 易受干扰，衰减快
* 频率低时，会沿着地表传播，高频时，直线传播，还会利用电离层反射

### Microwave transmission
* straight lines
* 高塔

### Infrared and millimeter waves
* 接近光的波长，只要有阻碍，很难进来
* 室内通信
* 遥控器
* 安全性高

### Lightwave transmission
* 气流影响

## Communication Satellites
* 地球同步卫星
> 卫星的卫星
* 中轨道卫星
> GPS
* 低轨道卫星
* 几乎没有遮挡
* 微波

## From Waveforms to Bits

* 傅立叶变换
![[image-79.png]]
* 带宽：
> ![[image-81.png]]
* 时域频域转换





## Symbol rate vs Data rate

* a symbol is one of several voltage, frequency, or phase changes
* **data rate=symbol rate * bits per symbol**
* the modulation technique determines the number of bits/symbol

## The Max. Data Rate of a Channel
* ![[image-82.png]]
* V：每个符号可判的**离散电平**
* 要 encode 0，1 信号，n 个信号，必须至少有 $2^n$ 个不一样的 symbol encode
* Shannon：
* ![[image-83.png]]

## Digital Modulation
![[image-84.png]]

* ![[image-85.png]]
* 编码
* 比特流
* NRZ：非归零编码
* NRZI
* Manchester（防止长 1 的信号衰减）
![[image-86.png]]

* modulation 以载频作为基准线
* >用两个 symbol 相当于一个 bit
* 也就是说
![[image-87.png]]
* Manchester 利用率为正常 NRZ 的一半
* 解决常 0 常 1 的问题：
4 bit 的东西用5 bit 来表示
![[image-88.png]]

* 调幅，调频，调相
* ![[image-89.png]]
* 星座图
> 每个点的坐标就是发射信号在 I-Q 分解下的幅度。

> 振幅由到原点的距离决定，相位通过夹角

> 存在上界，因为信道的 noisy 属性，信号失真（symbol 过于密集）


## Multiplexing
* 如何复用？
> 频分复用（分频带），但是需要 guard band 夹在中间，有频宽的浪费
* 时分复用
> 分时间轴
* OFDM
> 相邻子载波，正交
* Code Division
> 利用编码，码片两两之间内积为 0
![[image-124.png]]
![[image-125.png]]
* 码片相加，因为正交，所以可解

* WDM（波分，波长分，可以说是频分的一种）


## PSTN: 公用交换电话网络
![[image-234.png]]
* structure of the telephone system
![[image-126.png]]
* the local loop: modems, ADSL, and Fiber
> Modems: 调制解调器（电脑数字信号与模拟信号的转换）
> Codec：模拟信号与 PCM 脉冲数字信号的转换
* trunks and multiplexing
* switching
* connected to a local network, and connected to the outer net
* the biggest drawback: low bandwidth
![[image-127.png]]
![[image-220.png]]
* local can be seen as the LAN
* trunks: 核心主干网络
![[image-128.png]]
* fully connect: cost too much
* centralized switch: once the middle broken
* more level hierarchy
![[image-129.png]]
* connection oriented
![[image-130.png]]
![[image-131.png]]
* ADSL: 非对称用户线路
> Downstream is wider than upstream
![[image-132.png]]
![[image-133.png]]
* FttH: fiber
* codec: decode, high sampling, good modulation
* > PCM: Pulse Code Modulation: use the pulse in the signal (信号中的闪现)
![[image-134.png]]
* T 1 carrier: 非常高的数据率, 多层复用（时分复用，TDM）
* SONET/SDH：同步光纤网络
![[image-135.png]]
* 互联网的 switching，
> 分组交换将用户数据**分割成小块（称为“分组”或“包”）**，每个分组独立寻址、独立路由、独立转发，到达目的地后再重组还原成原始数据。

![[image-136.png]]

![[image-235.png]]
## Cellular Networks
* 蜂窝网络：中心相当于基站

## Cable Networks
* 混合

## Policy: Spectrum Allocation
* free frequencies
![[image-137.png]]

* in America, the protocal and policy is different, or diverse



