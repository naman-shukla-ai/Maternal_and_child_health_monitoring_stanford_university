# Maternal and Child Health Monitoring

Predicting six maternal and child health indicators for geographic DHS survey clusters across low- and middle-income countries (LMICs) using high-dimensional satellite and remote-sensing features.

This project was developed in collaboration with **Stanford University** as part of the Kaggle competition:

> **[Maternal and Child Health Monitoring in LMICs](https://www.kaggle.com/competitions/maternal-and-child-health-monitoring-in-lmics/overview)**

---

## Problem Description

Monitoring maternal and child health in LMICs is challenging due to the absence of comprehensive vital registration systems and infrequent household surveys. This project leverages satellite-derived geospatial features (from Google Earth Engine and DHS survey data) to predict community-level health outcomes, enabling scalable, low-cost monitoring.

---

## Targets (Health Indicators)

The model predicts six health indicators per DHS survey cluster (`DHSID`):

| Column | Description |
|--------|-------------|
| `Mean_BMI` | Mean Body Mass Index of women |
| `Median_BMI` | Median BMI of women |
| `Unmet_Need_Rate` | Proportion of women with unmet need for family planning |
| `Under5_Mortality_Rate` | Under-5 child mortality rate |
| `Skilled_Birth_Attendant_Rate` | Proportion of births attended by skilled personnel |
| `Stunted_Rate` | Proportion of children who are stunted |

---

## Dataset

The dataset is provided through the Kaggle competition page. It is **not** included in this repository.

| File | Description | Dimensions |
|------|-------------|------------|
| `x_features.parquet` | Feature matrix (satellite/GEE features) | 37,535 rows x 11,379 columns |
| `y_labels.parquet` | Label matrix (health indicators + DHSID) | 37,535 rows x 7 columns |

**To obtain the data:**
1. Join the competition at the link above.
2. Download the dataset files.
3. Place them anywhere accessible on your machine or Google Drive.
4. Set `X_PATH` and `Y_PATH` in the Configuration cell of `train.ipynb` (see Setup below).

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/Maternal_and_child_health_monitoring_stanford_university.git
cd Maternal_and_child_health_monitoring_stanford_university
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure dataset paths

Open `train.ipynb` and find the **Configuration** cell near the top.
Set the two path variables to point to your downloaded dataset files:

```python
X_PATH = '/path/to/x_features.parquet'
Y_PATH = '/path/to/y_labels.parquet'
```

**If running in Google Colab**, mount your Drive first, then set the paths
to their location within your Drive folder.

### 4. Run the notebook

```bash
jupyter notebook train.ipynb
```

Or open it directly in Google Colab / VS Code.

---

## Model Architecture

| Component | Details |
|-----------|---------|
| **Algorithm** | XGBoost Regression (`XGBRegressor`) |
| **Dimensionality reduction** | PCA to 500 principal components |
| **Input features** | 11,378 numeric columns (after dropping `DHSID`) |
| **Targets** | 6 separate models, one per health indicator |
| **NaN imputation** | Column mean, then zero for all-NaN columns |

### Hyperparameters

| Parameter | Value |
|-----------|-------|
| `n_estimators` | 2000 |
| `early_stopping_rounds` (Experiment 1) | 750 |
| `early_stopping_rounds` (Experiment 2) | 500 |
| `PCA n_components` | 500 |
| Train/test split (Experiment 1) | 70% / 30% |
| Train/test split (Experiment 2) | 75% / 25% |

All other XGBoost parameters use library defaults.

---

## Results

### Experiment 1 — Mean BMI only (70/30 split)

| Metric | Value |
|--------|-------|
| Test MSE | ~6.16 |

*Full per-round RMSE logs are available by re-running the notebook with `verbose=True`.*

### Experiment 2 — All 6 targets (75/25 split)

*(Training was interrupted; full results will appear here after a complete run.)*

---

## Environment

Originally developed in **Google Colab** with a T4 GPU.
GPU acceleration is recommended for the full 2000-estimator runs.
The notebook also runs in local Jupyter with a CPU, though training will be slower.

- Python 3.10
- See `requirements.txt` for full dependency list.

---

## Repository Structure

```
.
├── train.ipynb        # Main training notebook
├── requirements.txt   # Python dependencies
├── .gitignore         # Excludes data, models, and checkpoints
└── README.md          # This file
```

---

## License

Please check the Kaggle competition rules regarding data usage and any
applicable license constraints before publishing derived work.
