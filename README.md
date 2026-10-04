# Heart Disease Clustering

Unsupervised machine-learning project that groups patients from the UCI Heart Disease dataset using K-Means, Agglomerative Clustering, and DBSCAN.

## Project overview

The objective is to discover patient groups from clinical measurements without using the `target` diagnosis during model training. The target is retained only for post-clustering comparison and interpretation.

### Dataset
- 920 records
- 14 columns: 13 clinical features + `target`
- 2 duplicate rows removed during preprocessing
- 918 unique records used for clustering
- Target is excluded from clustering features

## Preprocessing pipeline

1. Load the 920-row dataset.
2. Remove duplicate rows.
3. Separate the `target` column from clustering features.
4. Handle missing values.
5. Cap continuous-feature outliers using the IQR rule.
6. Apply `VarianceThreshold`.
7. Standardize features using `StandardScaler`.
8. Apply PCA while retaining at least 90% cumulative variance.
9. Cluster the transformed data with three algorithms.

The corrected pipeline retains all 13 input features after variance filtering and uses 11 PCA components to retain approximately 92.76% of the variance.

## Models

### K-Means
K-Means was evaluated across multiple values of `k`. The selected solution uses `k=3`.

### Agglomerative Clustering
Hierarchical agglomerative clustering was evaluated with `k=3` for comparison with K-Means.

### DBSCAN
DBSCAN was tuned using the PCA-transformed feature space. The selected demonstration setting is `eps=2.8`, producing 3 clusters and 41 noise points (about 4.5%).

## Evaluation

| Algorithm | Clusters | Silhouette | Davies-Bouldin | Noise |
|---|---:|---:|---:|---:|
| K-Means (k=3) | 3 | 0.168 | 1.916 | 0 |
| Agglomerative (k=3) | 3 | 0.130 | 2.277 | 0 |
| DBSCAN (eps=2.8) | 3 | 0.272 | 1.205 | 41 |

Higher Silhouette is better. Lower Davies-Bouldin is better.

DBSCAN has the strongest internal validation scores in this run, but it also produces noise points and less balanced groups. K-Means provides the most straightforward three-group solution for the assignment's patient-profile interpretation.

## K-Means cluster interpretation

Based on post-clustering profile statistics:

- **Cluster 0 — lower observed severity:** 508 patients; lower average disease-severity value, higher average maximum heart rate, lower exercise-induced angina prevalence, and lower average ST depression.
- **Cluster 2 — intermediate observed severity:** 46 patients; clinical measurements generally fall between the other two groups.
- **Cluster 1 — higher observed severity:** 364 patients; higher average disease-severity value, lower average maximum heart rate, higher exercise-induced angina prevalence, and higher average ST depression.

These are **descriptive cluster interpretations, not clinical diagnoses**.

## Repository structure

```text
heart-disease-clustering/
├── clustering_assignment.ipynb
├── Heart Disease.csv
├── ML_Clustering_Report.docx
├── README.md
├── requirements.txt
├── .gitignore
└── results/
    └── clustering_results.csv
```

## How to run

Create a virtual environment, install the dependencies, then open the notebook:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Install packages:

```bash
pip install -r requirements.txt
```

Start Jupyter:

```bash
jupyter notebook
```

Open `clustering_assignment.ipynb` and use **Kernel → Restart & Run All** for a clean reproducible run.

## Important note

This project is for academic machine-learning analysis. The discovered clusters should not be interpreted as medically validated risk categories or used for clinical decision-making.

## Suggested GitHub commands

```bash
git init
git add .
git commit -m "Initial commit: heart disease clustering project"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/heart-disease-clustering.git
git push -u origin main
```
