# Acoustic Structure of Green-Cheeked Conure Vocalizations
## An Unsupervised Machine Learning Approach

* **Species:** *Pyrrhura molinae* (Green-cheeked Conure / Green-cheeked Parakeet)
* **Methods:** Bioacoustics · Signal Processing · K-Means Clustering · PCA
* **Tools:** Python · librosa · scikit-learn · pandas · matplotlib · seaborn
* **Data:** Xeno-canto — Grade A recordings only

---

## Overview
This project applies unsupervised machine learning to the vocalizations of *Pyrrhura molinae*, one of the most widely kept companion parrots in the world — and one of the least studied in the bioacoustics literature. No published machine learning-based vocalization classification exists for this species. This work establishes the first acoustic taxonomy of *P. molinae* vocalizations, laying the groundwork for future behavioral annotation and communication studies.

The project was developed as a portfolio submission for a Master's application in Data Science & Artificial Intelligence at **Université Côte d'Azur (UCA), Sophia Antipolis**.

---

## Motivation
Research on bottlenose dolphins and great apes has shown that acoustic structure — even without full semantic mapping — can carry meaningful communicative information. Psittacines (parrots) are among the few non-human taxa capable of vocal learning, making them uniquely compelling subjects for such investigation.

The author keeps green-cheeked conures personally. Direct behavioral observation of vocalization patterns in social and solitary contexts informed the design of this study and provided qualitative hypotheses that guided interpretation of results.

---

## Dataset

| Property | Detail |
| :--- | :--- |
| **Source** | Xeno-canto (xeno-canto.org) |
| **Quality Filter** | Grade A recordings only |
| **Recordings** | 27 files |
| **Segments** | 254 (3.0s fixed-length windows) |
| **Species** | *Pyrrhura molinae* |

> **Note:** Only Grade A recordings were used. Including lower-quality recordings (Grades B–E) substantially degraded cluster stability — a finding with practical implications for future bioacoustics work using citizen science databases.

---

## Methodology

### Feature Extraction
Each 3-second segment was represented as a 34-dimensional acoustic feature vector using `librosa`:

| Feature | Dimensions | What it captures |
| :--- | :---: | :--- |
| **MFCCs** | 20 | Timbral texture and spectral envelope — the acoustic "fingerprint" |
| **Chroma** | 12 | Pitch class energy — distinguishes tonal from noisy vocalizations |
| **Spectral Centroid** | 1 | Perceived brightness — high values = sharp, high-frequency calls |
| **Spectral Contrast** | 1 | Peak-to-valley energy — distinguishes clear calls from diffuse noise |

*All features were standardized using `StandardScaler` prior to clustering.*

### Clustering
K-Means clustering was applied across $k = 2$ to $k = 9$. Optimal $k$ was determined using two complementary methods:
* **Elbow Method** — minimizing within-cluster inertia
* **Silhouette Analysis** — maximizing mean silhouette coefficient

Both methods converged on **$k = 6$** (silhouette score: 0.2595). PCA (2 components) was used for visualization.

---

## Key Results

* **6 acoustically distinct vocalization clusters** identified.
* **Cluster 2** shows consistent spatial isolation along PC1 across multiple random initializations — indicating a structurally unique vocalization type, not an algorithmic artifact.
* **Qualitative listening** confirmed Cluster 2 segments have a sharper, more tonally focused character compared to other clusters.
* **Clusters 0, 1, 4, and 5** occupy overlapping PCA space — consistent with a graded acoustic continuum rather than discrete categorical calls.
* **Average Mel spectrograms** reveal distinct energy profiles per cluster (high-frequency concentration, harmonic banding, rhythmic vertical banding, diffuse low-amplitude spread).

The moderate silhouette scores (0.22–0.26) are consistent with comparable bioacoustics research — animal vocalizations exist on a continuous spectrum, and unsupervised methods on naturalistic recordings routinely produce scores in this range.

---

## Technical Summary

| Component | Detail |
| :--- | :--- |
| **Language** | Python 3 (Google Colab) |
| **Audio Processing** | librosa, soundfile |
| **Machine Learning** | scikit-learn (`KMeans`, `PCA`, `StandardScaler`, `silhouette_score`) |
| **Data Handling** | pandas, numpy |
| **Visualization** | matplotlib, seaborn |
| **Dataset** | 27 recordings, 254 segments (3.0s each) |
| **Feature Vector** | 34 dimensions |
| **Algorithm** | K-Means ($k=6$, $n\_init=10$, $random\_state=42$) |
| **Final Score** | Silhouette = 0.2595 at $k=6$ |

---

## Repository Structure

```text
conure-vocalization-clustering/
│
├── conure_vocalization_clustering.ipynb   # Full analysis notebook
├── Conure_Vocalization_Report.pdf         # Project report
└── README.md                              # This file
