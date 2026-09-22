# Hospital 30-Day Readmission Risk Prediction

A healthcare predictive analytics project that uses Python and Logistic Regression to predict whether a patient is likely to be readmitted to the hospital within 30 days of discharge.

## Project Overview

Hospital readmissions are an important healthcare analytics problem because identifying patients at elevated risk can help healthcare organizations prioritize post-discharge follow-up and care-management resources.

This project develops an interpretable machine-learning model for 30-day readmission risk prediction using clinical, utilization, comorbidity, laboratory, admission, and discharge-related variables.

The project follows an end-to-end healthcare analytics lifecycle:

Business Problem
→ Data Understanding
→ Exploratory Data Analysis
→ Data Preprocessing
→ Feature Engineering
→ Model Development
→ Class Imbalance Handling
→ Threshold Optimization
→ Model Evaluation
→ Model Interpretation
→ Healthcare Business Application

## Business Problem

Healthcare organizations need to identify patients who may be at elevated risk of returning to the hospital shortly after discharge.

The objective is to develop a risk prediction model that can help identify higher-risk patients who may benefit from additional post-discharge follow-up, care coordination, medication review, or transitional-care interventions.

## Objective

Build and evaluate a binary classification model that predicts:

- 0 = No readmission within 30 days
- 1 = Readmission within 30 days

The project focuses on model discrimination, sensitivity, specificity, precision, F1-score, ROC-AUC, and threshold selection.

## Dataset

The project uses a synthetic clinical dataset created for educational and portfolio purposes.

Dataset characteristics:

- 2,000 patient records
- 22 original variables
- Binary target variable: `readmission_30d`
- 1,536 patients not readmitted
- 464 patients readmitted
- 23.2% 30-day readmission prevalence

The synthetic dataset includes variables representing:

- Patient demographics
- Admission characteristics
- Discharge disposition
- Previous healthcare utilization
- Chronic disease burden
- Heart failure
- COPD
- CKD
- Laboratory measurements
- Follow-up scheduling
- Medication burden

The dataset does not contain real patient records or personally identifiable information.

## Key Analytical Challenges

### 1. Class Imbalance

Only 23.2% of patients in the dataset were readmitted within 30 days.

A naive model predicting every patient as "not readmitted" would achieve approximately 76.8% accuracy while identifying none of the true readmissions.

Therefore, accuracy alone is not an appropriate headline metric.

### 2. Missing Clinical Data

Several laboratory variables contain missing values.

The project uses a leakage-safe approach:

- Missingness indicators are created before imputation.
- Imputation statistics are calculated using the training data only.
- Clinically meaningful grouping is used for selected variables.
- The validation and test sets are not used to calculate training imputation statistics.

### 3. Threshold Selection

The default classification threshold of 0.50 is not automatically treated as the optimal operating point.

The project evaluates multiple probability thresholds and selects an operating threshold using Youden's J statistic on the validation set.

This allows the model to explicitly control the sensitivity-specificity trade-off.

## Methodology

### 1. Exploratory Data Analysis

The analysis examines:

- Target-class distribution
- Previous admissions
- Comorbidity burden
- Discharge disposition
- Missing-value patterns
- Relationships between selected clinical variables and readmission

### 2. Train / Validation / Test Split

The dataset is divided using a stratified:

- 60% training set
- 20% validation set
- 20% test set

Stratification preserves approximately the same readmission prevalence across the datasets.

### 3. Data Preprocessing

The workflow includes:

- Categorical variable encoding
- Missingness indicators
- Clinically informed median imputation
- Feature scaling
- Leakage prevention

### 4. Feature Engineering

Healthcare-oriented features are created to capture clinically relevant risk patterns, including:

- High-utilizer flag
- Polypharmacy flag
- Reduced ejection fraction flag
- Elevated BNP flag
- No-follow-up flag
- Chronic disease burden

### 5. Model

The primary predictive model is:

**Logistic Regression**

Logistic Regression was selected because the outcome is binary and the model provides interpretable coefficients that can be transformed into odds ratios.

### 6. Class Imbalance

The project compares:

- No class adjustment
- `class_weight="balanced"`
- SMOTE

The final primary model uses:

`class_weight="balanced"`

SMOTE is also evaluated as an alternative imbalance-handling strategy.

### 7. Cross-Validation

Model development uses:

**5-fold Stratified Cross-Validation**

ROC-AUC is used as the primary threshold-independent model comparison metric.

### 8. Threshold Optimization

The validation set is used to evaluate multiple probability thresholds.

The final operating threshold is selected using Youden's J statistic.

Final threshold:

**0.406**

The test set remains untouched during threshold selection.

## Model Performance

The final Logistic Regression model achieved the following results on the held-out test set:

| Metric | Result |
|---|---:|
| ROC-AUC | 0.808 |
| Accuracy | 0.672 |
| Sensitivity / Recall | 0.817 |
| Specificity | 0.629 |
| Precision | 0.400 |
| F1-score | 0.537 |
| Decision Threshold | 0.406 |

The threshold was selected using the validation set and then applied to the held-out test set.

## Model Interpretation

Logistic Regression coefficients are converted into odds ratios to improve interpretability.

Interpretation:

- Odds Ratio > 1: associated with higher odds of predicted readmission
- Odds Ratio < 1: associated with lower odds of predicted readmission
- Odds Ratio = 1: no change in odds associated with the feature

Features evaluated include utilization history, comorbidity burden, discharge-related factors, follow-up status, and clinical indicators.

Odds ratios are interpreted as associations within this predictive model and should not be interpreted as causal effects.

## Business Application

A hospital could potentially integrate a readmission-risk score into a discharge or transitional-care workflow.

Example workflow:

Patient Data
↓
Predictive Model
↓
30-Day Readmission Probability
↓
Risk Threshold
↓
High-Risk Patient Flag
↓
Care-Management Review
↓
Post-Discharge Intervention

Potential interventions could include:

- Post-discharge telephone follow-up
- Medication reconciliation
- Appointment confirmation
- Care-manager outreach
- Earlier outpatient follow-up
- Patient education

In a real healthcare environment, the threshold would also need to consider care-management capacity, intervention costs, clinical priorities, and prospective validation.

## Key Learnings

This project demonstrates:

- Healthcare problem formulation
- Exploratory data analysis
- Clinical feature engineering
- Missing-data handling
- Data leakage prevention
- Stratified train/validation/test methodology
- Logistic Regression
- Class-imbalance handling
- Cross-validation
- ROC-AUC analysis
- Precision-Recall analysis
- Threshold optimization
- Odds-ratio interpretation
- Translation of predictive analytics into healthcare workflow

## Limitations

This project uses a synthetic dataset and is intended for educational and portfolio purposes.

The model has not been clinically validated and should not be used for real patient-care decisions.

A real-world implementation would require:

- External validation
- Prospective validation
- Calibration assessment
- Clinical review
- Bias and fairness assessment
- Data privacy and security controls
- Integration with hospital information systems
- Monitoring for model drift
- Clinical governance and approval

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Imbalanced-learn
- Matplotlib
- Seaborn
- Jupyter Notebook
- Logistic Regression
- SMOTE
- Statistical and predictive analytics
