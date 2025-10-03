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
