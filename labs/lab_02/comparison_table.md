# Lab 1 vs Lab 2 — Comparison Table
## Benjamin Ignacio Pinto Faúndez & Adrean David Torres Fonseca

| Model / Baseline | Accuracy | Precision | Recall | F1 | ROC-AUC |
|------------------------|----------|-----------|--------|-------|---------|
| Majority class (Lab 1) | 0.5000 | 0.0000 | 0.0000 | 0.0000 | 0.5000 |
| Domain heuristic (Lab 1)| 0.7295 | 0.7265 | 0.7364 | 0.7314 | 0.7295 |
| Lab 2 model (Decision Tree) | 0.7614 | 0.7805 | 0.7273 | 0.7529 | 0.7676 |

## Primary metric: F1-score
(Chosen in Lab 1 because the dataset is balanced, but false positives and false negatives both have significant decision costs).

## Interpretation
The Decision Tree model achieved an F1-score of **0.7529**, beating the domain heuristic baseline (0.7314) by approximately **2.1 percentage points**. It also improved **Precision by ~5.4%** (0.78 vs 0.72) while slightly sacrificing Recall (-0.9%). This suggests the engineered features (specifically `constructor_tier` and rolling averages) successfully help filter out "false alarms"—drivers who qualify well (low grid) but are in slower cars or have poor recent form and thus drop out of the points. The model's higher ROC-AUC (0.7676 vs 0.7295) confirms it has better overall ranking capability than the simple grid rule.
