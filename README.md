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
- MotherNet
- TabPFNv2
- GAMformer
- Tabflex

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

##Index Calculation
- **Precision**:

  $$
  Precision = \frac{TP}{TP + FP}
  $$

- **Recall**:

  $$
  Recall = \frac{TP}{TP + FN}
  $$

- **F1-Score**:

  $$
  F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}
  $$

- **Accuracy**:

  $$
  Accuracy = \frac{TP + TN}{TP + TN + FP + FN}
  $$

- **AUC (Area Under the Curve)**:

  AUC is calculated from the ROC curve, which plots the True Positive Rate (TPR) against the False Positive Rate (FPR). It can be expressed as:

  $$
  AUC = \int_{0}^{1} TPR(FPR) \, d(FPR)
  $$

  where:
  - TPR = Recall = $\frac{TP}{TP + FN}$
  - FPR = $\frac{FP}{FP + TN}$



## 🎯 Prediction Output

After training on all valid patient samples for each task, we use the **fusion model (soft-voting VotingClassifier)** to output individual prediction probabilities.

### 📄 Output File

| File                                | Description                                         |
|-------------------------------------|-----------------------------------------------------|
| `fusion_patient_probabilities.xlsx` | Contains the predicted probabilities per patient for each task using the trained fusion model |

### 📑 Columns in Output Excel

| Column             | Description                                                   |
|--------------------|---------------------------------------------------------------|
| `patient`          | Extracted from the third-to-last column of the original Excel |
| `dataset`          | PET or CT                                                     |
| `label`            | Target label: `progression` or `death`                        |
| `true_label`       | Ground truth (0 or 1)                                          |
| `fusion_probability` | Predicted probability of the positive class (label = 1)     |

### 🧪 Example（NOT THE REAL ONE）:

| patient | dataset | label       | true_label | fusion_probability |
|---------|---------|-------------|------------|---------------------|
| P001    | PET     | progression | 1          | 0.84                |
| P001    | PET     | death       | 0          | 0.21                |
| P001    | CT      | progression | 1          | 0.75                |
| P001    | CT      | death       | 0          | 0.32                |

This result can be used for patient-level risk stratification and decision support.

---
## 🛠 Requirements

```bash
pip install pandas matplotlib scikit-learn xgboost openpyxl
pip install pyradiomics SimpleITK nibabel pandas numpy tqdm openpyxl
---
## References

- [TICL (Tabular In-Context Learning) - Microsoft](https://github.com/microsoft/ticl)
- [TabPFN - PriorLabs](https://github.com/PriorLabs/tabpfn)


