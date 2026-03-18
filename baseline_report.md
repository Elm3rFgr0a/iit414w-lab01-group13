## Baseline Report — iit414w-lab01-group13

## 1. Prediction target
Binary classification — predict whether a driver finishes in the Top 10 for a given race (`top10`) using pre-race information (one row per driver-race).

## 2. Majority-class baseline
- Metric: Accuracy
- Value: ~0.51 (always-predict-Top10; observed ~51.3% in the course examples and lab notebooks)
- Code: `DummyClassifier(strategy='most_frequent', random_state=414)`

## 3. Domain heuristic baseline
- Rule: Predict `Top10 = True` if `grid <= 10` (use only pre-race `grid` position).
- Metrics (validation, season 2023):
    - Accuracy: ~0.73–0.74 (heuristic reached ~74% on the validation set in the lab notebook)
    - Precision: ~76.1%
    - Recall: ~72.4%
    - F1-score: ~0.731
- Code cell (from `baseline.ipynb`):

```python
val_df['pred_top10'] = val_df['grid'] <= 10
accuracy = (val_df['pred_top10'] == val_df['top10']).mean()
from sklearn.metrics import precision_score, recall_score, f1_score
precision = precision_score(val_df['top10'], val_df['pred_top10'])
recall = recall_score(val_df['top10'], val_df['pred_top10'])
f1 = f1_score(val_df['top10'], val_df['pred_top10'])
```

## 4. Metric choice + justification
I chose F1-score as the primary metric. The dataset's `top10` target is roughly balanced (~51% positive), so accuracy alone can be misleading; the domain heuristic shows high accuracy but we must ensure the model balances precision and recall. False positives (predicting Top 10 when the driver does not finish Top 10) produce wasted attention on unlikely drivers, while false negatives (missing actual Top 10 finishers) may miss valuable opportunities — both errors have real decision costs. F1 balances these trade-offs and provides a single, informative summary while we explore whether precision- or recall-weighted objectives are warranted.

## 5. Leakage guard item
- Checklist item: "No test-set data used in preprocessing or baseline design" (LAB01 instructions).
- What I found: The notebook uses a temporal split by season (`train=2022`, `val=2023`, `test=2024`) and builds the heuristic from pre-race `grid` only; preprocessing (`dropna` on `grid`) is applied per-split. This split preserves temporal separation and avoids using future data during baseline construction.
- Fix required: None — temporal split and per-split preprocessing prevent test leakage.

## 6. Baseline comparison & interpretation
(a) Harder baseline: The domain heuristic (`grid <= 10`) is substantially harder to beat than the majority-class baseline. The ~24 percentage-point gap between always-predict-Top10 (~51%) and the heuristic (~74%) shows that `grid` is a very informative single feature; beating the heuristic requires meaningful feature engineering or modeling beyond a single threshold.

(b) If an ML model later scores below the domain heuristic baseline, I would conclude the model is not adding value over a simple pre-race rule. Next steps would be: verify the validation procedure (temporal split and no leakage), inspect feature engineering (add circuit, constructor, driver history, or interaction features), try stronger models or calibration, and consider circuit-specific heuristics or ensemble approaches.

---

Sources: `iit414w-lab01-group13/baseline.ipynb`, course lab guide and examples in `IIT414W-2026-1T/unit_I/W01_Wed_f1_data_ecosystem.ipynb` and `IIT414W-2026-1T/unit_I/week_03/W03_Wed_baselines_and_metrics.ipynb`.
