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
