* continuous scene 5 D vector
* whose input is a 3D location x = (x, y, z) and 2D viewing direction $(\theta, \phi)$
* output: emitted color c = (r, g, b) and volume density σ
![[image-2.png]]

* the direction represented as a 3D Cartesian unit vector d (方向向量)
* multi-view consistent by restricting the network to predict the volume density σ as a function of only the location x, while allowing the RGB color c to be predicted as a function of both location and viewing direction.
* the MLP:“first processes the input 3D coordinate x with 8 fully-connected layers (using ReLU activations and 256 channels per layer), and outputs σ and a 256-dimensional feature vector.” (Mildenhall et al., 2020, p. 5)
* “concatenated with the camera ray’s viewing direction and passed to one additional fully-connected layer (using a ReLU activation and 128 channels) that output the view-dependent RGB color.” (Mildenhall et al., 2020, p. 5)
* The volume density σ(x) can be interpreted as the differential probability of a ray terminating at an infinitesimal particle at location x.
![[image-3.png|411x44]]

* estimating this integral C(r) for a camera ray traced through each pixel of the desired virtual camera.
* positional encoding to leverage high frequency information
* deep networks are biased towards learning lower frequency functions
* positional encoding:
![[image-4.png]]
* Volume Sampling: Hierachical 先用 stratified sampling 得到数值，计算权重（第一个 network），然后根据权重新利用 inverse transform sampling 再 sample 数据，使得我们 sample 的数据对生成的结果的 contribute 要多，然后两者结合再渲染（第二个 network）“we use the sampled values as a nonuniform discretization of the whole integration”“This procedure allocates more samples to regions we expect to contain visible content.”
![[image-18.png]]

* “To do this, we first rewrite the alpha composited color from the coarse network Cˆc(r) in Eqn. 3 as a weighted sum of all sampled colors ci along the ray:” (Mildenhall et al., 2020, p. 8)
* Normalizing these weights as wˆi = wi/∑Nc  j=1 wj produces a piecewise-constant PDF along the ray. We sample a second set of Nf locations from this distribution using inverse transform sampling, evaluate our “fine” network at the union of the first and second set of samples, and compute the final rendered color of the ray  Cˆf (r) using Eqn. 3 but using all Nc +Nf samples.
## Implementation
* requires only a dataset of captured RGB images of the scene, the corresponding camera poses and intrinsic parameters, and scene bounds（需要外参，内参，取样范围）
* 合成数据使用渲染引擎精确的参数，而真实数据用 comap 做 sfm 近似
* The loss:
![[image-19.png]]

* “minimize the loss of Cˆc(r) so that the weight distribution from the coarse network can be used to allocate samples in the fine network.” (Mildenhall et al., 2020, p. 9)
* “Adam optimizer [18] with a learning rate that begins at 5 × 10−4 and decays exponentially to 5 × 10−5 over the course of optimization (other Adam hyperparameters are left at default values of β1 = 0.9, β2 = 0.999, and = 10−7)” (Mildenhall et al., 2020, p. 9)
* Volume Bounds:
* > For experiments with synthetic images, we scale the scene so that it lies within a cube of side length 2 centered at the origin, and only query the representation within this bounding volume.
* > reconstruct real scenes with “forward facing” captures in the normalized device coordinate (NDC) space that is commonly used as part of the triangle rasterization pipeline. ![[image-50.png]]
* Training details: “For real scene data, we regularize our network by adding random Gaussian noise with zero mean and unit variance to the output σ values (before passing them through the ReLU) during optimization, finding that this slightly improves visual performance for rendering novel views. We implement our model in Tensorflow” (Mildenhall et al., 2020, p. 17)
* Rendering Details: To render new views at test time, we sample 64 points per ray through the coarse network and 64 + 128 = 192 points per ray through the fine network, for a total of 256 network queries per ray.
![[image-51.png]]

* “resulting in between 150 and 200 million network queries per rendered image.” (Mildenhall et al., 2020, p. 18)
* 