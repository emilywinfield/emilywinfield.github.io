---
layout: post
title: KUKA Robotic Arm – Path & Trajectory Planning
date: 2024-11-29
description: A hands-on lab project applying kinematics, motion planning, and control theory to a 6‑DoF KUKA industrial arm using MATLAB.
img: kuka-drawing.jpg
fig-caption: Sample trajectory plotted in MATLAB
---

I worked with a **6‑DoF KUKA industrial manipulator**, implementing core robotics algorithms in **MATLAB**. The project focused on:

- **Forward and inverse kinematics** to calculate pose–joint relationships  
- **Trajectory generation** and control theory for smooth, stable motion  
- **Motion planning** using **artificial potential fields** to avoid obstacles

---

<iframe width="900" height="506" src="https://www.youtube.com/embed/hTpqYt6auBM" frameborder="0" allowfullscreen></iframe>

## What I Did

- Modeled the KUKA arm using **Denavit–Hartenberg parameters** and derived both forward and inverse kinematic solutions  
- Computed **Jacobian matrices** for mapping body velocities to joint space and identifying singularities  
- Designed **artificial potential field planners** to generate collision-free paths  
- Created **smooth trajectory profiles** (e.g. trapezoidal velocity) for point-to-point and continuous motion  
- Simulated **control behavior and joint dynamics** in MATLAB before deploying to a real or virtual robot


## Tools & Concepts

- **MATLAB Robotics Toolbox** and symbolic computation for solving kinematics  
- **Control theory** principles for trajectory tracking and stability  
- **Numerical validation** of workspace reachability, singularities, and plan feasibility


## Outcome & Learning

The lab provided direct experience in linking mathematical modeling with practical robot motion behavior. I developed a deeper understanding of how **kinematics and dynamic models translate into real-world trajectory tracking and planning**, and gained proficiency in both algorithm design and simulation for industrial manipulators.
