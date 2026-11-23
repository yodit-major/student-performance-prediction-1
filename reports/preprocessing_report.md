# Data Preprocessing Report
## Student Performance & Academic Risk Prediction

### 1. Overview
This report documents the data cleaning, preprocessing, feature engineering, and dataset preparation performed for the Student Performance & Academic Risk Prediction project.

The first version uses the UCI Student Performance Mathematics dataset (`student-mat.csv`).

### 2. Dataset Overview
- Original records: 395 students
- Original features: 33 columns
- Numerical columns: 16
- Categorical columns: 17

### 3. Data Quality Assessment
The following checks were performed:

| Check | Result |
|---|---|
| Missing values | 0 |
| Duplicate records | 0 |
| Infinite values in processed training features | 0 |
| Infinite values in processed testing features | 0 |

No records were removed during cleaning.

Unusually high absence values and students with a final grade of zero were retained because unusual values are not automatically invalid.

### 4. Feature Selection
The proposed early-prediction approach excludes:
- G1: First-period grade
- G2: Second-period grade
- G3: Final grade

G3 is retained as the regression target and is used to derive the classification target. It is not included among the input features.

**Important:** The early-prediction approach must be confirmed by the project team.

### 5. Data Splitting
The dataset was divided using an 80:20 train-test split.

| Dataset | Records |
|---|---:|
| Training | 316 |
| Testing | 79 |
| Total | 395 |

Random state: 42

Stratification was performed using the proposed academic-risk labels to preserve their proportions in both sets.

### 6. Feature Encoding and Scaling

**Categorical features**
- One-hot encoding was applied.
- `handle_unknown="ignore"` was used to handle categories that may appear in testing but not training data.

**Numerical features**
- StandardScaler was applied.
- The scaler was fitted using training data only and then applied to testing data.

### 7. Processed Feature Summary

| Feature type | Number of features |
|---|---:|
| Numerical | 13 |
| One-hot encoded categorical | 43 |
| Total | 56 |

Final dimensions:
- Training features: 316 × 56
- Testing features: 79 × 56

### 8. Prediction Targets

**Regression:** Predict the final Mathematics grade (G3).

**Classification:** Predict academic risk using the proposed rule:

- 1 = At risk (G3 < 10)
- 0 = Not at risk (G3 >= 10)

The proposed threshold requires confirmation from the project team.

### 9. Academic Risk Distribution

| Dataset | Not at risk (0) | At risk (1) |
|---|---:|---:|
| Full dataset | 265 | 130 |
| Training | 212 | 104 |
| Testing | 53 | 26 |

### 10. Output Files

All processed datasets are stored in `data/processed/`.

- `X_train.csv`
- `X_test.csv`
- `y_grade_train.csv`
- `y_grade_test.csv`
- `y_risk_train.csv`
- `y_risk_test.csv`

The original dataset row index is preserved as `student_index` to maintain feature-target alignment.

### 11. Final Verification
All six saved datasets were loaded and checked successfully.

Feature-target alignment was verified for both prediction tasks.

### 12. Notes and Future Improvements
- Confirm the early-prediction approach with the project team.
- Confirm the academic-risk threshold with the project team.
- Consider using a Scikit-learn Pipeline and ColumnTransformer for reusable preprocessing.
- Save the fitted encoder and scaler if the team needs to reuse the exact transformations.
- Evaluate the impact of different preprocessing choices during model development.