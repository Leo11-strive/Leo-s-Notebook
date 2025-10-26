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

