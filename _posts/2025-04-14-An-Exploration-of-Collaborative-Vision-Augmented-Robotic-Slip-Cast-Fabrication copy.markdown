---
layout: post
title: An Exploration of Collaborative Vision-Augmented Robotic Slip Cast Fabrication
date: 2025-04-14
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: kuka-setup.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
---
For my undergraduate thesis, I developed a proof-of-concept robotic system that explores how computer vision and robotic motion can be used to dynamically shape liquid clay within a mold. 

---

## Demo Videos
<iframe width="900" height="506" src="https://www.youtube.com/embed/077-BA6oasE" frameborder="0" allowfullscreen></iframe>
<iframe width="900" height="506" src="https://www.youtube.com/embed/FSrjYMK2hRw" frameborder="0" allowfullscreen></iframe>

## Project Context & Opportunity

Traditional **slip casting** is efficient for mass production but inherently inflexible. Each new form needs a new plaster mold, which increases cost, waste, and limits design potential. Meanwhile, robotic clay work has focused mostly on **solid-state extrusion and carving**, not liquid slip.

There was a gap: **Could we manipulate slip dynamically, using sensing and actuation, to create multiple shapes from one mold?**


## The Aim

Develop a **collaborative vision-augmented robotic slip casting system** to:
- Lay the groundwork for a collaborative vision augmented robotic slip cast fabrication method
- Enable a robot to manipulate slip within a single mold to produce varied forms
- Demonstrate basic ‘slip control’ through sensing and actuation
- Push to increase collective material intuition and understanding
  


## What I Built

- A **6-DoF KUKA robot** mounted on a linear rail  
- A **monocular eye-in-hand camera** tracking slip inside a semisphere mold  
- A **Python + OpenCV** pipeline for detecting slip silhouettes  
- An interface using **Grasshopper** + **KUKA\|PRC** to plan and execute mold motions  
- **Open Sound Control (OSC)** for communication between vision and control layers  


## Key Experiments

Two shapes were targeted:
- A **uniform bowl (circle)** with even slip distribution  
- A **four-lobed star** with alternating high and low walls  

The robot used vision data to adjust the mold’s orientation and redistribute the slip. Testing was done with both a cream simulant (for rapid iteration) and actual ceramic slip.

**Results:**
- The system achieved distinct profiles using *one mold*
- Average silhouette errors:  
  - Circle (cream): **5.3%**  
  - Circle (clay slip): **11.9%**  
  - Star (clay slip): **22.5%**  
- Vision tracking proved effective, even with a simple webcam setup


## Human-Robot Collaboration

The system was not fully autonomous—by design. A **human operator**:
- Poured and handled the clay
- Intervened if necessary
- Interpreted material states that were difficult to model

This hybrid workflow made the system more robust and reflective of real-world fabrication.


## Why It Matters

This project:
- Reduces **mold waste** and expands **design possibilities**
- Demonstrates a **low-cost, adaptive** approach to working with complex materials
- Blends **robotics, computer vision, and human intuition** into a new ceramic workflow

It offers a step toward **material-aware, collaborative robotic fabrication**—where robots don’t replace artisans, but enhance them.


## Next Steps

- Real-time feedback control  
- Non-symmetric and novel mold geometries  
- Integration into full ceramic production workflows  
- More autonomy via reinforcement learning  



## Check Out My Final Presentation Slides
<div style="width:100%;max-width:800px;">
  <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vSyte0RCSGf7R4UxfXSig7t2iHiQxmiGFAfS-6GFyho1KGQfKM4pKJSpvM080q2qVe8F5Hr9jl6kMdS/pubembed?start=false&loop=false&delayms=3000" frameborder="0" width="800" height="480" allowfullscreen></iframe>
</div>

## Read My Report
[Download (PDF)](/assets/files/EmilyWinfield_ThesisDocument.pdf)

---

*Project supervised by Prof. Maria Yablonina  
Completed as part of ESC499, University of Toronto*