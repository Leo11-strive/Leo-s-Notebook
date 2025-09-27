* “starting from sparse points produced during camera calibration, we represent the scene with 3D Gaussians that preserve desirable properties of continuous volumetric radiance fields for scene optimization while avoiding unnecessary computation in empty space” (Kerbl et al., 2023, p. 1)
* “perform interleaved optimization/density control of the 3D Gaussians, notably optimizing anisotropic covariance to achieve an accurate representation of the scene” (Kerbl et al., 2023, p. 1)
* “develop a fast visibility-aware rendering algorithm that supports anisotropic splatting and both accelerates training and allows realtime rendering” (Kerbl et al., 2023, p. 1)

## Introdcution

* NeRF and other continuous methods, using voxel grids, hasg grids, points
* interpolation
* stochastic sampling required for rendering is costly and can  result in noise
* Meshes and points are the most common 3D scene representations  because they are explicit and are a good fit for fast GPU/CUDA-based  rasterization.
* 3D Gaussians as a flexible and expressive scene representation
* cameras calibrated with Structure-from-Motion (SfM)
* initialize the set of 3D Gaussians with the sparse point  cloud produced for free as part of the SfM process
* The second component  of our method is optimization of the properties of the 3D Gaussians  – 3D position, opacity α, anisotropic covariance, and spherical harmonic (SH) coefficients
* interleaved with adaptive density control  steps, where we add and occasionally remove 3D Gaussians during  optimization
![[image-67.png]]

