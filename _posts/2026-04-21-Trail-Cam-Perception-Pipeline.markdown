---
layout: post
title: 🦌 Trail Cam Perception — Field Deployment Pipeline
date: 2026-04-21
description: A production-ready two-stage computer vision pipeline that automatically sorts and identifies wildlife from trail camera images, with a Streamlit UI and auto-generated PDF reports.
img: haycock-deer.JPG
fig-caption:
---

Following my [evaluation of SpeciesNet and domain shift on the Caltech Camera Traps dataset]({% post_url 2026-04-18-Camera-Trap-Classification %}), I built a deployable field tool that a hunter, wildlife manager, or researcher can actually hand their SD card to and get back a sorted folder and a report.

---

## What It Does

Drop a folder of trail camera images in. Get back:

- **Sorted folders** — one per species, plus `low_confidence/`, `empty/`, `person/`, `vehicle/`
- **Summary CSV** — every image with its species, detection confidence, classification confidence, timestamp (from EXIF), and camera location (from subfolder)
- **PDF report** — pie chart of observed entities, count table, and per-camera breakdown if images span multiple locations

---

## Two-Stage Pipeline

Wildlife classification from a full trail cam image is hard — the animal might be a small cluster of pixels in a dense forest scene. The pipeline mimics how a biologist would approach it:

**Stage 1 — MegaDetector** ([Microsoft/PytorchWildlife](https://github.com/microsoft/CameraTraps))
Runs a detector on the full image to find animals, people, and vehicles. If nothing is detected above the confidence threshold, the image goes straight to `empty/` without touching the classifier. For animal detections, it crops the bounding box of the highest-confidence animal.

**Stage 2 — SpeciesNet** ([Google/cameratrapai](https://github.com/google/cameratrapai))
The classifier runs on the crop only — not the full scene. This sidesteps SpeciesNet's internal detector and uses the classifier directly (`model.classifier.preprocess` + `model.classifier.predict`), which is faster and avoids double-detection. Images below the classification confidence threshold go to `low_confidence/`.

The two-stage approach means the classifier always sees a tight crop centered on the animal, dramatically reducing background noise.

---

## Streamlit UI

A local desktop UI built with Streamlit for non-technical users:

- Browse buttons using `tkinter.filedialog` (works on Windows)
- Adjustable detection and classification confidence thresholds
- Live progress bar and per-image status during processing
- Image count preview before running
- Summary metrics (total, empty frames, low confidence) and species table on completion
- One-click CSV download

```bash
streamlit run src/tc_perception/ui/app.py
```

---

## PDF Report

<iframe src="/assets/files/report.pdf" width="100%" height="800px" style="border:none;"></iframe>

---

## Tested on Real Data

Validated on 144 images provided by the Haycock family in Glen Allan (Ontario).

---

## Skills Demonstrated

- **Computer Vision** — two-stage detect-then-classify pipeline, bounding box cropping, confidence thresholding
- **ML Model Integration** — MegaDetector (PyTorch), SpeciesNet classifier API, direct submodule inference
- **Software Engineering** — modular pipeline with progress callbacks, CSV output, EXIF parsing, folder routing
- **Product Thinking** — designed for a non-technical end user: browse buttons, clear defaults, one-click output
- **Data Visualization** — custom single-page matplotlib PDF report with programmatic layout in absolute inches

---

## Repository

[github.com/emilywinfield/trail-camera-perception](https://github.com/emilywinfield/trail-camera-perception)

---

## References

- Beery, S., Van Horn, G., & Perona, P. (2018). Recognition in Terra Incognita. ECCV.
- Gadot T. et al. (2024). To crop or not to crop. IET Computer Vision.
- [PytorchWildlife — Microsoft CameraTraps](https://github.com/microsoft/CameraTraps)
- [SpeciesNet — google/cameratrapai](https://github.com/google/cameratrapai)
