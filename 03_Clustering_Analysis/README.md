# Clustering Analysis (Pabina — Clustering Analyst)

**Input:** `../02_Data_Engineering/data/processed/telco_clustering_ready_scaled.csv`

## To add here:
- `notebooks/` — clustering notebook (Elbow Method, Silhouette Score, K-Means training)
- `visualizations/` — cluster scatter plots, elbow/silhouette charts, segment profile charts

## Summary (fill in once finalised)
- Features used: `tenure`, `MonthlyCharges`
- Method: K-Means
- Optimal clusters: 4 (selected via Elbow Method + Silhouette Score)
- Key finding: newer customers with higher monthly charges show the highest churn rate; long-term, lower-charge customers show the lowest churn rate.
