# Customer Churn Analysis for a Telecommunications Company

Group project submission — analysing customer churn using data preprocessing,
clustering analysis, and predictive modelling (ANN), with retention
recommendations for stakeholders.

## Team

| Member  | Role                  |
|---------|-----------------------|
| Prajwol | Data Engineer         |
| Ankit   | Predictive Modelling  |
| Pabina  | Clustering Analysis   |
| Sumina  | Business Analyst      |

**Start date:** 26/07/2026 · **Target completion:** 26/10/2026

## Repository Structure

```
customer-churn-analysis/
├── 01_Project_Charter/            # Stage 1 — project charter (scope, goals, timeline, risks)
├── 02_Data_Engineering/            # Data loading, cleaning, encoding, feature engineering
│   ├── data/
│   │   ├── raw/                    # Original dataset
│   │   └── processed/              # Cleaned/encoded/scaled handoff files
│   ├── notebooks/                  # Preprocessing notebook
│   └── reports/                    # Data quality / QA log
├── 03_Clustering_Analysis/         # Customer segmentation (K-Means)
│   ├── notebooks/
│   └── visualizations/
├── 04_Predictive_Modelling/        # ANN churn prediction model
│   ├── notebooks/
│   └── models/
├── 05_Final_Report_and_Presentation/  # Final report, slides, findings & recommendations
└── 06_Meeting_Notes/                # Sprint meeting summaries and agendas
```

## Pipeline Overview

1. **Data Engineering** → cleans and encodes the raw dataset, engineers new
   features (`ContractMonths`, `TenureYears`, `ServiceCount`,
   `MonthlyChargePerService`, etc.), and exports two handoff files:
   - `telco_clustering_ready_scaled.csv` → scaled, no target (for clustering)
   - `telco_ann_ready.csv` → encoded, unscaled, with target (for the ANN)
2. **Clustering Analysis** → segments customers using K-Means on tenure and
   monthly charges, informed by the Elbow Method and Silhouette Score.
3. **Predictive Modelling** → trains an ANN on `telco_ann_ready.csv` to
   forecast churn.
4. **Final Report** → combines findings from all three stages into churn
   drivers, retention recommendations, and a stakeholder presentation.

## Tools & Technologies

- Python (Pandas, NumPy, Matplotlib, Scikit-learn, Seaborn)
- Jupyter Notebooks / Google Colab
- Jira (task/sprint tracking) & Confluence (documentation)
- Git / GitHub for version control

## Known Data Limitations

- No unique customer ID (302 exact-duplicate rows retained, not dropped)
- No `TotalCharges`/billing-history field
- Weak apparent relationship between tenure/spend and contract/internet type
- Mild class imbalance in the target (~26.5% churn)

See `02_Data_Engineering/reports/data_quality_report.md` for full details.
