* 画深度的倒数
* 要更加稠密一些
* 相机深度
> 这里指的是离相机的距离，z 轴，离相机平面多远
* 很多领域深度很重要
* 可以通过雷达，波形测算深度
* 立体视觉的体现，两只眼睛
![[image-687.png]]

![[image-688.png]]
![[image-689.png]]
* 被动的感知，SFM

* 人通过运动，双目来感知深度，即立体视觉

* 我们要算所有像素
> 光流
![[image-690.png]]
* 只要匹配这条线即可
![[image-691.png]]
* 任意一个点，在另一个角度中是一条线
* 如果两个相机平面是平行的，一定是水平的
* 视差
![[image-692.png]]

![[image-693.png]]
![[image-694.png]]
* 不满足平行条件，则可以通过图像矫正来实现平行，image Rectification
![[image-695.png]]
![[image-696.png]]
![[image-697.png]]
* 比较两个窗口，看两者的差别，差别最小的地方就是最终的地方
* window size：太小，噪声影响大，因为可能一个窗口就是一个噪点，错误匹配；太大，会过于光滑 (因为匹配时，多个点可能匹配到一起，深度变化缓慢)
* graph cut: 匹配关系存在连续性
![[image-708.png]]
* 相邻像素尽量相似（disparity）
* 两个 loss 和在一起，降低噪声
## Baseline
![[image-698.png]]
* 足够准，越大越好
* 相机参数误差，相机分辨率不够
* 视差太大了，匹配不上
* 把匹配变得更加简单：结构光相机
* 一个相机，如何获取深度信息？
![[image-709.png]]
* 有投影仪就相当于已经有了一个相机了

* 深度相机一般是红外的
![[image-710.png]]

* ToF: Lidar 传感器，传播时间算深度













## Multi-view stereo
* MVS
* 选定参考图像（即这个图片中所有像素的深度）
* 然后构建一个 loss，可以对每一个都算一遍
![[image-699.png]]

![[image-700.png]]


* PatchMatch
* 相邻两个点，offset 尽量相似
* 先初始化，随机
* propagation：传播，offset 传播给其他的地方，如果让对方变好了，就替换
* 替换之后再做局部搜索，对的 offset 传播给周围像素



## 3D surface reconstruction
![[image-701.png]]
* Convert depth map to point cloud
* Compute normal vector for each point
* 计算法向，做 PCA
![[image-702.png]]

* poisson 重建
![[image-703.png]]

![[image-704.png]]
* 表面法向就是图片的颜色梯度方向
![[image-705.png]]
![[image-706.png]]

![[image-707.png]]