## Related Works
* *SFM:* SfM estimates a sparse point cloud during  camera calibration, that was initially used for simple visualization  of 3 D space.
* MVS 对无纹理、反射/透明、薄结构、运动物体常“掉点”。几何上没有 $\mathbf{X}$，就**没法给像素找来源**，只能留洞、拉花或靠启发式修补(inpainting/拉伸），效果有限。
    
- **过度重建/假几何（floaters/ghosts）**：MVS 把高光、阴影、重复纹理当“真实表面”重建出来。重投影时这些假面会**错误遮挡**真实表面，或把错误颜色“硬拷贝”到新视角，产生双影、撕裂、条纹等伪影。
### Neural Rendering and Radiance Fields
* temporal flickering: CNN 每一帧独立预测 blending weights，如果没有显式的时间一致性约束：
    
    - 帧 A：某个像素 CNN 判定用相机1的颜色；
        
    - 帧 B：相邻视角 CNN 判定用相机2的颜色；
        
    - 结果：颜色突然跳变 → **闪烁（flickering）**。



* hash grid + occupancy grid / sparse voxel grid
![[image-68.png]]
*  NeRF 仍然采用了视角转换：视角相关性（现实世界的不同表征方式）
*  “Pulsar [Lassner and Zollhofer 2021] achieves fast sphere rasterization which inspired our tile-based and sorting renderer.” (Kerbl et al., 2023, p. 3)
* 球谐函数：球面函数的正交基展开

## Main Process
* “The input to our method is a set of images of a static scene, together with the corresponding cameras calibrated by SfM [Schönberger and Frahm 2016] which produces a sparse point cloud as a sideeffect.” (Kerbl et al., 2023, p. 4)
* “From these points we create a set of 3D Gaussians (Sec. 4), defined by a position (mean), covariance matrix and opacity α, that allows a very flexible optimization regime” (Kerbl et al., 2023, p. 4)
* “The directional appearance component (color) of the radiance field is represented via spherical harmonics (SH)” (Kerbl et al., 2023, p. 4)
* The key to the efficiency of our method is our tile-based rasterizer  (Sec. 6) that allows α-blending of anisotropic splats, respecting visibility order thanks to fast sorting.

## Differentiable 3D GAUSSIAN SPLATTING
* “we model the geometry as a set of 3D Gaussians that do not require normals.” (Kerbl et al., 2023, p. 4)
* Gaussians are defined  by a full 3D covariance matrix Σ defined in world space [Zwicker  et al. 2001a] centered at point (mean) μ
![[image-69.png]]
* “This Gaussian is multiplied by α in our blending process.” (Kerbl et al., 2023, p. 4)
* project from 3D to 2D gaussian
* 视角转换矩阵 $W$ ,相机坐标
* 投影矩阵，使用雅各比矩阵，一阶泰勒，做近似（协方差线性传播）
![[image-70.png]]
* “Zwicker et al. [2001a] also show that if we skip the third row and column of Σ′, we obtain a 2×2 variance matrix with the same structure and properties as if we would start from planar points with normals, as in previous work” (Kerbl et al., 2023, p. 4)
* 就是丢掉深度
* covariance matrix 是半正定的
![[image-71.png]]
* 对这个对称的矩阵做分解
* “a quaternion q to represent rotation.” (Kerbl et al., 2023, p. 4)
* > 用四元数表征旋转
* “making sure to normalize q to obtain a valid unit quaternion.” (Kerbl et al., 2023, p. 4)
* “To avoid significant overhead due to automatic differentiation during training, we derive the gradients for all parameters explicitly.” (Kerbl et al., 2023, p. 4)

## Optimization with Adaptive Density Control of 3D Gaussians
* “we also optimize SH coefficients representing color c of each Gaussian to correctly capture the view-dependent appearance of the scene.” (Kerbl et al., 2023, p. 4)
* “interleaved with steps that control the density of the Gaussians to better represent the scene.” (Kerbl et al., 2023, p. 4)
![[image-77.png]]
* “sigmoid activation function for α to constrain it in the [0 − 1) range” (Kerbl et al., 2023, p. 5)
* 参数需要 activation function，学的是协方差矩阵和透明度的参数
* “initial covariance matrix as an isotropic Gaussian with axes equal to the mean of the distance to the closest three points.” (Kerbl et al., 2023, p. 5) 先初始化为各向同性的三维高斯，轴长的量度依据he mean of the distance to the closest three points.
* 训练早期希望高斯的**中心位置** $\mu_i$ 能够大幅移动以对齐几何；后期则希望稳定不再乱飘。因此给**位置的学习率**用指数衰减
* 损失函数：$\mathcal{L}=(1-\lambda)\mathcal{L}1+\lambda\,\mathcal{L}{\text{D-SSIM}}$

  - $\mathcal{L}_1$：预测图像与真值图像的像素 L1 误差（逐像素、可加权）。
    
  - $\mathcal{L}_{\text{D-SSIM}}$：结构相似度的**不相似**项（常用 $\text{D-SSIM}=\frac{1-\text{SSIM}}{2}$），鼓励结构/纹理对齐、减少“糊”的外观。
    
  - $\lambda$：权重（常取 0.1–0.2 量级；实现中也可 schedule）。
- 3DGS 采用 $\lambda = 0.2$
- “After optimization warm-up (see Sec. 7.1), we densify every 100 iterations and remove any Gaussians that are essentially transparent, i.e., with α less than a threshold εα” (Kerbl et al., 2023, p. 5) "adaptive control of gaussians"
- “Densify Gaussians with an average magnitude of view-space position gradients above a threshold τpos, which we set to 0.0002 in our tests” (Kerbl et al., 2023, p. 5)
- 小高斯（高斯不足）：“by simply creating a copy of the same size, and moving it in the direction of the positional gradient” (Kerbl et al., 2023, p. 5)
- “We replace such Gaussians by two new ones, and divide their scale by a factor of φ = 1.6 which we determined experimentally. We also initialize their position by using the original 3D Gaussian as a PDF for sampling.” (Kerbl et al., 2023, p. 5) 用旧高斯本身当作 3D 概率密度函数（PDF）去**采样两个位置**$\mu_1,\mu_2 \sim \mathcal N(\mu, \Sigma)$ ,- 把每个新高斯的**标准差**按因子 $\phi=1.6$ 缩小 $\quad \Sigma=R\,\mathrm{diag}(\lambda_1,\lambda_2,\lambda_3)\,R^\top \ \Rightarrow\ \Sigma’ = R\,\mathrm{diag}\!\Big(\tfrac{\lambda_1}{\phi^2},\tfrac{\lambda_2}{\phi^2},\tfrac{\lambda_3}{\phi^2}\Big)\,R^\top$
* 