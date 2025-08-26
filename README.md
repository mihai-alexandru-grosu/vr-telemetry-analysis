# VRBloons Game Telemetry Analysis

This repository contains all scripts, data, and reproducible analysis code for the clustering and behavioral profiling of player sessions in the VR Balloon Game experiment.

---

## Overview

- **Goal:** Identify and interpret behavioral patterns among child players in a VR game where balloons are popped using hand controllers.
- **Data:** 151 valid gameplay sessions, with detailed input, performance, and movement metrics extracted from raw JSON logs.
- **Main Results:** Four distinct behavioral clusters identified using KMeans on engineered features, confirmed with statistical testing and robustness checks.

---

## Pipeline Steps

1. **Data Filtering:**  
   Remove incomplete/invalid sessions (score < 50 or darts < 10). Export valid data to CSV.

2. **Feature Engineering:**  
   Compute derived metrics (`grip_ratio`, `score_per_throw`, `throws_per_s`, `hand_movement`) and rename/normalize core features.

3. **Preprocessing:**  
   Winsorize features at 1st/99th percentiles; z-score standardize all columns.

4. **Exploratory Analysis:**  
   - Summary statistics, correlation heatmaps, and outlier checks.
   - PCA scree plot (variance explained per component).

5. **Clustering:**  
   - Main features: `grip_ratio`, `accuracy`, `score_per_throw`, `throws_per_s`, `hand_movement`.
   - KMeans (k=2 to 6), silhouette analysis, final k=4 selected.

6. **Cluster Interpretation:**  
   - Centroid analysis, behavioral profiles, statistical validation (Kruskal–Wallis + Dunn).

7. **Robustness Checks:**  
   - Noise sensitivity (ARI after ±5% perturbation).
   - Clustering at alternative k (k=3, 5).

8. **Visualization:**  
   - PCA 2D/3D, t-SNE, cluster distribution plots.

---

## Requirements

The work was conducted using Python 3.10.7
for a full list of requirements to run it see `requirements.txt`

---

## Usage

Clone the repo and run the main analysis notebook:

```bash
git clone <your_repo_url>
cd <repo_name>
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```


