* How to combine two images?
![[image-463.png]]
* 图像中每一个点的变化都可以用同一个等式表示
![[image-464.png]]
* 我们需要把矩阵转回去
![[image-465.png]]
* 相机中心不变，传入的光线不变，坐标就是单一变换，一一对应
* 中点移动，但是 scene 是一个平面
* 投影变化，8 个自由度
![[image-466.png]]
![[image-467.png]]
![[image-468.png]]
* 可以做反向变换回去已知的图像
![[image-469.png]]
* 解决 pixels 在 pixel 中间的问题

## Image Stitching
![[image-470.png]]
 * 线形方程组
![[image-471.png]]
![[image-472.png]]
![[image-473.png]]
![[image-474.png]]
![[image-475.png]]
* 对 h 加一些约束（长度等于一）
![[image-476.png]]

* 存在 outlier（不满足我们之前的假设）
![[image-477.png]]
* 使用 ransac
* 使用比较少的点匹配

![[image-478.png]]
* 取票数最高的

![[image-479.png]]
![[image-480.png]]
* 但是这样的话，越往边上，变换越严重
![[image-481.png]]
![[image-482.png]]
* 投影到柱面坐标，先投影，再匹配
* 柱面坐标的转换就是简单的平移关系，但是可能会有飘移（误差等导致）
![[image-483.png]]
* 可以把最后一张和第一张拼起来，高度变化最终要有限制