* 上一堂课程
![[image-292.png]]
* 引入 $q_i(z)$ 近似
![[image-293.png]]
* 作为一个 lower bound


## Entropy
![[image-294.png]]
* 对于 $q_i(z)$ 而言，this loss make it imitate $p(z|x)$

## KL-Divergence
![[image-295.png]]

![[image-296.png]]

* 对于 q (x) 来说
![[image-297.png]]
* 减少当前分布的 KL 散度

* 对于这两个来说，增大 log likelihood都是正确的方向

* q(z)不断追赶越来越正确的 P（z｜x）

![[image-298.png]]


* 通过单独的一次 sample 作为期望的近似值，然后对这个近似值进行优化
* 问题就是，如果按照上述方法，参数过多，不如 $q_i(z)$ 直接神经网络表示

## Amortized Variational Inference
* 接下来的问题是，如何计算那个我们之前假设的 $q_i(z)$
![[image-299.png]]
* 但是，policy gradient 的问题是，支持离散的 z 不错，但是连续的情况需要每次进行大量采样，而且 high variance, 不稳定，追逐一变化的目标
* 那么，我们给连续的 z 设置一个分布集
![[image-300.png]]
* 还是，想办法用采样来近似期望

* 我们进一步思考

![[image-301.png]]
* 做一次 sample
![[image-302.png]]

## Variational Autoencoders
![[image-303.png]]
* 生成的时候直接 sample 就好
* 这里的 KL 散度就是强迫两个分布的相近
* 好的，接下来可以尝试使用全卷积，但是
![[image-304.png]]
![[image-305.png]]

* 全卷积意味着维度之间必然存在相关性（相同的，共用的 weight），其中一个变化，另一个也会有变化
* 而全连接层，因为每一层的权重独立，所以，intuition 可以是正交投影


## VAE in Practise
![[image-306.png]]
* latent code ignored, 训练的时候学到了 average
![[image-307.png]]

* latent code not compressed
![[image-308.png]]



## Simpler Model

![[image-309.png]]

![[image-310.png]]
![[image-311.png]]

## NICE Nonlinear Independent Components Estimation
![[image-313.png]]
* recover, 就是从 x 会推到 z，gradient 会使用到这个部分
![[image-314.png]]

* 提高 NICE 的表达能力（观察这个分布转换矩阵，在给定 z 的分布下，表达能力差，从 intuition 中也能感觉出来）


## Real-NVP
![[image-315.png]]
![[image-316.png]]
