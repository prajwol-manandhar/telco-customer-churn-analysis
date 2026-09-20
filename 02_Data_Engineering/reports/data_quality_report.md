# Data Quality & Preprocessing Log

Generated automatically by `data_preprocessing.py`.


## STAGE 1: LOAD, AUDIT & CLEAN

- Loaded dataset: 7043 rows, 10 columns
- Columns: ['gender', 'SeniorCitizen', 'Dependents', 'tenure', 'PhoneService', 'MultipleLines', 'InternetService', 'Contract', 'MonthlyCharges', 'Churn']
- Missing values: NONE found across any column.
- Trimmed whitespace on categorical columns: ['gender', 'Dependents', 'PhoneService', 'MultipleLines', 'InternetService', 'Contract', 'Churn']
- Exact duplicate rows found: 302
- DECISION: Duplicates are NOT dropped. The dataset has no unique customer ID, and the feature space is small (2 genders x 2 SeniorCitizen x 2 Dependents x 2 PhoneService x 2 MultipleLines x 2 InternetService x 3 Contract x ~73 tenure values x ~101 MonthlyCharges values). At n=7,043 rows, exact-row collisions are statistically plausible by chance for genuinely distinct customers who happen to share every recorded attribute. Without an ID column there is no way to confirm these are true duplicate records rather than coincidental matches, so removing them risks discarding real customers. This is flagged for the team as a data collection limitation (recommend requesting a customer ID in future extracts).
- Logical inconsistency found: 260 rows have PhoneService='No' but MultipleLines='Yes' (impossible combination).
- DECISION: Recoding MultipleLines to 'No' wherever PhoneService is 'No', consistent with how this field is defined in standard telco churn datasets (MultipleLines is only meaningful when phone service exists).
- tenure range: 0–72 months (no negatives/nulls)
- tenure == 0 (new customers, first billing cycle): 11 rows — valid, kept as-is.
- MonthlyCharges range: 18–119 (no negatives/nulls)
- Binary-encoded columns: ['gender', 'Dependents', 'PhoneService', 'MultipleLines', 'InternetService', 'Churn']
- One-hot encoded 'Contract' into: ['Contract_Month-to-month', 'Contract_One year', 'Contract_Two year']
- Stage 1 complete: dataset cleaned and fully encoded.


## STAGE 2: FEATURE ENGINEERING & SCALING

- Engineered 'ContractMonths': numeric commitment length (0/12/24).
- Engineered 'TenureYears': tenure expressed in years for interpretability.
- Engineered 'TenureGroup' (binned tenure) + one-hot dummies — useful for profiling/interpreting clusters in plain business language.
- Engineered 'ServiceCount': count of phone-related add-on services (0-2).
- Engineered 'MonthlyChargePerService': MonthlyCharges normalised by service footprint — a proxy for price sensitivity.
- Selected features for clustering: ['tenure', 'MonthlyCharges', 'ContractMonths', 'ServiceCount', 'MonthlyChargePerService', 'InternetService', 'SeniorCitizen', 'Dependents']
- Applied StandardScaler (zero mean, unit variance) to clustering feature set. NOTE: this scaler is fit on the FULL dataset, which is appropriate for unsupervised clustering (no train/test leakage concern since there is no target being predicted).
- Stage 2 complete: features engineered and scaled.


## STAGE 3: VALIDATION, QA & HANDOFF

- QA check passed: no nulls present after transformation.
- QA check passed: ServiceCount within expected range [0, 2].
- QA check passed: Churn target is binary (0/1).
- QA check passed: PhoneService/MultipleLines logical inconsistency resolved.
- QA check passed: all engineered features populated with no nulls.
- Target class balance: Churn=Yes in 26.5% of rows (Churn=No in 73.5%). Mild imbalance — recommend the ANN team use stratified train/test splitting and consider class weighting or a threshold-tuning step during evaluation.
- Saved: outputs/telco_cleaned_encoded.csv (7043 rows, 18 cols)
- Saved: outputs/telco_clustering_ready_scaled.csv (7043 rows, 8 cols)
- Saved: outputs/telco_ann_ready.csv (7043 rows, 15 cols). NOTE: left unscaled on purpose — scale AFTER train/test split to prevent leakage.

Stage 3 complete: files validated and handed off to Clustering & ANN teams.

## Known Limitations & Challenges (for the team report)

- **No customer ID column.** This blocks definitive duplicate-record
  detection and means the 302 duplicate rows cannot be conclusively
  resolved — they were retained (see Stage 1 decision above). Recommend
  the team request a unique customer key in future data pulls.
- **No `TotalCharges` or account-value field.** Unlike the standard public
  Telco churn dataset, this extract has only `MonthlyCharges`, so we
  cannot derive lifetime value or a `TotalCharges`-based sanity check
  (e.g. tenure × MonthlyCharges vs. actual billed total).
- **Weak apparent relationship between tenure/spend and contract type or
  internet type.** Mean tenure is ~32 months and spend distributions are
  nearly identical across `Contract` and `InternetService` groups —
  patterns that differ from typical real-world telco behaviour (where
  longer contracts usually correlate with longer tenure). This suggests
  either genuinely low signal in this extract or a partially synthetic /
  resampled dataset. Flagged so the Clustering and ANN teams calibrate
  their expectations on model performance accordingly.
- **Limited feature set (9 predictors).** No billing history, support
  tickets, payment method, or demographics beyond gender/senior
  status/dependents were available, which caps the ceiling on predictive
  accuracy achievable from this extract alone.
- **Mild class imbalance** (26.5% churn) — noted above for the ANN team's
  train/test split and evaluation-metric choices (prefer F1/recall/ROC-AUC
  over raw accuracy).
