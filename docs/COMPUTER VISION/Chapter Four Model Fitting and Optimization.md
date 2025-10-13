
## Optimization
![[image-204.png]]

* examples in conputer vision
![[image-205.png]]

## Model Fitting
![[image-206.png]]

* mean square error
* MSE: maximum likelihood


## Numerical Methods
![[image-207.png]]
* 求导即可，有解析解

* 无解析解，步步达到目的
![[image-208.png]]

![[image-209.png]]

* Taylor expansion
![[image-210.png]]
* 与梯度同方向相反
> 最速梯度下降法

### How to determine the step size?
![[image-211.png]]
![[image-212.png]]
* 希望 alpha 落在绿色区间
* 越到后面，用一个平面取近似越不正确

![[image-213.png]]
* 找到最合适的 $\delta x$
* 问题就是二阶导，如何近似呢？

![[image-214.png]]

* 只对里面做
![[image-215.png]]

![[image-216.png]]

* 但问题是如果奇艺的话，方程组会有无穷多个解
![[image-217.png]]

![[image-218.png]]

![[image-219.png]]

