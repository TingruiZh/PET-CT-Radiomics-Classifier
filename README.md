# PET-CT-Radiomics-Classifier

A comprehensive pipeline for multi-model classification of PET/CT radiomics data, including individual classifiers and decision-level fusion via soft voting. ROC curves are used to evaluate performance on both individual models and the ensemble.

---

## 📌 Project Overview

This project performs binary classification (progression and death prediction) based on radiomics features extracted from PET and CT scans. We compare **9 classic machine learning models**, **4 advanced tabular models**, and a **fusion ensemble**. The pipeline includes:

- Feature selection
- Train/test split
- Model training and ROC evaluation
- Soft-voting fusion
- Visualization with ROC curves and SHAP values

---

## 🧬 Radiomics Feature Extraction

We extract 3D radiomics features from CT and PET scans using [PyRadiomics](https://github.com/AIM-Harvard/pyradiomics). Each scan is paired with a patient-specific binary or labeled segmentation mask.

### 🗂 Input Folders

- `/H/ROI/` — contains patient masks in `.nii` format
- `/H/converted_nii/` — contains CT and PET images in NIfTI format (suffix `_CT`, `_PET`)
- `tumor.yaml` — PyRadiomics parameter configuration file

### ⚙️ Process Overview

For each patient:
- Extract patient name from the folder
- Match corresponding CT and PET images using the name fragment
- Load images using `SimpleITK` (fallback to `nibabel` if needed)
- Resample CT and PET images to align with the mask resolution
- Extract features using PyRadiomics with YAML configuration
- Save features separately for CT and PET

### 📁 Output Files

| File                        | Description                                       |
|-----------------------------|---------------------------------------------------|
| `radiomics_features_all.xlsx` | Excel file with `CT` and `PET` feature sheets     |
| `failed_cases_all.csv`      | Log of patients with missing or invalid data      |

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
- MotherNet
- TabPFNv2
- GAMformer
- Tabflex

---

## 📊 Tasks

Four classification tasks are implemented:

- PET — Progression Prediction
- PET — Death Prediction
- CT — Progression Prediction
- CT — Death Prediction

Each task uses **10 selected radiomics features**.

---

## 📈 Visualizations

- ROC curves per task (train set)
- Annotated AUC values per model
- Fusion model included in each plot
- Optional SHAP value plots (coming soon)

---

## 📐 Index Calculation

- **Precision**:  
  `Precision = TP / (TP + FP)`

- **Recall**:  
  `Recall = TP / (TP + FN)`

- **F1-Score**:  
  `F1 = 2 * (Precision * Recall) / (Precision + Recall)`

- **Accuracy**:  
  `Accuracy = (TP + TN) / (TP + TN + FP + FN)`

- **AUC (Area Under the Curve)**:  
  AUC is calculated from the ROC curve, which plots the True Positive Rate (TPR) against the False Positive Rate (FPR).  
  - TPR (Recall) = `TP / (TP + FN)`
  - FPR = `FP / (FP + TN)`

---

## 🎯 Prediction Output

After training on all valid patient samples for each task, the **fusion model (soft-voting VotingClassifier)** outputs individual prediction probabilities.

### 📄 Output File

| File                                  | Description                                            |
|---------------------------------------|--------------------------------------------------------|
| `fusion_patient_probabilities.xlsx`   | Predicted probabilities per patient using the fusion model |

### 📑 Columns in Output Excel

| Column              | Description                                                   |
|---------------------|---------------------------------------------------------------|
| `patient`           | Patient ID extracted from the Excel sheet                      |
| `dataset`           | PET or CT                                                      |
| `label`             | Target label: `progression` or `death`                         |
| `true_label`        | Ground truth (0 or 1)                                          |
| `fusion_probability` | Predicted probability of the positive class (label = 1)       |

#### Example (NOT THE REAL ONE):

| patient | dataset | label       | true_label | fusion_probability |
|---------|---------|-------------|------------|---------------------|
| P001    | PET     | progression | 1          | 0.84                |
| P001    | PET     | death       | 0          | 0.21                |
| P001    | CT      | progression | 1          | 0.75                |
| P001    | CT      | death       | 0          | 0.32                |

This result can be used for **patient-level risk stratification** and **decision support**.

---

## 📚 References

- [TICL (Tabular In-Context Learning) - Microsoft](https://github.com/microsoft/ticl)
- [TabPFN - PriorLabs](https://github.com/PriorLabs/tabpfn)
- [PyRadiomics - AIM-Harvard](https://github.com/AIM-Harvard/pyradiomics)

---

## 🛠 Requirements

```bash
pip install pandas matplotlib scikit-learn xgboost openpyxl
pip install pyradiomics SimpleITK nibabel numpy tqdm
