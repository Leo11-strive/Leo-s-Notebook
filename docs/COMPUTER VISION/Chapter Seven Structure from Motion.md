* 得到初始化物体点和相机位姿
![[image-557.png]]

![[image-558.png]]

* 坐标系，先转换为相机坐标，再投影到成像平面上
* 再从成像平面坐标转换为像素平面

* 相机的外参，内参
![[image-559.png]]
> 外参就是位置和朝向


## Coordinate transformation
![[image-560.png]]
* 外参：相机位置和旋转矩阵
![[image-561.png]]
* an affine transformation
![[image-562.png]]
* 透视投影

* 接下来分辨率，每毫米几个像素
![[image-563.png]]
* 再做一个平移
![[image-564.png]]
![[image-565.png]]
* 内参矩阵
* 投影矩阵就是内参外参的乘积
![[image-566.png]]

## Camera Calibration procedure
* 如何求得内参和外参
*  Capture an image of an object with known geometry, e.g. a calibration board
* Identify correspondences between 3D scene points and image points.
![[image-567.png]]
* 已知三维坐标
![[image-568.png]]
![[image-569.png]]
* 有无穷多个解
![[image-570.png]]
![[image-571.png]]
* 接下来进行分解
* 进行 QR 分解
![[image-572.png]]
* 正好对应
![[image-573.png]]

* 大部分是内参已知
* 直接求外参即可
* 我们做的，很多时候是角点检测，这样三位坐标已知
![[image-574.png]]
![[image-575.png]]

## PNP
* 第一种方法
> 假设内参不知道
![[image-576.png]]
* 三位空间六个自由度（在内参已知的情况下）
![[image-577.png]]
* 必然在这三条射线上
![[image-578.png]]
![[image-579.png]]
* 但是这样存在 4 种可能的解
* 至少要 4 对点


![[image-580.png]]

* 用之前的方法做 P 3 P 初始化

* 三维的点和二维的点如何对应？
> SFM，得到对应之后，再计算相机外参

## SFM
* 已知内参
* 求三维点，相机外参
![[image-581.png]]
* 假设第一张图片的相机坐标为世界坐标
* 求解第二张图像
* Epipolar Geometry
![[image-582.png]]
* 因为相机的中心动了，我发用户 homography 来描述
* 因此我们使用 epipolar geometry
  * `el`, `er`: 假设互相看的到对方，那么这些就是相机
![[image-583.png]]
![[image-584.png]]

![[image-585.png]]

![[image-586.png]]
* E 本质矩阵
* 再做奇异值分解
![[image-587.png]]
![[image-588.png]]
* xl 和 xr 我不知道
![[image-589.png]]
* 用投影矩阵
![[image-590.png]]
![[image-591.png]]

![[image-592.png]]

![[image-593.png]]

![[image-594.png]]
* 做三角化
![[image-595.png]]
* 再做优化
![[image-685.png]]

* 投影与 gt 的距离
* 多张图，两两算，算好之后加入第三张，这样误差累计，如何处理？
> 首先使用 PnP 解决问题，在已经重建好的点上计算相机外参，然后再加入其他点
* 做特征匹配 
* 同时优化每个点的三维位置和相机参数
![[image-686.png]]
* reprojection error
