* Each of these anchors tethers a set of neural Gaussians with learnable offsets, whose attributes (i.e. opacity, color, rotation, scale) are dynamically predicted based on the anchor feature and the viewing position. Unlike the vanilla 3D-GS which allows 3D Gaussians to freely drift and split, our strategy exploits scene structure to guide and constrain the distribution of 3D Gaussians, whilst allowing them to locally adaptive to varying viewing angles and distances. We further develop the corresponding growing and pruning operations for anchors to enhance the scene coverage.
* “hash grid and achieved drastically faster feature query” (Lu et al., 2023, p. 2)

## Method
* “The physical properties of neural Gaussians are decoded from the learned anchor features” (Lu et al., 2023, p. 3)
![[image-546.png]]
* “We then remove duplicate entries, denoted by {·} to reduce the redundancy and irregularity in P.” (Lu et al., 2023, p. 3)
* The center of each voxel v ∈ V is treated as an anchor point, equipped with a local context feature fv ∈ $R^{32}$, a scaling factor lv ∈ R3, and k learnable offsets Ov ∈ Rk×3
* We further enhance fv to be multi-resolution and view-dependent
* we 1) create a features bank {fv, fv↓1 , fv↓2 }, where ↓n  denotes fv being down-sampled by 2n factors; 2) blend the feature bank with view-dependent weights to form an integrated anchor feature fˆv
![[image-547.png]]

![[image-548.png]]

![[image-549.png]]

* “more efficient, we only keep neural Gaussians whose opacity values are larger than a predefined threshold τα.” (Lu et al., 2023, p. 4)

* “growing policy that grows new anchors where neural Gaussians find significant.” (Lu et al., 2023, p. 4)
* “and a new anchor point is thereby deployed at the center of that voxel if there was no anchor point established.” (Lu et al., 2023, p. 4)

![[image-550.png]]
* multi-resolution voxel grid
![[image-551.png]]

![[image-552.png]]

![[image-553.png]]

* “found that the initial points play a crucial role for high-fidelity results.” (Lu et al., 2023, p. 8)
* “To enhance the view-adaptability, we update the anchor feature through a view-dependent encoding.” (Lu et al., 2023, p. 12)
![[image-555.png]]

![[image-556.png]]
* “adapted to point cloud density, yielding denser anchors with enhanced rendering quality but might introduce more computational overhead;” (Lu et al., 2023, p. 13)

