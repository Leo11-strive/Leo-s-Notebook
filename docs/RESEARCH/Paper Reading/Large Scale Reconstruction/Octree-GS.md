* 3DGS: it struggles in large-scale scenes due to the high number of Gaussian primitives, particularly in zoomed-out views, where all primitives are rendered regardless of their projected size.
* Octree-GS: a Level-of-Detail (LOD) structured approach that dynamically selects appropriate levels from a set of multi-scale Gaussian primitives

## Related Works
* A key drawback of 3D-GS is the misalignment between the distribution of 3D Gaussians and the actual scene structure
* Yet, their drawback is every object detail is rendered, regardless of distance, leading to redundant computations and inconsistent rendering speeds
![[image-398.png]]
* 3DGS:
> This limitation hampers its practicality in speed-demanding applications, such as gaming in open-world environments and other immersive experiences, particularly for large indoor and outdoor scenes with computation-restricted devices.
* “NSVF [20] efficiently skipping the empty voxels leveraging the neural implicit fields structured in sparse octree grids.” (Ren et al., 2024, p. 3)
> 稀疏八叉树来跳过 empty 体素点
* PlenOctree [17] stores the appearance and density values in every leaf to enable highly efficient rendering.
* “improved capabilities such as real-time rendering stability irrespective of trajectory changes.” (Ren et al., 2024, p. 3)
* LOD：NeRF 本身积分时，所有的点都是趋近于无限小，而我们在采样的时候，很有可能会把及其细小的地方遗漏，本来高频的地方变成低频，Mip-NeRF 把点放大
![[image-444.png]]

* City-Gaussian
> manual distance threshold adjustments
> 闪烁，跳变

## Method
* “hierarchically organizes anchors into an octree structure to learn a neural scene from multiview images” (Ren et al., 2024, p. 4)
* “Each anchor can emit different types of Gaussian primitives, such as explicit Gaussians [5], [15] and neural Gaussians [3]” (Ren et al., 2024, p. 4)
* “These anchors are positioned at the centers of sparse, uniform voxel grids with varying voxel sizes. Specifically, anchors with higher LOD L are placed within grids with smaller voxel sizes” (Ren et al., 2024, p. 4)
* “Note that our LOD design is cumulative: the rendered images at LOD K rasterize all Gaussian primitives from LOD 0 to K” (Ren et al., 2024, p. 4)
![[image-542.png]]

* select correct anchors
![[image-543.png]]

* 梯度 threshold
![[image-544.png]]
* 巨大落差，不够细致
* “define ‘view-frequency’ as the probability that anchors are selected in the training views, which directly correlates with the received gradient. We remove anchors with the view-frequency below τv, where τv represents the visibility threshold. This strategy effectively eliminates floaters, improving visual quality and significantly reducing storage, as demonstrated in Fig. 4.” (Ren et al., 2024, p. 5)
* “begins by training on a subset of anchors representing lower LOD levels” (Ren et al., 2024, p. 6)
![[image-545.png]]


