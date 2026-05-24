# Model Card: Customer Churn Prediction Model
## D2C Personal-Care Brand

---

## Model Overview

| Property | Detail |
|---|---|
| Model type | XGBoost Binary Classifier |
| Task | Predict customer churn in next 60 days |
| Decision threshold | 0.30 |
| Version | 1.0 |
| Last trained | May 2026 |

---

## Intended Use

**Primary**: Flag at-risk customers for the retention team to prioritize outreach.
**Secondary**: Rank customers by churn probability to allocate campaign budget.

**NOT intended for**: Automatically denying services, pricing decisions, or HR use.

---

## Data Used

| Source | Date Range | Size |
|---|---|---|
| RFM Modeling Snapshot | Historical | 2,401 customers |

**Snapshot date**: 2025-09-30 — no post-snapshot data was used as model input.

**Train / Val / Test split**: 70% / 15% / 15% (stratified by churn label)

---

## Model Performance

| Metric | Value |
|---|---|
| ROC-AUC | 0.8723 |
| PR-AUC | 0.8550 |
| Recall (churners) | 0.9226 |
| Precision (churners) | 0.7078 |
| F1-Score (churners) | 0.8010 |
| Accuracy | 0.7708 |

Threshold was set to **0.30** to prioritize **recall** — it is more costly to miss a churner
than to incorrectly flag a retained customer. This achieved a recall of >92%.

---

## Key Features (Top 5)

| Rank | Feature | Interpretation |
|---|---|---|
| 1 | `recency_days` | Most predictive feature. Higher recency strongly correlates with churn. |
| 2 | `last_visit_days_ago` | Lack of recent app/website activity flags churn risk. |
| 3 | `negative_ticket_rate_90d` | Support dissatisfaction is a direct indicator of leaving. |
| 4 | `return_rate_180d` | High return rates predict dissatisfaction and churn. |
| 5 | `category_diversity_180d` | Lower diversity means the user relies on us for fewer needs. |

---

## Limitations

- Model trained on one brand's customer base; may not generalize to other categories
- Customers with fewer than 2 orders have unreliable predictions
- Does not capture qualitative information (complaint tone, product reviews)
- Performance may degrade over time as customer behaviour evolves

---

## Ethical Risks

| Risk | Mitigation |
|---|---|
| Bias against certain demographics | Audit churn rates by age/location before campaign use |
| Over-reliance on model score | Always combine with human judgment for high-value decisions |
| Discount inequity | Do not reserve discounts only for flagged customers who belong to one demographic group |

---

## When NOT to Use This Model

- During major sale events (Diwali, Black Friday) — atypical buying behaviour
- For customers with fewer than 30 days of history
- If the data pipeline has not been refreshed in more than 7 days
- For making irreversible decisions (account closures, blocking)

---

## Monitoring Needs

- **Weekly**: Check feature drift on `recency_days` and `negative_ticket_rate_90d`
- **Monthly**: Recompute ROC-AUC against ground truth churn labels
- **Trigger retraining if**: ROC-AUC drops below 0.70, or feature distributions
  shift by more than 20% from training baseline
