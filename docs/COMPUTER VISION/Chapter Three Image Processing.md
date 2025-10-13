* Image processing basics
* Image sampling
* Image magnification

## Contrast
![[image-115.png]]
* 亮的调亮，暗的调暗


## Filter
* edge
* blur

* 在频域的处理
![[image-116.png]]

* 离散的卷积
* Padding
* ![[image-117.png]]
* ![[image-118.png]]
* 这个滤波器是 I-blur（I）+I（double 高频信息）

![[image-119.png]]
* edge
* 长成什么样子的卷积核，图像中类似特征的地方会有大的响应
* Bilateral filter
![[image-120.png]]
* 滤波器随图像内容变化而变化



## Image Sampling

* image resizing
* image sampling
* > 走样
* Aliasing:
![[image-121.png]]


### Undersampling
* 采样的频率远远不如数据原始的频率
![[image-186.png]]
* 傅立叶变换
* 信号的频谱：
![[image-187.png]]
* 傅立叶变换
> 可以理解为能量的转换？
![[image-188.png]]
![[image-189.png]] 
* 特别函数的变换
![[image-190.png]]
* 时域的卷积就是频域的乘积
* 二维频域：表示不同方向的频率
![[image-191.png]]
* 强化了低频信息
* 低通：
![[image-192.png]]
* 窗口越大，更低频
![[image-193.png]]
* 采样函数，间隔多少有一个数值，就是源函数乘上一个离散的函数
![[image-194.png]]
* 买从函数
* 对这个采样函数做傅立叶变换
* 就是频域的卷积
![[image-195.png]]
* 不能发生混合
![[image-196.png]]
* 大于原来信号的两倍 f 0
![[image-197.png]]

### How can we reduce aliasing?
* increasing sampling rate
* 去掉原来信号的高频部分
![[image-198.png]]
* 降采样
![[image-199.png]]

## Image magnification
* image interpolation
> Nearest neighbour, not continuous, not smooth

* Cubic interpolation
> 连接点数值和一阶导相同
* 二维线性差值
![[image-200.png]]
* 二维 Cubic 差值
* Super Resolution
> Prediction , deep learning

### Change the aspect radio
* Changing aspect ratio causes distortion, thus cropping may remove important contents
![[image-201.png]]
* edge is important
![[image-202.png]]
![[image-203.png]]
* 梯度小，从上往下连续
> Intuition: 最短路径算法, using D P

