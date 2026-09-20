# Training and Testing Sets — Documentation

## Split Method
- **Method:** Stratified train/test split (via `sklearn.model_selection.train_test_split`)
- **Split ratio:** 80% train / 20% test
- **Stratification:** on the `Churn` target column, to preserve class balance
  in both sets
- **Random state:** 42 (for reproducibility)

## Size & Composition

| Set   | Rows  | % of Total | Churn = No | Churn = Yes | Churn Rate |
|-------|-------|------------|------------|-------------|------------|
| Train | 5634 | 80.0% | 4139 | 1495 | 26.5% |
| Test  | 1409 | 20.0% | 1035 | 374 | 26.5% |
| **Total** | **7043** | **100%** | **5174** | **1869** | **26.5%** |

Stratification kept the churn rate nearly identical across both sets
(~26.5%), so the test set is representative of the overall population and
won't give a misleadingly easy or hard evaluation.

## Files in this folder

| File | Description |
|------|--------------|
| `train_set.csv` | Training set, encoded but **unscaled** |
| `test_set.csv` | Test set, encoded but **unscaled** |
| `train_set_scaled.csv` | Training set with numeric features scaled (StandardScaler fit on this set) |
| `test_set_scaled.csv` | Test set with numeric features scaled (using the **same** scaler fit on train — transform only) |

## Why both scaled and unscaled versions are provided
Some models (e.g. tree-based models) don't require scaled inputs, while
others (e.g. the ANN, or distance-based methods) perform better with
scaled numeric features. Providing both avoids re-deriving the split for
different modelling needs, while keeping the scaling methodology fully
transparent and leak-free (see `Scaling_Techniques_Documentation/` for
details).
