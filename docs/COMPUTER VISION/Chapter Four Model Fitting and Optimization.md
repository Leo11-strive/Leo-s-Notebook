
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


## Outliers
* 因为存在一个平方，因此，残差越大，影响越大，梯度越大，模型被带偏
* 可以使用 $L 1$ 范数距离, error 更加线形（第一层导数），会相较二范数，outlier 影响更小，但是存在不可导的问题。但是现在 pytorch 的封装帮你解决了这个问题
* $L 1$, $L 2$ loss 的混合，Huber Loss

### RANSAC
* Random Sample Concensus
> 拟合一条直线，因此只需要两个点，随机找两个点
> The distribution of inliers is similar while outliers differ a lot
* Use data point pairs to vote，设置预值，分布差不多的点会集中在某条正确的直线上，outlier 和正确的分布存在不同
![[image-335.png]]
* 用给它投票的点再进行拟合，这个时候 outlier 就少了
* 采样，拟合，求解都没法计算（维度过高，参数量较高时）


## Overfit
* 这里不再赘述

## Ill-posed Problem

* 变量的个数大于采样点的数量，一定有解
![[image-336.png]]
* use prior knowledge to add more constraints（添加先验概率作为约束）
![[image-337.png]]
* $L2$ 正则化
* $L1$ 正则化
> 参数为 0 的更多，让求解的参数更加稀疏