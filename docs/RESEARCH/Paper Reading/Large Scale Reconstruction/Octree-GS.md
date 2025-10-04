* 3DGS: it struggles in large-scale scenes due to the high number of Gaussian primitives, particularly in zoomed-out views, where all primitives are rendered regardless of their projected size.
* Octree-GS: a Level-of-Detail (LOD) structured approach that dynamically selects appropriate levels from a set of multi-scale Gaussian primitives

## Introduction
* A key drawback of 3D-GS is the misalignment between the distribution of 3D Gaussians and the actual scene structure
* Yet, their drawback is every object detail is rendered, regardless of distance, leading to redundant computations and inconsistent rendering speeds