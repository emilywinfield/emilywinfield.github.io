---
layout: post
title: Autonomous Wildfire Surveying Drone
date: 2025-04-11
description: An autonomous drone system for wildfire surveying and rescue, developed by a four-person capstone team using ROS 2, real-time perception, and occupancy grid mapping.
img: drone.jpg
fig-caption: Indoor flight testing with printed fire and human targets
---

As part of my final-year engineering capstone project, I co-led the development of an **autonomous wildfire surveying drone** designed to assist emergency responders with real-time aerial mapping and hazard localization. Our team of four, *Team DARE*, delivered a full-stack robotic system that detects fire hotspots and human survivors, generates a live 2D map, and autonomously navigates based on mission logic.

We tackled this challenge using an integrated platform built with **Python**, **ROS 2**, and a **Jetson Nano**, working in tandem with a **Pixhawk PX4 flight controller** and **visual-inertial odometry (VIO)**. Our custom software stack performed onboard perception, waypoint planning, and occupancy grid mapping in real time.

---

## My Role

I led the **technical coordination** of the project and contributed to the **software development** across perception, mapping, and control subsystems. My responsibilities included:

- End-to-end **system integration**
- **Hardware setup** of the Jetson Nano, VIO, and PX4 flight controller
- **Team workflows**, timeline planning, and test coordination
- Cross-functional debugging during perception-mapping-navigation integration

My software contributions focused on:

- Developing the **ROS 2 node for real-time person and fire detection** using OpenCV and PyTorch
- Integrating an **ArUco marker detection pipeline** for fire localization and geotagging
- Writing **image preprocessing routines**, including frame throttling, downsampling, and camera calibration
- Assisting in the development of the **occupancy grid mapping node** for visualizing hazards in 2D space
- Working on the **VIO integration** from the Intel RealSense T265 into the ROS 2 transform tree, including calibration of coordinate frames across camera, body, and world
- Contributing to the integration of the **control node** between ROS 2 and PX4 via MAVROS for autonomous waypoint execution
- Debugging complex **inter-node communication** in ROS 2 (topics, transforms, and service calls)
- Coordinating **subsystem testing**, flight timeline planning, and final system integration

This role required both hands-on implementation and higher-level systems thinking, especially when troubleshooting interactions between perception, localization, and control under real-time constraints.

## System Architecture

The drone platform combines the following components:

- **Jetson Nano**: Runs lightweight real-time inference (SSD-Mobilenet) and ArUco fire marker detection
- **Intel RealSense T265**: Provides VIO-based localization for mapping and waypoint tracking
- **Monocular RGB Camera**: Streams 720p video for onboard person and fire detection
- **PX4 Flight Controller**: Executes low-level control via MAVROS and ROS 2
- **ROS 2 Nodes**: Handle modular pipelines for:
  - Perception (human + fire detection)
  - Global 2D occupancy grid mapping
  - Waypoint generation and adaptive second-pass rescue logic


## Key Features

- **Fire & Person Detection**  
  Onboard inference detects ArUco markers (fires) and human silhouettes, geolocated using real-time pose.

- **Occupancy Grid Mapping**  
  Grid cells update live with detection log-odds and are published as `nav_msgs/OccupancyGrid` to RViz for operator feedback.

- **Autonomous Navigation**  
  First pass: lawnmower pattern; second pass: fly low to revisit detected humans for simulated rescue.

- **Fully Onboard & Untethered**  
  All processing runs on Jetson Nano; system operates without ground-station dependency during missions.


## Indoor Flight Demo

<iframe width="900" height="506" src="https://www.youtube.com/embed/KhkN0I3-Mu8" frameborder="0" allowfullscreen></iframe>

We validated our system in an indoor Vicon motion capture arena, using printed ArUco tags and celebrity photos to simulate fire and humans. The drone successfully mapped hazards and executed both survey and revisit phases.


## Challenges & Reflections

- **Jetson Nano Limitations**: We had to optimize neural inference and image processing for low-power compute, which shaped many design decisions (e.g. simplified image sizes, frame throttling).
- **Sensor Constraints**: Lacking thermal and depth sensors, we used visual markers to validate the core perception → mapping → planning stack.
- **Testing Time**: Limited flight slots required us to log ROS bag data extensively and debug offline between flights.
- **System Integration**: Coordinating VIO, ROS topics, camera calibration, and controller tuning was non-trivial—especially under time pressure.

Despite these challenges, we built a working prototype that lays groundwork for future iterations with thermal and LiDAR sensors.


## Looking Forward

This project taught us how to design, integrate, and deploy a robotic system under real-world constraints. With additional sensing and compute upgrades (e.g. Jetson Orin Nano + thermal IR), this platform could move toward deployment-ready wildfire surveying and rescue operations.

---

*Completed as part of ROB498 Capstone Project
In collaboration with Daniel Asadi, Rayhanneh Behravesh, and Alyssa Wing*