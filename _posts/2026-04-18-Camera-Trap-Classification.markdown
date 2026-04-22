---
layout: post
title: 🦝 Camera Trap Perception
date: 2026-04-18
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: trail-cam-raccoon.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
---

A data engineering and computer vision pipeline for automated wildlife monitoring using camera trap imagery. Built on the [Caltech Camera Traps (CCT)](https://lila.science/datasets/caltech-camera-traps) benchmark dataset, with a focus on **domain shift** — how model performance changes when deployed to camera locations not seen during training.



---

## Project Overview

Camera traps generate thousands of images per season. This project automates the analysis pipeline:

- **Parses** raw COCO-style annotation JSON files into a structured SQLite database
- **Analyzes** species distributions and domain shift patterns with SQL
- **Runs** [SpeciesNet](https://github.com/google/cameratrapai) (Google's pretrained wildlife classifier) as a zero-shot baseline
- **Evaluates** prediction accuracy across cis (same-location) and trans (new-location) splits
- **Visualizes** everything in an interactive Streamlit dashboard

---

## Key Finding

SpeciesNet, trained on 65M images globally, shows **strong robustness to domain shift** — accuracy drops only ~5–8 percentage points on completely new camera locations (88% trans vs 91% cis). This contrasts with models trained only on local data, which typically degrade more sharply on unseen locations.

---

## Dataset

**Caltech Camera Traps (CCT) — ECCV 2018 Benchmark Subset**

| Split | Images | Annotations | Locations |
|---|---|---|---|
| train | 13,553 | 14,071 | 10 |
| cis_val | 3,484 | 3,582 | 10 (same as train) |
| cis_test | 15,827 | 16,395 | 10 (same as train) |
| trans_val | 1,725 | 1,865 | 1 (new) |
| trans_test | 23,275 | 24,028 | 9 (new) |

16 species including opossum, raccoon, bobcat, coyote, rabbit, squirrel, and empty frames.

Download from [LILA BC](https://lila.science/datasets/caltech-camera-traps).

---

## Project Structure

```
trail-cam-perception/
├── data/
│   ├── raw/
│   │   └── CCT/                  # Downloaded dataset (not tracked in git)
│   └── processed/
│       └── wildlife.db           # SQLite database (built by pipeline)
├── notebooks/
│   ├── 01_exploration.ipynb      # Image visualization and dataset inspection
│   ├── 02_build_database.ipynb   # ETL: JSON → SQLite
│   └── 03_sql_analysis.ipynb     # SQL queries and exploratory charts
│   └── 04_speciesnet.ipynb       # Speciesnet Evaluation
├── src/tc_perception/
│   ├── config.py                 # Centralized path configuration
│   ├── data/
│   │   ├── preprocessing.py      # CCT JSON parser → DataFrames
│   │   └── database.py           # SQLite schema, safe inserts, prediction loader
│   └── dashboard/
│       └── app.py                # Streamlit dashboard
└── requirements.txt
```

---

## Database Schema

```sql
images       (id, file_name, location, datetime, split)
categories   (id, name)
annotations  (id, image_id, category_id, bbox_x, bbox_y, bbox_w, bbox_h, split)
predictions  (id, image_id, filename, detection_conf, bbox_x, bbox_y, bbox_w,
              bbox_h, predicted_class, taxonomy, source)
```

`predictions` links back to `images` via `image_id`, enabling direct SQL joins between ground truth and model output.

---

## Getting Started

### 1. Clone and set up environment

```bash
git clone https://github.com/YOUR_USERNAME/trail-cam-perception.git
cd trail-cam-perception
python -m venv .venv
.venv\Scripts\activate       # Windows
pip install -r requirements.txt
```

### 2. Download the dataset

Download the CCT ECCV 2018 benchmark subset from [LILA BC](https://lila.science/datasets/caltech-camera-traps) and place it at:
```
data/raw/CCT/
```

### 3. Build the database

Open and run `notebooks/02_build_database.ipynb`. This parses all 5 annotation splits and loads ~57k images and ~60k annotations into `data/processed/wildlife.db`.

### 4. Run SpeciesNet inference (optional)

```bash
python -m speciesnet.scripts.run_model \
  --folders data/raw/CCT/images/stratified_sample \
  --predictions_json data/processed/speciesnet_sample.json \
  --nogeofence
```

Then load predictions into the database via `notebooks/03_sql_analysis.ipynb`.

### 5. Launch the dashboard

```bash
streamlit run src/tc_perception/dashboard/app.py
```

---

## Dashboard Pages

**Dataset Overview** — species distribution, empty frame rates, split statistics

<p float="left">
  <img src="/assets/img/species_distribution.png" width="45%" />
  <img src="/assets/img/empty_frame_rate.png" width="45%" />
</p>

**Domain Shift Analysis** — species proportion heatmap across cis and trans splits, highlighting distribution differences between training and transfer locations

<img src="{{site.baseurl}}/assets/img/domain_shift_heatmap.png" style="width:75%;display:block;margin:0 auto;">

**SpeciesNet Results** — accuracy by split, per-species accuracy, interactive prediction browser

<p float="left">
  <img src="/assets/img/speciesnet_accuracy_by_split.png" width="45%" />
  <img src="/assets/img/speciesnet_species_accuracy.png" width="45%" />
</p>

---

## Skills Demonstrated

- **Data Engineering** — ETL pipeline, SQLite schema design, safe bulk inserts, config-driven paths
- **SQL** — multi-table joins, window functions, CASE expressions, aggregations
- **Python** — modular package structure, Pandas, Pathlib, error handling
- **Machine Learning** — pretrained model inference, stratified sampling, label normalization, accuracy evaluation
- **Data Visualization** — Matplotlib, Seaborn, Streamlit interactive dashboard

---

## Next Steps

- [ ] Train YOLOv8 detector on CCT training split
- [ ] Evaluate trained model on cis vs trans test splits
- [ ] Compare trained model accuracy against SpeciesNet baseline
- [ ] Write model predictions back to database for unified dashboard view
- [ ] Power BI report for additional data analytics showcase

---

## References

- Beery, S., Van Horn, G., & Perona, P. (2018). Recognition in Terra Incognita. ECCV.
- Gadot T. et al. (2024). To crop or not to crop. IET Computer Vision.
- [LILA BC — Camera Trap Datasets](https://lila.science)
- [SpeciesNet — google/cameratrapai](https://github.com/google/cameratrapai)
