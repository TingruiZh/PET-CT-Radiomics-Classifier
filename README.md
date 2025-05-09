# PET-CT-Radiomics-Classifier
A comprehensive pipeline for multi-model classification of PET/CT radiomics data, including individual classifiers and decision-level fusion via soft voting. ROC curves are used to evaluate performance on both individual models and the ensemble.

---

## 📌 Project Overview

This project performs binary classification (progression and death prediction) based on radiomics features extracted from PET and CT scans. We compare 9 classic machine learning models and one fusion ensemble, using:

- Feature selection
- Train/test split
- Model training and ROC evaluation
- Soft-voting fusion
- Visualization with ROC curves and SHAP values
---

---

## 🧬 Radiomics Feature Extraction

We extract 3D radiomics features from CT and PET scans using [PyRadiomics](https://github.com/AIM-Harvard/pyradiomics). Each scan is paired with a patient-specific binary or labeled segmentation mask.

### 🗂 Input Folders

- `/H/ROI/` – contains patient masks in `.nii` format
- `/H/converted_nii/` – contains CT and PET images in NIfTI format (`_CT`, `_PET` suffix)
- `tumor.yaml` – PyRadiomics parameter config file

### ⚙️ Process Overview

- For each patient in `ROI`:
  - Extracts patient name from folder
  - Matches corresponding CT and PET folder using name fragment
  - Loads images using `SimpleITK`, falls back to `nibabel` if needed
  - Resamples CT and PET images to match the mask resolution
  - Uses PyRadiomics to extract features (with a YAML config)
  - Separately stores CT and PET features

### 📁 Output Files

| File | Description |
|------|-------------|
| `radiomics_features_all.xlsx` | Excel file with `CT` and `PET` sheets for extracted features |
| `failed_cases_all.csv`        | Log of patients with missing or invalid data |


---

## 🧪 Classification Models Used

- Random Forest  
- Logistic Regression  
- SVM (RBF Kernel)  
- Decision Tree  
- K-Nearest Neighbors  
- Gaussian Naive Bayes  
- Gradient Boosting  
- AdaBoost  
- XGBoost  
- **Fusion Model (Soft Voting)**

---

## 📊 Tasks

Four classification tasks are implemented:

- PET - Progression Prediction  
- PET - Death Prediction  
- CT - Progression Prediction  
- CT - Death Prediction  

Each task uses 10 selected radiomics features.

---

## 📈 Visualizations

- ROC Curve per task (train set)  
- AUC values annotated per model  
- Fusion model included in each plot  
- Optional SHAP value plots (coming soon)

---

## 🛠 Requirements

```bash
pip install pandas matplotlib scikit-learn xgboost openpyxl
pip install pyradiomics SimpleITK nibabel pandas numpy tqdm openpyxl

