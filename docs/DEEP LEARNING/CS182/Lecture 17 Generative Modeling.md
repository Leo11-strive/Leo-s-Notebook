## The types of autoencoders
* 强制型的对生成的 representation 做一个输入不一样的变化
![[image-283.png]]
* layer wise pretraining
> 在算力不足的年代，会通过分层，然后把第一层的 encoder 给第二层做重建，再做了一次信息压缩，然后继续
![[image-284.png]]

## Latent Variable Models
* to simply the distribution, we add another variable to create a convolution-like structure
![[image-285.png]]
* in general
![[image-286.png]]
* 接下来，我们需要利用 MLE，但是做出改变
* 首先需要把积分离散化，变成期望
> [!note] Proof
> ![[image-287.png]]
> ![[image-288.png]]
> 去掉与 $\theta$ 无关项即可
* probabilistic inference
> [!note] Intuition
> A latent variable deep generative model is (usually) just a model that turns random numbers into valid samples (e.g., images)
![[image-289.png]]

* representation
![[image-290.png]]

* Other choices (not covered in this lecture): discretized logistic, binary cross-entropy, especially common for best performing models
* can use transpose convolutions for $p_\theta (x|z)$
![[image-291.png]]

