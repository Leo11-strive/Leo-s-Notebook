* 语意分割
> 逐像素分类
* Label each pixel in the image with a category label。 Do not differentiate instances
* object detection
* instance segmentation
> 实例分割
* 像素类别是一样的，但是单纯分割没法把每一只猫分开

## Segmentation
* 最简单的做法：
![[image-823.png]]
* 问题是太慢了
* 如果每一个图像块单独过网络，其实就是黑乎乎的一片（很有可能），因此它的 receptive field（感受野） 就太小了
* 而且物体边缘的分类会很差
* 是否存在更好的做法？
* 使用全卷积网络
![[image-824.png]]
* 每个像素都是一个概率分布
* loss function 如何定义？
> Cross entropy per pixel
* receptive field 取决于网络
* 去做 pooling 和 unpooling/strided convolution
* unpooling 可以使用差值先增大，然后再过卷机，来学真正的 unpooling
* Transposed convolution
![[image-825.png]]


* pooling 的过程总是丢失信息的，尽量还原，但是重投影因为奇艺性还是会有损失
* 如何把原来丢掉的信息加回来？
* 类似 residual，可以把之前的信息加过来
* concatination
* skip connnection
![[image-826.png]]
* dense prediction: 每个像素都要有一个输出的情况

### DeepLab
* 膨胀卷机：
> Atrous convolution: 做内积的时候，隔着一个点乘一下，感受野更大（同样大小的卷机核）
* 很多时候使用 transformer：有一个全局的感受野
* 最后的输出层：CRF：保证输出的 label，相邻的像素输出的结果接近

* segment anything：image+text

* 如何衡量是否正确呢？
> IOU，重合程度
![[image-827.png]]

## Object detection
* 输出一堆 bounding box
* object detection
* detect：网络输出类别，bounding box 的类别
![[image-828.png]]
* 输出大小不确定
* sliding window
* 存在严重问题：太慢了
* region proposal
* Find a small set of boxes that are likely to cover all objects 
![[image-829.png]]
* R-CNN
* Run region proposal method to compute ~2000 region proposals
* Resize each region to 224x224 and run through CNN
* Resize each region to 224x224 and run through CNN
* Predict class scores and bbox transform
* Use scores to select a subset of region proposals to output
* 同时使用 IOU 来判定和控制 prediction 和 gt 的重合度
![[image-830.png]]
* 不断选择

* 还是速度慢

* Fast R-CNN
![[image-831.png]]
* 大的卷机得到特征，然后使用 proposal 过一遍卷机，用更小的网络，降低使用时间（就是先用大的提取所有的特征，然后 regions of interest 再在已经变成 local 特征的地方再做卷机）

* Faster R-CNN
> 减少 region proposal：
* 提高 proposal 的准确程度
* RPN：用来训练得到 proposal
![[image-832.png]]
* 第一步，先预测这些框是否有东西，然后再输出一个微小的调整 bias
* 做一个阈值
![[image-833.png]]
* single stage object detector
* 单阶段法
![[image-834.png]]
* 再加一个 classification 就可以了
* YOLO


## Instance segmentation
![[image-835.png]]
* 先做目标检测，然后再分割前景和背景
* 1，0 分别（mask）
* 全景分割
![[image-836.png]]

* Human Pose
* 人的姿态
* 简单骨架
* 深度图
![[image-837.png]]
* 检测关键点会更简单
> 使用 random forests
* single human
![[image-838.png]]
* 直接求坐标
* 从图像到坐标，这个映射比较难
![[image-839.png]]
* 使用 U net
* output heatmap

* Multiple humans
* 把检测出来的关键点连接成一个人
![[image-840.png]]
* 两个人可能会有重合，所以 top down 也会出现问题
* bottom-up
> Detect keypoints and group keypoints to form
* OpenPose
> Part affinity field, 向量场，人的连接方向
* 一般来说，top down 对单人来说比较准确，但是 bottom up 的方法够快
* optical flow：
> LK 光流是传统方法
![[image-841.png]]

* 给 cost volume：任意两点的差别，给网络引导

* Estimate flow iteratively from a coarse-to-fine cost volume
* video classification
* 3 d cov
* Given a long untrimmed video sequence, identify frames corresponding to different actions
![[image-842.png]]
* 目标检测并且跟踪




