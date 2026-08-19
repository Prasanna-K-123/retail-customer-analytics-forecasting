# Customer Analytics, Segmentation & Revenue Forecasting

End-to-end retail analytics project using the **UCI Online Retail II** transaction dataset. The project combines data cleaning, SQL-based business analysis, customer segmentation, forecasting, and an executive Power BI dashboard.

## Business questions

- What data-quality issues must be handled before analysis?
- Which customer groups differ meaningfully in recency, purchase frequency, and monetary value?
- Can a simple, interpretable forecasting approach outperform a more complex classical model on unseen future data?
- How can the analytical output be translated into business-facing customer and revenue insights?

## Dataset

**UCI Online Retail II** — two years of transactional data from a UK-based online retailer.

- Raw transactions: **1,067,371**
- Positive realized-revenue transactions after cleaning: **1,007,913**
- Customer-attributable transactions: **779,425**
- Unique customers used for RFM analysis: **5,878**

## Analytical workflow

1. **Data audit and cleaning**
   - Audited missing values, duplicates, non-positive quantities/prices, and cancellation invoices.
   - Parsed transaction dates and retained an auditable raw copy.
   - Excluded cancellations and non-positive transactions for realized-revenue analysis.

2. **Business analysis with Python and SQL**
   - Used Pandas for revenue, country, product, and time-based analysis.
   - Used DuckDB SQL for monthly revenue and country-level order/revenue summaries.

3. **RFM customer segmentation**
   - Engineered **Recency, Frequency, Monetary** features.
   - Applied log transformation and standardization before K-Means.
   - Compared `K = 2...6` using silhouette score.
   - Selected **K = 2**, with silhouette score **0.439**.

4. **Revenue forecasting**
   - Built a 28-day future holdout.
   - Compared a **seasonal-naive baseline** against **Holt-Winters exponential smoothing**.
   - Evaluated performance using MAE and RMSE.

5. **Power BI executive dashboard**
   - Built KPI cards, customer-segment visualization, model comparison, and actual-vs-forecast reporting.

## Key verified results

### Customer segmentation

| Segment | Customers | Avg. Recency | Avg. Frequency | Avg. Monetary Value |
|---|---:|---:|---:|---:|
| Cluster 1 — high-value / active | 2,332 | 51.05 days | 12.67 | £6,520.68 |
| Cluster 0 — low-engagement / lapsed | 3,546 | 301.05 days | 2.09 | £611.55 |

Cluster 1 represents **39.67% of customers** but contributes approximately **87.5% of total RFM monetary value**, indicating a commercially important high-value customer group.

### Forecasting

| Model | Test MAE | Test RMSE |
|---|---:|---:|
| Seasonal Naive | **£15,169.10** | **£30,928.91** |
| Holt-Winters | £17,779.62 | £31,865.50 |

The seasonal-naive baseline achieved **14.7% lower MAE** than Holt-Winters on the 28-day holdout. This is an important modeling result: additional complexity did not improve generalization enough to justify itself on this test period.

## Business interpretation

- The customer base separates into a smaller, recently active, high-frequency, high-value group and a larger, substantially less active group.
- The high-value segment should be prioritized for retention and service-quality analysis, while interventions should still be tested rather than assumed to cause higher retention.
- Forecasting results demonstrate the importance of comparing sophisticated models against strong baselines before deployment.

## Technology stack

- **Python:** Pandas, NumPy, Matplotlib
- **Machine Learning:** scikit-learn, K-Means, StandardScaler, silhouette score
- **Forecasting:** statsmodels, Holt-Winters exponential smoothing
- **SQL:** DuckDB
- **Visualization:** Power BI, Matplotlib

## Repository structure

```text
retail-customer-analytics-forecasting/
├── README.md
├── accenture_retail_analytics.ipynb
└── requirements.txt
```

## Reproducing the analysis

1. Open `accenture_retail_analytics.ipynb` in Google Colab or Jupyter.
2. Install the dependencies listed in `requirements.txt`.
3. Run the notebook from top to bottom.
4. The notebook loads the UCI dataset, performs the analysis, and exports segmentation and forecast output files for dashboarding.

## Limitations

- Transactional observational data does not establish causal effects of customer interventions.
- Customer IDs are missing for part of the raw data, so customer-level modeling uses the attributable subset.
- Cluster labels are descriptive groupings, not intrinsic customer types.
- Forecast performance is based on one historical holdout and may change under future regime shifts.
