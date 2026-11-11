
# Dataset Source, Data Dictionary & Initial Quality Report

## 1. Dataset Source Metadata (DS-01)
* **Dataset Name:** Student Performance
* **Repository:** UCI Machine Learning Repository
* **Source URL:** https://uci.edu
* **Authors / Collectors:** Paulo Cortez and Alice Silva (University of Minho, Portugal)
* **Files Included:** 
  * `data/raw/student-mat.csv` (Mathematics course: 395 instances, 33 attributes)
  * `data/raw/student-por.csv` (Portuguese language course: 649 instances, 33 attributes)
* **Delimiter:** Semicolon (`;`)

---

## 2. Data Dictionary (DS-02)
Both datasets share the exact same 33 attributes:

| # | Column Name | Data Type | Category / Role | Meaning & Valid Values |
| :--- | :--- | :--- | :--- | :--- |
| 1 | `school` | Categorical (Binary) | Demographic | Student's school (`GP` = Gabriel Pereira, `MS` = Mousinho da Silveira) |
| 2 | `sex` | Categorical (Binary) | Demographic | Student's sex (`F` = female, `M` = male) |
| 3 | `age` | Numerical (Integer) | Demographic | Student's age (15 to 22) |
| 4 | `address` | Categorical (Binary) | Demographic | Home address type (`U` = urban, `R` = rural) |
| 5 | `famsize` | Categorical (Binary) | Family | Family size (`LE3` = <= 3, `GT3` = > 3) |
| 6 | `Pstatus` | Categorical (Binary) | Family | Parent's cohabitation status (`T` = living together, `A` = apart) |
| 7 | `Medu` | Numerical (Ordinal) | Family | Mother's education (0 = none, 1 = primary/4th grade, 2 = 5th–9th grade, 3 = secondary, 4 = higher) |
| 8 | `Fedu` | Numerical (Ordinal) | Family | Father's education (0 = none, 1 = primary/4th grade, 2 = 5th–9th grade, 3 = secondary, 4 = higher) |
| 9 | `Mjob` | Categorical (Nominal) | Family | Mother's job (`teacher`, `health`, `services`, `at_home`, `other`) |
| 10 | `Fjob` | Categorical (Nominal) | Family | Father's job (`teacher`, `health`, `services`, `at_home`, `other`) |
| 11 | `reason` | Categorical (Nominal) | School | Reason to choose this school (`home`, `reputation`, `course`, `other`) |
| 12 | `guardian` | Categorical (Nominal) | Family | Student's guardian (`mother`, `father`, `other`) |
| 13 | `traveltime` | Numerical (Ordinal) | Lifestyle | Home to school travel time (1 = <15 min, 2 = 15–30 min, 3 = 30 min–1 hr, 4 = >1 hr) |
| 14 | `studytime` | Numerical (Ordinal) | Academic | Weekly study time (1 = <2 hrs, 2 = 2–5 hrs, 3 = 5–10 hrs, 4 = >10 hrs) |
| 15 | `failures` | Numerical (Integer) | Academic | Number of past class failures (`n` if 1 <= n < 3, else 4) |
| 16 | `schoolsup` | Categorical (Binary) | Support | Extra educational support (`yes`, `no`) |
| 17 | `famsup` | Categorical (Binary) | Support | Family educational support (`yes`, `no`) |
| 18 | `paid` | Categorical (Binary) | Support | Extra paid classes within the course subject (`yes`, `no`) |
| 19 | `activities` | Categorical (Binary) | Lifestyle | Extra-curricular activities (`yes`, `no`) |
| 20 | `nursery` | Categorical (Binary) | Background | Attended nursery school (`yes`, `no`) |
| 21 | `higher` | Categorical (Binary) | Aspiration | Wants to take higher education (`yes`, `no`) |
| 22 | `internet` | Categorical (Binary) | Access | Internet access at home (`yes`, `no`) |
| 23 | `romantic` | Categorical (Binary) | Lifestyle | With a romantic relationship (`yes`, `no`) |
| 24 | `famrel` | Numerical (Ordinal) | Family | Quality of family relationships (1 = very bad to 5 = excellent) |
| 25 | `freetime` | Numerical (Ordinal) | Lifestyle | Free time after school (1 = very low to 5 = very high) |
| 26 | `goout` | Numerical (Ordinal) | Lifestyle | Going out with friends (1 = very low to 5 = very high) |
| 27 | `Dalc` | Numerical (Ordinal) | Lifestyle | Workday alcohol consumption (1 = very low to 5 = very high) |
| 28 | `Walc` | Numerical (Ordinal) | Lifestyle | Weekend alcohol consumption (1 = very low to 5 = very high) |
| 29 | `health` | Numerical (Ordinal) | Health | Current health status (1 = very bad to 5 = very good) |
| 30 | `absences` | Numerical (Integer) | Academic | Number of school absences (0 to 93) |
| 31 | `G1` | Numerical (Integer) | Target/Leakage | First period grade (0 to 20) — **Exclude for early-warning models** |
| 32 | `G2` | Numerical (Integer) | Target/Leakage | Second period grade (0 to 20) — **Exclude for early-warning models** |
| 33 | `G3` | Numerical (Integer) | **Primary Target** | Final year grade (0 to 20) |

---

## 3. Data-Quality Audit Summary (DS-03)
* **Missing Values:** Both `student-mat.csv` and `student-por.csv` contain **0 explicit `NaN` / null values** across all 33 columns.
* **Exact Duplicates:** Both files contain **0 exact duplicate rows**. *(Note: According to UCI documentation, 382 students appear in both datasets, which Member 2 must account for if merging).*
* **Formatting Quirk:** Both CSV files use semicolons (`;`) rather than commas (`,`) as separators. Loading without `sep=';'` will parse the entire row into a single column.
* **Suspicious / Domain-Specific Values (`G3 == 0`):** 
  * In `student-mat.csv`, **38 students (~9.62%)** have `G3 = 0`.
  * In `student-por.csv`, **15 students (~2.31%)** have `G3 = 0`.
  * **Important Note for Member 2:** Many students with `G3 = 0` had passing scores in `G1` and `G2` (e.g., 7–11). In the Portuguese grading system, a `0` in `G3` often indicates a student dropped out, did not sit for the final exam, or had unexcused absences rather than earning zero points on a test. Member 2 must decide whether to keep or filter `G3 = 0` records.
