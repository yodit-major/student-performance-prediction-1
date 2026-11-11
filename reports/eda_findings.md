# Exploratory Data Analysis (EDA) Findings Report
**Author:** Member 1 (Dataset Research & EDA)  
**Dataset:** UCI Student Performance (`student-mat.csv` & `student-por.csv`)

## 1. Executive Summary
The UCI Student Performance dataset contains demographic, social, family, and school-related attributes for secondary school students in Portugal across two subjects: Mathematics ($N = 395$) and Portuguese ($N = 649$). Initial exploration confirms clean tabular structure with zero missing values, strong predictive signals from prior academic history, and an important structural anomaly (`G3 = 0`) that requires a clear team decision before model training.

---

## 2. Key Findings & Patterns Associated with G3

### A. Target Variable (`G3`) Distribution & Academic Risk Balance
* **Math (`student-mat.csv`):** Mean `G3` is **10.42** ($\text{SD} = 4.58$), with a median of **11.0**. Using a project-defined risk threshold of `G3 < 10`, **130 students (32.91%)** fall into the at-risk category.
* **Portuguese (`student-por.csv`):** Mean `G3` is higher at **11.91** ($\text{SD} = 3.23$), with a median of **12.0**. Only **100 students (15.41%)** fall below `G3 < 10`, indicating stronger class imbalance in Portuguese than in Math.

### B. Data Leakage Warning (`G1` and `G2`)
* First-period (`G1`) and second-period (`G2`) grades show extremely high linear correlation with `G3` ($r \approx 0.80$ to $0.90$).
* **Recommendation:** To build a realistic **early-warning model**, `G1` and `G2` **must be excluded** from training features. A secondary comparison experiment including `G1` and `G2` may be run separately if clearly labeled as a mid-year intervention model.

### C. Strongest Non-Grade Predictors (Early-Warning Features)
1. **Past Class Failures (`failures`):** Exhibits the strongest negative correlation with `G3` ($r \approx -0.36$ in Math, $-0.39$ in Portuguese). Students with 1 or more past failures show sharply lower median final grades.
2. **Higher Education Aspiration (`higher`):** Students who intend to pursue higher education (`yes`) score substantially higher than those who do not (`no`).
3. **Parental Education (`Medu` & `Fedu`):** Higher maternal and paternal education levels correlate positively ($r \approx 0.21$) with higher `G3` scores.
4. **Study Time (`studytime`):** Shows a modest positive association with `G3`, whereas frequent going out (`goout`) and high workday alcohol consumption (`Dalc`) show negative associations.

### D. The `G3 = 0` Anomaly
* Exactly **38 Math students (9.62%)** and **15 Portuguese students (2.31%)** have `G3 = 0`, despite having `0` recorded absences and positive `G1`/`G2` grades (often between 7 and 11).
* This creates a bimodal spike at zero in the `G3` histogram. These records likely represent exam non-attendance or administrative dropouts rather than true numeric exam performance.

---

## 3. Actionable Handoff Notes for Team Members
* **For Member 2 (Preprocessing):**
  * Parse CSVs using `sep=';'`.
  * Decide whether to keep `G3 = 0` rows (realistic dropout/failure behavior) or inspect their impact on regression error (RMSE).
  * Separate features into two explicit sets: **Early-Warning Set** (excludes `G1`, `G2`) and **Mid-Year Set** (includes `G1`, `G2`).
  * Use stratified train/test splitting for classification due to the ~33% (Math) vs. ~15% (Portuguese) risk-class imbalance.
* **For Member 3 (Regression):** Expect `G3 = 0` outliers to disproportionately inflate RMSE if left in the dataset; compare MAE alongside RMSE.
* **For Member 4 (Classification):** Using `G3 < 10` as the risk threshold yields a well-balanced ~33% positive class in Math, making Math an ideal primary benchmark dataset for classification.
* **For Member 5 (Integration & Ethics):** All findings reflect statistical associations in two Portuguese schools from 2005–2006. Feature correlations (such as parental education or past failures) must be framed as exploratory risk signals rather than deterministic or causal judgments.
