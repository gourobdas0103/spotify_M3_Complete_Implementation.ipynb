# M3 Spotify Music Clustering - Complete Pipeline

**Author:** Gourob Das

## Project Overview
This project implements an end-to-end unsupervised machine learning pipeline to analyze and cluster a dataset of Spotify tracks. The primary objective is to resolve injected data quality issues, identify underlying audio profiles using K-Means clustering, and detect anomalous tracks that deviate from standard patterns.

## Data Pipeline & Methodology

### 1. Data Cleaning and Preprocessing
* **Outlier Mitigation:** Addressed severe skewness in the `acoustic_electronic_ratio` by clipping extreme values at the 99th percentile and applying a log transformation (`ratio_log`).
* **Deduplication:** Removed multi-genre duplicate entries using unique `track_id`s, reducing the working dataset to 8,947 unique tracks.
* **Normalization:** Standardized track durations from milliseconds to seconds and scaled all numeric audio features using `StandardScaler` for distance-based algorithms.

### 2. Clustering Analysis
* **Algorithm:** K-Means clustering was applied to the standardized feature set.
* **Validation:** Evaluated cluster cohesion and separation using Silhouette and Davies-Bouldin scores across multiple values of *k*.
* **Optimal Selection:** Two distinct clusters (*k=2*) were identified as the optimal structure, achieving a Silhouette Score of approximately 0.177.
    * **Cluster 0:** Characterized by higher energy and a lower acoustic-electronic log ratio.
    * **Cluster 1:** Characterized by very low energy and a highly elevated log ratio.

### 3. Dimensionality Reduction & Feature Importance
* **PCA:** Principal Component Analysis was utilized to reduce the 9-dimensional feature space into 2 components (capturing 30.6% of the variance) for 2D visualization.
* **Decision Tree Analysis:** A shallow Decision Tree classifier (max depth of 4) was trained on the cluster labels to extract feature importance, confirming that `energy` and `ratio_log` are the primary drivers of cluster separation (yielding 99.8% classification accuracy).

### 4. Anomaly Detection
* **Local Outlier Factor (LOF):** Deployed LOF with a 10% contamination rate to detect local density anomalies.
* **Results:** Successfully isolated 895 anomalous tracks that exhibited highly irregular energy-to-acoustic profiles compared to their immediate local neighborhoods.

## Repository Contents
* `M3 Complete Implementation.ipynb`: The main Jupyter Notebook containing the end-to-end Python implementation and visualizations.
* `spotify_complete.csv`: The final, cleaned, and preprocessed dataset exported from the notebook.
* `spotify_results.png`: A composite grid of visualizations showcasing the data cleaning impacts, validation metrics, cluster scatter plots, and anomaly distributions.

## Dependencies
To run the notebook, ensure the following Python libraries are installed:
* `pandas`
* `numpy`
* `matplotlib`
* `seaborn`
* `scikit-learn`

## Usage
Run the Jupyter Notebook sequentially from top to bottom. The notebook will automatically generate the simulated dataset, perform the cleaning steps, execute the clustering and anomaly detection algorithms, and export the final CSV and visualization PNG to your local directory.
