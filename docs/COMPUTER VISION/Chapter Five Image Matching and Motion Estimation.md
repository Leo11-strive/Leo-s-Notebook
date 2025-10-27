
* Image matching
> Detection, Description, Learning based matching
![[image-338.png]]

## Detection

* 特征点检测
![[image-339.png]]
* 匹配角点，因为角点匹配
> 寻找最特殊的点
* how to define ubniqueness
![[image-340.png]]
![[image-341.png]]
* 通过协方差矩阵
* 用 PCA 找到最大方差
* 找到主方向即可
![[image-342.png]]
![[image-343.png]]
* 更简单一些：
> 角点检测器：
![[image-344.png]]

* Find local maxima of response function (nonmaximum suppression)

* be invariant to image transformations

![[image-345.png]]
* 对于图像亮度变化敏感吗？
> Partially invariant：因为差距会变大，超过 threshold
* 平移不敏感
* Second moment ellipse rotates but its shape (i.e.eigenvalues) remains the same
* scaling：会被窗口大小影响，这是一个很大的问题

## Automatic Scale Selection

* 使用多个不同窗口的大小，找到 f 值最大的点
![[image-346.png]]
* 角点对应的尺度
> 不同所放的图片，这个尺度是一致的
![[image-347.png]]
* 每个点四个 f 值



## Blob Detector
![[image-348.png]]
![[image-349.png]]
* 拉普拉斯，二阶导（blob，导数的快速变化）
* LOG：
![[image-351.png]]
![[image-350.png]]
* 先对高斯核本身做拉普拉斯
![[image-352.png]]
## DoG
![[image-353.png]]

![[image-354.png]]

## Detection
![[image-355.png]]
![[image-356.png]] 
* 方向做归一化

## Feature Matching
* 接下来做匹配（SIFT descriptor）
* 比较每一对点的描述子
![[image-445.png]]
* 问题是重复的描述子，比如：
![[image-446.png]]
* 如何消除奇异性的匹配
### Ratio test
![[image-447.png]]
* 如果这个 ratio 几乎等于 1，那么具有奇艺性，匹配不可靠，起到一个过滤的作用

![[image-448.png]]
* 相互最近邻的方法

## Motion Estimation
![[image-449.png]]
* 给我一个视频，解决这个视频如何动的
* 要找到视频中第一帧的点在第二帧在哪里
* 光流，每个像素的位置变化
* 都使用 LK 算法
* What is the difference to feature matching?
![[image-450.png]]
![[image-451.png]]
* 对所有 x，y 成立，要求解的是 u 和 v
![[image-452.png]]
* 就是关于 u，v 的线形方程，但是目前只有一个方程
* 再加上临近的点运动相似
![[image-453.png]]
* 接下来，使用 least square solution
![[image-454.png]]
* 直接解解析解即可
![[image-455.png]]
* 当纹理近似的时候，有无数组解
![[image-456.png]]
![[image-457.png]]
![[image-458.png]]

## Errors
 ![[image-459.png]]
 
 
* 还会有遮挡
* 第一个问题，光线变化
> 可以使用梯度
* 第二个问题，motion
![[image-460.png]]
* 所以可以先模糊一下，这样平滑，像素位移变小（以像素为距离单位）

![[image-461.png]]
* 使用 mipmap 形式的金子塔
* 基于上层的结果，移动第二层的像素，使得他们更加接近，然后可以把向量加起来

（向量，位移能力的表征）


* 可以把向量可视化
![[image-462.png]]
* 可以用作 video stabilization
* denoising

