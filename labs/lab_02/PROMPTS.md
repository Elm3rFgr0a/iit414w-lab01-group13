# PROMPTS Log

## Entry 1 — 2026-03-22 — Feature Engineering (Cell 5)

**Context:** We needed to engineer features from domain knowledge (lag, rolling, categorical) without data leakage.
**Prompt(s):** "Create code to add `constructor_tier` mapped from team names (Mercedes/RB=1, etc), a lag feature `prev_race_position` using shift(1), and a rolling feature `avg_pos_last_3` ensuring no current race data is used. Explain the leakage prevention."
**Output:** The AI provided pandas code grouping by driver, using `.shift(1)` for the lag feature, and `.shift(1).rolling(3)` for the rolling average to strictly exclude the current race. It also included a dictionary for constructor tiers.
**Validation:** We verified the `.shift(1)` logic correctly introduces NaNs for the first race (handled by filling with `grid`) and that the rolling window does not include variable `position` from the current row.
**Adaptations:** We adjusted the tier dictionary slightly to better reflect 2021/2022 performance tiers (e.g., putting Ferrari in Tier 1).
**Final Decision:** Used — the code correctly implements the features while passing the leakage guard checklist.

## Entry 2 — 2026-03-22 — Model Training (Cell 7)

**Context:** We needed to train a simple, interpretable model to beat the Lab 1 baseline.
**Prompt(s):** "Train a Decision Tree Classifier on the engineered features (`grid`, `constructor_tier`, `prev_race_position`, `avg_pos_last_3`). Use a temporal split (Train=2022, Val=2023). Set `max_depth=5` to prevent overfitting and `random_state=414` for reproducibility."
**Output:** The AI generated code to split the data temporally, initialize `DecisionTreeClassifier`, fit it on the 2022 training set, and predict on the 2023 validation set.
**Validation:** We inspected the shapes of `X_train` and `X_val` to ensure the split guideline was followed and confirmed the model fit without errors.
**Adaptations:** None needed; the parameters were exactly as requested.
**Final Decision:** Used — this provided the core model for Lab 2 validation.

## Entry 3 — 2026-03-22 — Evaluation Metrics (Cell 8)

**Context:** We needed a comprehensive comparison of the new model against the Lab 1 baselines (Majority Class and Domain Heuristic).
**Prompt(s):** "Calculate Accuracy, Precision, Recall, F1, and ROC-AUC for: 1) Majority Class, 2) Lab 1 Heuristic (grid<=10), and 3) The new Decision Tree. Print them in a formatted comparison table."
**Output:** The AI produced a script using `sklearn.metrics` to compute all five metrics for the three approaches and printed a clean markdown-compatible text table.
**Validation:** We ran the cell and checked that the Majority Class baseline had ~0.5 accuracy/0.0 F1 and the Heuristic baseline matched the Lab 1 report (~0.73 F1), confirming the metric calculations were correct.
**Adaptations:** Integrated the provided code directly into the notebook to replace the placeholder evaluation.
**Final Decision:** Used — this table became the basis for `comparison_table.md`.

## Entry 4 — 2026-03-22 — Error Analysis (Cell 10)

**Context:** We needed to understand *why* the model makes mistakes to guide future improvements.
**Prompt(s):** "Identify False Positives and False Negatives from the validation predictions. Print the top 5 examples of each, showing race, driver, grid, and finished status. specifically check how many False Positives were DNFs (Did Not Finish)."
**Output:** The code created `fp` and `fn` dataframes by filtering the validation set and calculated the percentage of False Positives that were DNFs.
**Validation:** The output revealed that a significant portion of False Positives were indeed DNFs (crashes/engine failures), which accurate pre-race features cannot easily predict.
**Adaptations:** Added print statements to explicitly label these "Failure Modes" for the report.
**Final Decision:** Used — this analysis provided the "Failure Modes" section of the notebook.
