## Feature mapping
* SFM
* feature mapping
* Haris Corner descripter
* 匹配：nearest neighbour
![[image-932.png]]
* 只有局部几何特征，纹理较弱的时候也会相对比较差
* 尤其是 sift，viewpoint change 无法识别
* localization 本身的问题
* 对 motion blur 非常敏感（模糊）

## SuperPoint
![[image-933.png]]
* 最早用卷机神经网络做匹配
* 使用全卷机网络，输出特征点和特征描述字，类似 U net 的结构（interset point 高亮）
![[image-934.png]]
* heat map
* non maximum suppression
![[image-935.png]]

* we need local maxima
* loss function:
> 标识脚点
![[image-936.png]]
* 热力图，L 2 loss（高标点以小高斯作为点）
* 再加上经过 transformation 之后输出还是一致的
* 做一致

![[image-937.png]]

* descriptor 无法直接做约束
* 对应的点的 descriptor 一致，而不对应则不一致
![[image-938.png]]
* loss 的距离超过 M，那么就不参与计算了(设置为定值)，contrastive loss
* triplet loss：训练快一些，L 2
> 三元组 loss
![[image-939.png]]
* 距离的差别同样也需要有一个差别阈值
* where is the training data from?
> 可以手标
* 可以合成
> 图片，之后 warp，只有二维的变换
* 三维重建
> SFM 加上 MVS，SFM 使用多张图的匹配，因此更多的 constraint 消化了两张图带来的奇异性，因此本身会相对更加精确
![[image-940.png]]
* superglue
> 构造网络，做图神经网络匹配两对点
* 但是还是三个阶段的方式
* 检测出来的点的可重复性不强（之前的神经网络训练，泛化的问题）
* LoFTR：不需要检测器的方法，只做稠密的匹配
![[image-941.png]]
* 粗糙网格上的匹配
* 然后根据 localization 就近做进一步匹配
* efficient lofter

## Object Pose Estimation
* 估计物体在相机坐标下的位姿
![[image-942.png]]

* 得到三位模型和对应关系
* 第一种可以使用 sfm，得到三维点云
![[image-943.png]]

* 然后，新的图像和原来的重建图像做匹配，那么这个新的图片的 3 d 点和 2 d 点的对应关系就可以知道了，因此就可以使用 PnP

* Direct Pose Regression Methods
![[image-944.png]]

* 直接输出位姿会有问题，不够精确，而且泛化能力不够
* 可以检查有寓意信息的检查

![[image-945.png]]

* 3 D Human Pose Estimation
* Mocap System
> 姿态估计，光学运动捕捉系统，黑衣白球（反射红外光线），系统周围有很多相机，白点反射红外光，被红外详细拍摄到。多视角相机，知道外参，直接 sfm 构建人的位姿即可，要穿紧身衣，问题是需要贴小球
* 可以使用神经网络解决问题
> 本身二维图像的关键点找到了，直接三角化就好了
* 多个人显然会更难一些
* easymocap
* monocular 3 d human pose estimation
> 约束太少
* 形变 mesh
> 人的姿态参数，（减少自由度，形变参数，运动参数，全局参数），至少看起来像一个人

* Dense reconstruction
> 传统重建通过 SFM，MVS，然后柏松重建
> 传统 MVS：cost volume（每个位置，每个深度的误差），我们需要求最小值
* MVSNet
> 输入是 cost volume，输出是 depth map
> 还是使用特征提取网络，cost volume 本身是特征的差别
![[image-946.png]]
> 比较特征图之间的差别
> 到现在为止是挺好的

* mesh 对优化很不友好，而且渲染的过程本身不可微
> Volume 问题，分辨率有限，point cloud 过于粗糙，mesh 不容易被优化
* 可以使用隐式表达
* 三位平面的圆
![[image-947.png]]
* 我们可以采用神经网络
* occupacy; SDF
> 0,1 (内部，外部) 函数值，分辨率无限； SDF: 到表面的距离
> 0 等值面就是位置
* 使用神经网络即可
![[image-948.png]]
* mesh 不好优化
* 分辨率理论无限大，网络的参数量

## NeRF
* 神经辐射场

## NeuS
![[image-949.png]]
* 神经表面表达
* 对表面位置有精确刻画（SDF）
* 无法使用 volume rendering
> 把 SDF 转成 density，问题是很慢，采样过程

## 3DGS

## 2DGS
* 精确的表面表示
* 二维高斯椭球
* 渲染方式一致

## Single image to 3D
* 单目深度估计
* 没有尺度
> 本身具备奇异性，即使已经知道内参
* 训练的时候需要考虑尺度，需要归一化 scale
![[image-950.png]]
* a 是一个可以训练的参数，通过 a 那一项来抵消 scaled loss，共同的 a，都为 0，说明 scale 相同

* Depth anything
* 腾讯浑元
* SAM 3D










