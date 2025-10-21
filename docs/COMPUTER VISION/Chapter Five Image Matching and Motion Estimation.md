
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