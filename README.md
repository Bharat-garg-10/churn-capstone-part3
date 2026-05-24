# Part 3: Churn Prediction Model & Model Card
## D2C Customer Churn Capstone

### Overview
Binary classification model to predict D2C customer churn in the next 60 days,
with full model evaluation, interpretability, error analysis, and model card.
An XGBoost classifier was chosen over a Baseline Logistic Regression due to superior handling of non-linear patterns.

### Setup
```bash
pip install -r requirements.txt
```

### Data
Download from: `https://drive.google.com/drive/folders/1PmLapJI1VSDgvl_AxARNKwM1MCd3WFX0`
Place files in `data/`.

### How to Run
1. Install: `pip install -r requirements.txt`
2. Add data to `data/`
3. Run `churn_model.ipynb` top to bottom
4. Outputs: `model.pkl`, `metrics.json`, charts

### Load the Saved Model
```python
import joblib, pandas as pd
model = joblib.load('model.pkl')
# Assuming X has the exact features generated from the prep cell
X = pd.DataFrame([{...your features...}])
prob = model.predict_proba(X)[0][1]
```

### Key Files
| File | Description |
|---|---|
| `churn_model.ipynb` | Full modeling workflow |
| `model.pkl` | Trained XGBoost model (joblib) |
| `metrics.json` | ROC-AUC, F1, confusion matrix |
| `error_analysis.md` | FP/FN analysis with 10 exact case studies |
| `model_card.md` | Structured model documentation using production metrics |