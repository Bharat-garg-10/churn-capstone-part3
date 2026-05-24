# Error Analysis Report
## Churn Prediction Model — Part 3

---

## Overview

| Metric | Value |
|---|---|
| Test set size | 336 |
| True Positives (correct churn) | 155 |
| True Negatives (correct retain) | 104 |
| False Positives (wrong churn flag) | 64 |
| False Negatives (missed churner) | 13 |

**Decision threshold**: 0.30
**Business implication**: False Negatives are more costly — we lose a customer entirely.
False Positives waste retention budget but cause no direct revenue loss. We set a low threshold (0.30) to prioritize Recall (92.26%).

---

## False Positive Analysis
*Predicted churn — Customer actually stayed*

| Customer ID | Churn Prob | Key Signals | Why the Model Was Wrong |
|---|---|---|---|
| CUST01370 | 0.929 | High recency (161 days), 0 tickets | Looked completely disengaged but organically returned late in the window |
| CUST01246 | 0.908 | Very high recency (262 days), $0 spend | Natural seasonal buyer, flagged due to inactivity but hadn't formally churned |
| CUST00437 | 0.906 | High recency (151 days), 1 order | Looked like a one-off buyer but re-engaged without intervention |
| CUST01017 | 0.904 | High return rate (50%), 133 days recency | Returns were size-related exchanges, not complete brand dissatisfaction |
| CUST01325 | 0.889 | High recency (186 days), $0 spend | Model overly penalizes extreme inactivity; customer retained naturally |

**Business Risk of FPs**: Wasted retention spend (discount sent to loyal or naturally returning customer who did not need it).

---

## False Negative Analysis
*Predicted retained — Customer actually churned*

| Customer ID | Churn Prob | Key Signals | Why the Model Missed |
|---|---|---|---|
| CUST00938 | 0.296 | Low recency (26), High spend ($1882), 1 ticket | Highly active and high monetary value masked underlying dissatisfaction |
| CUST00438 | 0.288 | 100% return rate, high tickets | Should have been flagged, but high frequency/spend caused the model to underestimate risk |
| CUST00247 | 0.282 | Medium recency (57 days), 0 tickets | "Silent Churner" — left with absolutely no negative warning signals |
| CUST00838 | 0.214 | Very low recency (9 days), 1 ticket | Active very recently, making the model assume they were highly engaged |
| CUST02072 | 0.190 | High frequency (7), High spend ($4340), 0 tickets | VIP loyalist who abruptly left; historical engagement overshadowed the churn |

**Business Risk of FNs**: No intervention — customer is lost with no revenue recovery opportunity. We missed these 13 customers entirely.

---

## Recommendations

1. **Lower threshold for high-value customers** (monetary top 20%) to reduce FN risk. Our VIPs (like CUST02072) need a separate logic because high historical frequency blinds the model.
2. **Add text-sentiment features** from support ticket descriptions to catch silent dissatisfaction (like CUST00938).
3. **Refine return logic**: Differentiate between exchanges and actual refunds to prevent false positives (like CUST01017).
4. **Schedule monthly error reviews** to catch concept drift.
