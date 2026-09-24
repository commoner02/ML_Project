# Digital Device Use, Generative AI Adoption & Academic Performance

A Machine Learning study and curated empirical dataset investigating the relationships between daily screen habits, Generative AI adoption, sleep hygiene, and Cumulative Grade Point Average (CGPA) among engineering undergraduates.

---

## 📌 Overview

- **Dataset Size:** 1,271 verified student records (28 attributes)
- **Target Variable:** 5-class ordinal CGPA bracket (`Below 3.00`, `3.00–3.24`, `3.25–3.49`, `3.50–3.74`, `3.75–4.00`)
- **Institution:** Khulna University of Engineering & Technology (KUET) and engineering universities across Bangladesh
- **Course:** CSE 4112 — Machine Learning Laboratory

---

## 📁 Repository Structure

```text
├── data.csv          # Survey dataset (N = 1,271)
├── main.ipynb        # Preprocessing, feature engineering & EDA notebook
├── figures/          # Generated EDA plots & visualizations
├── requirements.txt  # Python package dependencies
└── README.md         # Project documentation
```

---

## 🛠️ Data Preprocessing & Feature Engineering

The raw survey data underwent a multi-step curation pipeline in **`main.ipynb`** prior to Weka modeling:

1. **Metadata & Leakage Control:**
   - Stripped survey `Timestamp` to eliminate temporal bias and prevent leakage.
   - Verified 100% complete records with zero missing or corrupt entries.

2. **Screen Duration Discretization:**
   - Converted interval string bins (e.g., `'0 - 2 hours'`, `'2 - 4 hours'`, `'More than 8 hours'`) into continuous numeric midpoints (`Daily_Usage_Hours`, `Academic_Usage_Hours`).

3. **Sub-Item Likert Scaling (1–5):**
   - **Non-Academic Activities (5 features):** Extracted time spent on *Social Media*, *Messaging*, *YouTube/Streaming*, *Gaming*, and *Browsing/News* into quantitative 1–5 scales.
   - **Task-Specific GenAI Adoption (5 features):** Extracted frequency of AI tool use for *Theory/Lab Comprehension*, *Report Writing*, *Exam Preparation*, *Research Papers*, and *Presentations* (1 = Not at all, 5 = Mostly Dependent).
   - **Behavioral Distraction Battery (7 features):** Maintained Likert indicators capturing notification interruptions, aimless checking, study multitasking, and difficulty halting device use.

4. **Sleep Chronotype & Biphasic Sleep Engineering:**
   - **Bedtime & Wake-up Times:** Transformed categorical time intervals into continuous 24-hour clock values (`Sleep_Hour`: 22.5 to 25.5; `Wake_Hour`: 5.5 to 8.5).
   - **Second Sleep (Daytime Nap):** Mapped `Yes/No` to binary flag `Has_Second_Sleep` (1/0), parsed clock strings into decimal hours (`SecondSleep_Start_Hour`, `SecondSleep_End_Hour`), and applied conditional median imputation (15.75 and 17.25) for non-sleepers.

5. **Target & Categorical Encoding:**
   - **Target Variable (CGPA):** Encoded into an ordinal integer target (`CGPA_Score`: 1 to 5) and numerical interval midpoints (`CGPA_Midpoint`: 2.50 to 3.87).
   - **Demographics & Hardware:** Applied One-Hot Encoding on *Department*, *Academic Year*, and *Primary Device*, dropping baseline reference columns to avoid multicollinearity.

---

## ⚙️ Weka Benchmark Workflow

```text
data.csv ──> Python Preprocessing (main.ipynb) ──> ARFF/CSV Export ──> Weka 3.8.6 (10-Fold CV)
```

- **Evaluation Scheme:** 10-Fold Stratified Cross-Validation (Seed 1)
- **Evaluated Models:**
  - **J48 Decision Tree:** `weka.classifiers.trees.J48 -C 0.25 -M 2`
  - **Random Forest:** `weka.classifiers.trees.RandomForest -I 100 -K 0 -M 1.0 -S 1`
  - **Naive Bayes:** `weka.classifiers.bayes.NaiveBayes`

---

## 📊 Benchmark Results (Weka 10-Fold CV)

| Model | Accuracy | Cohen's Kappa ($\kappa$) | Weighted Precision | Weighted Recall | Weighted ROC-AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Random Forest (100 Trees)** | **35.01%** | 0.1519 | 0.354 | 0.350 | 0.655 |
| **Naive Bayes** | 33.99% | **0.1681** | **0.366** | 0.340 | **0.683** |
| **J48 Decision Tree** | 30.84% | 0.1103 | 0.309 | 0.308 | 0.584 |

---

## 🚀 Quick Start

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Run Preprocessing & EDA
Open and run **`main.ipynb`** in Jupyter Notebook or VS Code to reproduce the data transformations and exploratory charts.

---

## 👥 Contributors (Group 18)

| Roll | Name | Department |
| :---: | :--- | :---: |
| **2107103** | Saleheen Uddin Sakin | CSE, KUET |
| **2107105** | Abdullah Al Noman | CSE, KUET |
| **2107110** | Nure Alam Siddiki Prince | CSE, KUET |
| **2107113** | Ankon Roy | CSE, KUET |
| **2107116** | Sree Shuvo Kumar Joy | CSE, KUET |
| **2107117** | Niloy Chowdhury | CSE, KUET |

---

## 📜 License
This project and dataset are licensed under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.
