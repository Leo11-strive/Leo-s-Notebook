![[image-317.png]]
* use a network to distinguish the distribution one generation belongs
![[image-318.png]]
* 降低这个 loss
![[image-319.png]]

![[image-320.png]]

* "Just backpropagate from the discriminator into the generator!"
![[image-321.png]]
![[image-322.png]]


![[image-323.png]]

* 快速 converge

![[image-324.png]]

* may need structural constraint (my opinion)

![[image-325.png]]

* GAN 需要有一个更加平滑的 gradient
![[image-326.png]]
![[image-327.png]]

* JS Divergence does not account for distance

## WGAN
![[image-328.png]]
![[image-329.png]]
* 计算总距离
![[image-330.png]]
* 选择特定的能达到上确界的 f
![[image-331.png]]

![[image-332.png]]

加一个 regularizor
![[image-333.png]]

![[image-334.png]]
