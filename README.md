# Synthetic Sample Generation via Tetrahedral Density

This repository implements a novel Python pipeline to generate synthetic data for imbalanced binary classification by constructing tetrahedrons around positive samples in PCA space and sampling based on local density.

---

## 📋 Project Structure

```
synthetic-tetrahedron-sampler/
├── data/                      # Example datasets (not included)
│   └── newdata.csv            # Example CSV with feature columns + target 'Class'
├── tetra_sampler.py           # Main script containing data loading, tetrahedron logic, sampling, and visualization
└── README.md                  # This file
```

---

## ⚙️ Requirements

* Python 3.7+
* numpy
* pandas
* scikit-learn
* matplotlib

Install via pip:

```bash
pip install numpy pandas scikit-learn matplotlib
```

---

## 🚀 Usage

1. **Prepare your dataset**: a CSV with features and a binary target column (e.g. `Class` where positive class = 1).
2. **Edit file paths** in `tetra_sampler.py` (or pass via command line):

   ```python
   file_path = r"D:\data\newdata.csv"
   target_column = 'Class'
   ```
3. **Run the script**:

   ```bash
   python tetra_sampler.py
   ```
4. **View output**:

   * Console logs of dataset shape, synthetic sample counts, and success/warning.
   * A 3D PCA scatter plot showing negative samples (blue), original positives (red), and generated synthetic points (black).

---

## 🛠️ Key Components

* **Data Loading & Preprocessing** (`load_dataset`)

  * Reads CSV, verifies target column
  * One-hot encodes categorical features
  * Imputes missing values with feature means
  * Returns feature matrix `X` and binary label vector `y`

* **Tetrahedron Construction** (`construct_tetrahedron`)

  * For each positive point in PCA space, finds two negative neighbors (1st & 3rd nearest)
  * Computes centroid, finds nearest positive neighbor
  * Builds a 4-vertex tetrahedron

* **Density & Volume**

  * `compute_tetrahedron_volume`: exact volume via determinant
  * `density`: ratio of positive points inside tetrahedron to its volume

* **Synthetic Sample Generation**

  * Distributes synthetic sample counts across tetrahedrons proportional to density
  * For each, generates points via random convex combinations on triangular faces (`generate_synthetic_samples_with_triangles`)

* **Visualization** (`visualize_data`)

  * Projects data into 3D via PCA
  * Scatter-plot negative, positive, and synthetic points

---

## 🔧 Customization

* **PCA dimensions**: adjust `PCA(n_components=3)` for different projections
* **Neighbor counts**: change `n_neighbors` in `NearestNeighbors` calls
* **Imputation strategy**: swap `SimpleImputer(strategy='mean')` for median or most-frequent
* **Sampling rules**: modify how synthetic counts are allocated or how triangles are formed
* **Plot appearance**: tweak `elev`/`azim` angles and markers in `visualize_data`

---

Happy synthesizing! 🎲
