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

## Telemetry Schema

Each VRBloons session produces a single JSON entry with summary-level metrics logged at runtime.  
These values are recorded automatically at the end of a play session.

### Session-Level Fields
- `playerId` *(string, GUID)* — Randomly generated unique identifier for each session.  
- `timestamp` *(ISO 8601 string)* — UTC timestamp marking the start of the session.  
- `score` *(int)* — Final total score earned by the player from all popped balloons.  
- `dartsFired` *(int)* — Total number of darts thrown during the sessions.  
- `balloonsPopped` *(int)* — Total number of balloons successfully hit by the player.  
- `accuracy` *(float, 0–1)* — Hit accuracy calculated as
**balloonsPopped** / **dartsFired**
.  

### Input Metrics
- `buttonPresses` *(array)* — Number of presses per input action:  
  - `actionName` *(string)* — e.g., `"trigger"`, `"grip"`.  
  - `count` *(int)* — number of presses.  

### Movement Metrics
- `headMovement` *(float)* — Total movement distance of the HMD during gameplay.  
- `leftControllerMovement` *(float)* — Total movement distance of the left controller during the session (meters).  
- `rightControllerMovement` *(float)* — Total movement distance of the right controller during the session.  
- `lookAroundYawRange` *(float)* — Total yaw rotation range (left and right) in degrees, capped at 360°.  
- `lookAroundPitchRange` *(float)* — Total pitch rotation range (up and down) in degrees, capped at 180°.  

### Example
```json
{
  "playerId": "2f2b5eaf-497e-49bc-b2eb-0a51dfc2e3c9",
  "timestamp": "2025-05-28T10:41:51.7812250Z",
  "score": 5495,
  "dartsFired": 243,
  "balloonsPopped": 182,
  "accuracy": 0.7489,
  "buttonPresses": [
    {"actionName": "trigger", "count": 285},
    {"actionName": "grip", "count": 26}
  ],
  "headMovement": 11.42,
  "leftControllerMovement": 1.85,
  "rightControllerMovement": 59.64,
  "lookAroundYawRange": 359.77,
  "lookAroundPitchRange": 42.42
}
