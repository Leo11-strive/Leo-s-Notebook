# Essay Reading

## 3DGS:

* preknowledge

> * NERF: 通过多角度的照片view，通过position encoding，处理位置的高频信息，输入位置和摄像机角度，输出颜色和density，同时在模型训练好之后，通过类似ray tracing的方法，为每个ray上的点采样，从而得到某一方向上的点的信息。做体渲染，因为其可微分
> * $C(\mathbf{r}) = \int_{t_n}^{t_f} T(t) \sigma (\mathbf{r}(t)) \mathbf{c} (\mathbf{r}(t), \mathbf{d}) dt, \quad \text{where} \quad T(t) = \exp \left( - \int_{t_n}^{t} \sigma (\mathbf{r}(s)) ds \right)$
> * 把射线分成N个小区间加权
> * 一个粗糙网络（Coares）一个精细网络（Fine）。粗糙网络是在均匀采样得到比较少$(N_c)$的点进行渲染并训练的网络，用来输出进行采样概率估计。然后用$\omega_{i}$的概率分布采样，训练精细网络。

!!! note neural network architecture of NERF

    ![alt text](<Screenshot 2025-03-20 at 15.26.59.png>)
    
    * 把坐标$(x,y,z)$映射出60维的向量，输入到全连接网络得到密度 $\sigma$；视角$(\theta, \phi)$映射得到的24维向量和$\phi$的前一层输出特征拼接在一起，经过两层MLP得到RGB值。值得注意的是，为了加强坐标信信，坐标的信息会在网络中间再输入一次。这里的网络结构设计体现了，密度和视角无关，颜色和视角相关。

* **3DGS**

> * 3DGS 是在离散和连续间的一个平衡：在高斯球内部是连续的、可微的；在整个空间中，每个高斯球又是离散的
> * NeRF中的噪声在随机采样，蒙特卡洛积分，网络拟和优化时会产生
> * 3DGS辐射场
> * 三维高斯分布：用来表征3D点
$L_{3DGS}(x,y,z,\theta,\phi)=\sum_{i=1}G(x, y, z, \mu_i, \sigma_i) \cdot c_i(\theta,  \phi)$
> * $c_i(\theta, \phi)$是一个球谐函数，表示不同视角下的颜色
> * 球谐函数
> * real-time rendering
> * 初始化操作
> $\sigma$是正定矩阵，所以$RSS^TR^T$来初始化
> ågradient descent 把梯度传到下一层
> 由于放缩变换都是沿着坐标轴，所以只需要一个3D向量，旋转则用四元数表达
> * 以高斯概率分布构建三维模型
> * Splattiing



