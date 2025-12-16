* Nevertheless, it falls short in capturing intricate geometry since the volumetric 3D Gaussian, which models the complete angular radiance, conflicts with the thin nature of surfaces.
* “Specifically, 3DGS evaluates a Gaussian’s value at the intersection between a pixel ray and a 3D Gaussian [Keselman and Hebert 2022, 2023], which leads to inconsistency depth when rendered from different viewpoints.” (Huang et al., 2024, p. 2)
* “While our 2D Gaussian approach excels in geometric modeling, optimizing solely with photometric losses can lead to noisy reconstructions, due to the inherently unconstrained nature of 3D reconstruction tasks,” (Huang et al., 2024, p. 2)
* “The depth distortion term concentrates 2D primitives distributed within a tight range along the ray, addressing the rendering process’s limitation where the distance between Gaussians is ignored” (Huang et al., 2024, p. 2)
* “ensuring alignment between the geometries defined by depth and normals” (Huang et al., 2024, p. 2)
![[image-1138.png]]

* “Third, the rasterization process in 3DGS lacks multiview consistency, leading to varied 2D intersection planes for different viewpoints” (Huang et al., 2024, p. 3)
* “Additionally, using an affine matrix for transforming a 3D Gaussian into ray space only yields accurate projections near the center, compromising on perspective accuracy around surrounding regions [Zwicker et al. 2004].” (Huang et al., 2024, p. 3)
![[image-1141.png]]

![[image-1142.png]]

![[image-1143.png]]

![[image-1144.png]]

![[image-1145.png]]
