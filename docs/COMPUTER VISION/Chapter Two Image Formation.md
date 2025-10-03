## Camera and lens
> [!question]
> ![[image-8.png]]

* the raze is not one to one
> [!note] Original Solution
>  ![[image-9.png]]

* 进光量，衍射
* Lens: Same projection as pinhole, but gather more light.
* Field of view: sensor/focal length or o
> [!note] Field of View
>  ![[image-11.png]]

* Aperture: 光圈：进光量
> [!note] F Number
>  ![[image-12.png]]

* Lens Defocus: 
> [!note] Lens Defocus
>  ![[image-13.png]]

* Depth of Field: 景深，焦距越大，景深越小, 光圈越大，景深越小，物距越大，景深越大
> [!note] Depth of Field
> ![[image-14.png]]


## Geometric Image Formation
> [!note] Pinhole Camera Model
> ![[image-15.png]]

* Homogeneous coordinates are invariant to scaling
* Perspective Projection
> [!note] Transform Coordinates
>  ![[image-16.png]]

* Visualize Perspective Projection, mirroring the image plane
> [!note] 
>  ![[image-17.png]]

* 成像的点能够确认方向
* 失去深度信息
* 以图像中心为原点
* 相机坐标系

## Vanishing Point
* 透视投影的，平行线交点
![[image-105.png]]
* 一组平行线对应同一个点
![[image-106.png]]


## Vanishing Lines
![[image-107.png]]

* 地平线，海平面

## Perspective distortion
![[image-108.png]]
* 相机平面与建筑不平行
* 轴移镜头
* what does a sphere do? 椭圆
* The distortion is not due to lens flaws

## Radial Distortion
* 直线应该还是直线
* ![[image-109.png]]
* 桶形：广角
* 长焦：枕型畸变


## Orthograhpic projection
* 正交投影




## Photometric image formation

* shutter:
> ![[image-110.png]]
* 曝光时间大概几十毫秒
* 曝光时间过短，感光时间太短，画面整体发暗（曝光不足）
* 曝光时间过长，过曝，相机会动（模糊），可以把光圈调低
* 动态使用长的曝光时间
* The pixel value is equal to the integral of the light intensity within the exposure time
* 拍视频的时候，风扇叶片出问题，rolling shutter effect
> 一条一条的接受信息
* RGB，HSV（color hue， s 饱和度，v 强度）


![[image-111.png]]

* Bayer filter
* ![[image-112.png]]
* 筛选不同颜色的光的波长
* 每个转换器只能有一个滤镜
* 人眼对绿色更敏感
* 如何得到原来的像素大小？差值
* shading：
![[image-113.png]]

* Compute light reflected toward camera at a specific point.
* Material：Bidirectional reflectance distribution function: how light is reflected at a surface
![[image-114.png]]



