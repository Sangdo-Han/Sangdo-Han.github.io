---
layout: default
title: 3D Image Reconstruction
parent: Computer Vision
grand_parent: AI Review
math: katex
nav_order: 1
---

# 3D Reconstruction

Review survey (23.07.02)
{: .label .label-green}

Review Detailed Methodology (comming-soon)
{: .label .label-yellow}

## Introduction

3D reconstruction, which is 3-Dimensional representation of objects,  can be used for many applications such as video games, animation, navigation and so on.    

There are many traditional methods in 3D reconstruction like SfM(Structure from Motion), Dense Reconstruction and MVS(Multi-View Stereo). Those traditional methods are based on photogrammetry.   

It is true that understanding the basics of photogrammetry is very important to understand deep-learning based 3D reconstruction because deep-learning based methods are built on top of these photogrammetry techniques.   

 In this post, however, it is assumed that the readers have knowledge about deep learning models rather than 3D reconstructions. Hence, the discussion will focus on deep-learning based 3D reconstructions while traditional techniques will be mentioned only as needed for readers to comprehend the deep-learning models.

## 3D reconstruction using deep learning : a survey [[1](#jin-et-al)]

Starting with a good review paper helps to understand the field and the potential research directions. In deep learning based 3D reconstruction, luckily we can access a good review paper [[1](#jin-et-al)] for free.    

### Datasets
The paper[[1](#jin-et-al)] show us several useful dataset for 3D reconstructions:

1. [ShapeNet](https://shapenet.org/)    
It is a very large scale dataset for CAD models developed by Chang et al.[[2](#shapenet)] from Stanford University, Princeton University and the Toyota Technological Institute at Chicago, USA.

2. [Pascal3D+](https://cvgl.stanford.edu/projects/pascal3d.html)    
It is a multi-view datasets, if you are familiar with Object Detection Task, you might be heard of PASCAL VOC dataset. Pascal3D+ has 12 categories of rigid object from PASCAL VOC 2012. This dataset is developed by Yu Xiang et al[[3](#pascal3d)] from Computational Vision and Geometry Lab at Stanford University.

3. [ObjectNet3D](https://cvgl.stanford.edu/projects/objectnet3d/)  
It is a large scale database for 3D objects. Like as Pascal3D+, this dataset is developed by Yu Xiang et al[[4](#objectnet)] from Computational Vision and Geometry Lab at Stanford University.

4. [KITTI](https://www.cvlibs.net/datasets/kitti/)  
If you are familiar with Computer Vision in automous driving and mobile robotics, I am sure that you heard of this dataset. This was introduced by Andres Geiger et al[[5](#kitti-dataset)].    

### Metrics
The paper[[1](#jin-et-ßal)] briefly explains 3 commonly used metric used: MSE, Voxel IoU and cross-entropy. I think the reader might familiar with MSE and cross-entropy. Therefore, I will skip their details.

***Voxel IoU***

In many cases in Computer Vision, IoU represents an abbreviation of Intersection over Union. As you might guess. Voxel IoU is mere volumetric extension of 2D-pixel-IoU. which is:    

$${IoU={G \cap P \over G \cup P}}$$  

Here, G stands for a set of voxels in a ground truth, while P stands for a  set of voxels in prediction/reconstruction. This metric is also widely used for 3D object detection or segmentation.

Also, for point cloud and mesh representation, sevaral distance metrics between groundtruth and reconstruction can be used as metric. I beleive that readers are familiar with Euclidean Distance, I will post about Chamfer Distance and EMD instead. 

***Chamfer Distance***

Chamfer Distance is average of the summation of closest point pairs. 

$$Chamfer(G,P) = {1\over{n}}(\sum_i \min_j(||g_i - p_j||) + \sum_j \min_i(||g_i - p_j||))$$

More precisely, The formula represents average of distance from each point in one point cloud to its closest point in the other point cloud. and vice versa.  

***EMD***   

Earth mover's distance (EMD), also known as the Wasserstein distance, stands for the distance between probability distributions over a region in statistics. 

$$EMD(P, \hat{P})=\min_{\phi:P\rightarrow\hat{P}} \sum_{p_i\in P}||p_i - \phi(p_i)||$$

This can be computed using the Hungarian Algorithm or Sinkhorn-Knopp algorithm. 

---------    
  

### 3D Computer Vision Representation     
        
The original survey [[1](#jin-et-al)] divides the reconstruction techniques into two main categories by the number of sources. (Reconstruction based on single image or multiple images.) After that, it lists up middle categories with way of representations or how outcome look like with a short descriptions.     

These could be very helpful for the researchers who are familiar with 3D Computer Vision. However, for the newcomers (I assume that most of readers of survey papers are newcomers), I think that it would be more effective writing if it explains the ways of representing 3D object first, and then explains the models and results.


***Voxel Representation***

Voxel is a volumetric representation which can be comparing to pixel representation in 2D images. Actually, by adding depth (D) into pixel, Voxel achieves representation of 3D.

$$pixel : H \times W \times C$$
$$voxel : H \times W \times D \times C$$
$$, where \ H : height , \ W : width, \ D : depth, \ C : channel \ (color) $$

***Point Cloud Representation***

Point Cloud is another way to represent 3D object. A single point cloud is a collection of 3D points (mostly a collection of Cartesian coordinate positions) : 

```python
point_cloud = [
    [x1, y1, z1],
    [x2, y2, z2],
    ...
    [xN, yN, zN]
]
```

***Mesh Representation***

Mesh representation could be another option. Mesh is a collection of base geometry to represent surface. Since the least number of points to construct a surface is 3 (triangle), triangle is widely used for mesh representations:

```python
mesh_representation = [
    [point_1, point_2, point_3],
    [point_4, point_5, point_6],
    ...
    [point_a, point_b, point_c]
]
```

Each point (vertex) represents 3d point (x,y,z)

***Other Representation***

*Implicit Surface*

Implicit Surface is a model based surface representation. Models can be a continuous decision boundary of 
deeplearning network classifiers, suggested in OccNet[6](#OccNet), or multi-layer network architecture to extract geometry features 
and represents 3D shapes in an Euclidean preserving latent space as in UCLID-Net[7](#EUCLID-Net)
This concept is novel but thinking of the mathematics, we can easily see the concepts:    

$$f_{model}(x,y,z) = 0$$    

which resembles our well-known implicit surface:

$$f(x,y,z) = x^2+y^2+z^2 -1 =0$$

a sphere!


*depth*    

About generating depths based on a 2D images, 3D representation also be achieved.



# References
<span id="jin-et-al">[1]</span> Jin, Y., Jiang, D., & Cai, M. (2020). 3d reconstruction using deep learning: a survey. Communications in Information and Systems, 20(4), 389-413.

<span id="shapenet">[2]</span> Chang, A. X., Funkhouser, T., Guibas, L., Hanrahan, P., Huang, Q., Li, Z., ... & Yu, F. (2015). Shapenet: An information-rich 3d model repository. arXiv preprint arXiv:1512.03012.  

<span id="pascal3d">[3]</span> Y. Xiang, R. Mottaghi and S. Savarese, "Beyond PASCAL: A benchmark for 3D object detection in the wild," IEEE Winter Conference on Applications of Computer Vision, Steamboat Springs, CO, USA, 2014, pp. 75-82, doi: 10.1109/WACV.2014.6836101.

<span id="objectnet">[4]</span> Xiang, Y., Kim, W., Chen, W., Ji, J., Choy, C., Su, H., ... & Savarese, S. (2016). Objectnet3d: A large scale database for 3d object recognition. In Computer Vision–ECCV 2016: 14th European Conference, Amsterdam, The Netherlands, October 11-14, 2016, Proceedings, Part VIII 14 (pp. 160-176). Springer International Publishing.  

<span id="kitti-dataset">[5]</span> A. Geiger, P. Lenz and R. Urtasun, "Are we ready for autonomous driving? The KITTI vision benchmark suite," 2012 IEEE Conference on Computer Vision and Pattern Recognition, Providence, RI, USA, 2012, pp. 3354-3361, doi: 10.1109/CVPR.2012.6248074.

<span id="OccNet">[6]</span> L. Mescheder, M. Oechsle, M. Niemeyer, S. Nowozin, and A. Geiger,
“Occupancy networks: Learning 3d reconstruction in function space,” in
Proceedings of the IEEE Conference on Computer Vision and Pattern
Recognition, 2019, pp. 4460–4470.

<span id="EUCLID-Net">[7]</span> L. Mescheder, M. Oechsle, M. Niemeyer, S. Nowozin, and A. Geiger,
“Occupancy networks: Learning 3d reconstruction in function space,” in
Proceedings of the IEEE Conference on Computer Vision and Pattern
Recognition, 2019, pp. 4460–4470.