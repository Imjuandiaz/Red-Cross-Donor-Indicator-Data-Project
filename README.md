# Data-Driven Insights on Donor Behavior (2025)

Predictive analytics project to classify donor likelihood (`DonorIndicatorFlag`) using demographics, geography (ZIP/State), and donation history.
Built with pandas, PyCaret (classification), and Tableau for visualization.

---

## 📁 Repository Structure

```
.
├── data/
│   ├── raw/                # original CSVs (not tracked / or use small samples)
│   ├── interim/            # optional temporary artifacts
│   └── processed/          # cleaned datasets used by notebooks & models
├── models/                 # CSV outputs: clustering, anomaly detection, donor prediction
├── notebooks/              # Jupyter notebooks (ordered 01..05)
├── reports/
│   ├── profiling/          # ydata-profiling HTML
│   ├── autoviz/            # AutoViz HTML reports
│   └── slides/             # final PDF; link to Google Slides
├── src/
│   ├── data/               # cleaning scripts
│   ├── features/           # feature engineering (ZIP -> State)
│   ├── models/             # training / evaluation
│   └── visualization/      # figure generation helpers
├── visuals/
│   ├── eda/                # EDA charts (before/after cleaning, heatmaps)
│   └── model/              # ML plots (confusion matrix, ROC, PR, etc.)
├── requirements.txt
└── README.md
```

> **Note:** Large files should live in `data/raw/` and be excluded via `.gitignore`. A small anonymized sample is included for quick runs.

---

## 🎯 Business Case

Identify which factors most influence donor participation and build a classifier to predict whether an individual is a donor (1) or not (0). Insights guide targeting, retention, and regional strategy.

---

## 🧹 Data Preparation (summary)

- **Rows:** 34,508  
- **Duplicates:** none detected  
- **Removed columns (>\~20% missing or not actionable):** `MaritalStatus`, `AcademicDegreeLevel`, `DonorDateOfBirth`, `IsMemberFlag`  
- **Feeder variable excluded (to avoid leakage):** `CumulativeDonationAmount`  
- **Kept due to business value:** `WealthRating`  
- **Standardization:** donations → numeric; flags (Y/N) → 0/1; ZIP → string (no `.0`, preserves leading zeros)

---

## 🧱 Feature Engineering

- **ZIP → State:** added `DonorState` using `pgeocode`
- **Consistency:** `ConsecutiveDonorYears`
- **Donation history:** last 5 fiscal years + current fiscal year
- **Categoricals handled:** `GenderIdentity`, `WealthRating`, `PreferredAddressType`, `DonorState`, `DonorPostalCode`

---

## 🔎 EDA (Tableau)

Key visuals (in `visuals/` and presentation):
- Donor Age Distribution (Donors Only)
- Donor Rate by Wealth Rating
- Donor vs Non-Donor Split
- Top 10 ZIP Codes by Donor Rate & Donor Rate by Preferred Address Type
- Donor Distribution by State
- Donor Rate by Gender Identity

---

## 🤖 Modeling (PyCaret – Classification)

- Multiple models compared with `compare_models()`
- **Selected:** Quadratic Discriminant Analysis (QDA)
- **Hold-out metrics (approx.):**
  - Accuracy ≈ **0.69**
  - AUC ≈ **0.75**
  - Precision ≈ **0.98**
  - Recall ≈ **0.48**

Supporting plots: Confusion Matrix, ROC, Precision-Recall, Learning Curve, Calibration.

> Trade-off: high precision (very few false positives) with lower recall. Balanced versions and threshold tuning are outlined in notebooks.

---

## 🚀 Quickstart

```bash
# 1) Create venv (recommended Python 3.10 or 3.11)
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate

# 2) Install deps
pip install -r requirements.txt

# 3) (Optional) Jupyter kernel
python -m ipykernel install --user --name donor-ml --display-name "donor-ml"

# 4) Run notebooks
jupyter lab   # open notebooks/01_data_cleaning.ipynb → ... 05_model_eval.ipynb
```

---

## 🔧 Reproducible pipeline (scripts)

```bash
# Example CLI flow (if you use src scripts)
python src/data/clean_data.py
python src/features/build_features.py
python src/models/train_model.py
python src/models/evaluate_model.py
```

---

## 📊 Reports & Slides

- **Profiling (HTML):** `reports/profiling/…`
- **AutoViz (HTML):** `reports/autoviz/…`
- **Slides (PDF / Google Slides link):** `reports/slides/…`

---

## 📜 License & Acknowledgements

- Add a license if you plan to share publicly (e.g., MIT).
- Data has been processed to remove leakage; cumulative donation was excluded from modeling.

---

## ✉️ Contact

**Juan Díaz — Data Analyst**  
(LinkedIn https://www.linkedin.com/in/juan-d%C3%ADaz-2003b6335/        / Email jdrd6903@gmail.com)
