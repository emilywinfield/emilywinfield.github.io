---
layout: post
title: Computer Vision Projects
date: 2024-09-21
description: Computer Vision Projects
img: code.jpg
fig-caption: 
---

As part of ROB501: Computer Vision for Robotics (Fall 2024), I developed and tested a range of computer vision algorithms used in robotic perception, depth estimation, and visual servo control. Each project was implemented in Python and evaluated on real-world and benchmark datasets, with an emphasis on algorithm design, performance optimization, and result visualization.

---

## Camera Pose Estimation (Planar Target)
- **Gaussian smoothing** + **subpixel saddle-point** fitting to extract checkerboard cross-junctions.  
- Ordered 2D features and paired them with known 3D target points.  
- Derived the **full pose Jacobian** and solved **PnP** with **nonlinear least squares** (Euler-angle parameterization).

<p float="left">
  <img src="/assets/img/rob501-ass2-part1.png" width="45%" />
  <img src="/assets/img/rob501-ass2-part2.png" width="45%" />
</p>

![img]({{site.baseurl}}/assets/img/rob501-ass2-part4.jpg)

---

## Stereo Correspondence & Depth
- Baseline: **SAD block matching** (fixed window, winner-take-all) with disparity bounds and valid-overlap masks.  
- Improved matcher that exceeded the baseline on datasets like **Middlebury**/**KITTI** (and Mars rover imagery).  
- Evaluated via **RMS disparity error** and **bad-pixel rate**; tuned parameters for speed/quality trade-offs.

<p float="left">
  <img src="/assets/img/rob501-ass3.png" width="45%" />
  <img src="/assets/img/cones.png" width="45%" />
</p>

---

## Image-Based Visual Servoing (IBVS)
- Implemented the **image Jacobian** and a **pseudo-inverse (Moore–Penrose)** based controller.  
- Proportional control to drive observed features toward desired image positions.  
- Added **depth refinement** to handle uncertainty; experimentally tuned gains for convergence and stability.

![img]({{site.baseurl}}/assets/img/rob501-ass4-demo.gif)

---

## Image Warping & Homography
- Computed homographies via **Direct Linear Transform (DLT)**.  
- Used **bilinear interpolation** for accurate resampling and **histogram equalization** for contrast.  
- Demo: perspective-correct “billboard replacement” in real scenes.

![img]({{site.baseurl}}/assets/img/billboard-hack.png)
