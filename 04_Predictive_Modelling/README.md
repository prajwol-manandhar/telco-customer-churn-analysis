# Predictive Modelling (Ankit — Predictive Modelling)

**Input:** `../02_Data_Engineering/data/processed/telco_ann_ready.csv`
(encoded, intentionally UNSCALED — split into train/test first, then fit
your scaler on the training set only, to avoid data leakage)

## To add here:
- `notebooks/` — ANN architecture, training, evaluation notebook
- `models/` — saved model file(s) (e.g. `.h5`, `.keras`, or `.pkl`)

## Notes
- Target column: `Churn` (0 = No, 1 = Yes)
- Class balance: ~73.5% No / ~26.5% Yes — use a stratified train/test split
- Recommended evaluation: F1-score, recall, ROC-AUC (not just accuracy)
- Target performance: ≥80% accuracy, F1 ≥ 0.70
