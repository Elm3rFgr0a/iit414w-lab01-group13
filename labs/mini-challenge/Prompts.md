# Prompts and LLM Interactions

## Interaction 1

**Context:** Creating a rolling historical feature capturing a team's average pit stop duration from prior races to use as a predictive feature.
**Prompt:** "Create a historical_team_avg_duration feature using only stops from prior races. Calculate the mean pit_stop_duration by Team, season, and round, and use expanding().mean() with a shift(1) pattern to avoid data leakage."
**Output:** The AI provided the GroupBy logic to first aggregate the race means per team, then applying an expanding mean with `shift(1)`, and finally merging it back into the main DataFrame via a left join.
**Validation:** I manually reviewed the shifted values in the test set to confirm that the `shift(1)` logic successfully pushed previous race averages forward without incorporating the current race's times, verifying temporal safety.
**Adaptations:** Minor adjustments to ensure sort order by `Team`, `season`, and `round` was perfectly preserved before computing the expanding mean so that the shifting accurately reflected chronological constraints.
**Final Decision:** Implemented the historical feature as suggested, maintaining proper race-granularity shifting.

## Interaction 2

**Context:** Selecting features and configuring `scikit-learn`'s `ColumnTransformer` for robust categorical and numerical data preprocessing.
**Prompt:** "Set up `X_train` and `X_test` using the following features: 'circuit', 'Team', 'TyreLife', 'LapNumber', 'Stint', 'Compound', and 'historical_team_avg_duration'. Create a `ColumnTransformer` using `SimpleImputer` for numericals and `OneHotEncoder` with `SimpleImputer` for categoricals."
**Output:** The AI created separated `cat_feats` and `num_feats` lists, wrapping them in a `ColumnTransformer` with `SimpleImputer(strategy='mean')` and a categorical pipeline dealing with missing/unknown categories.
**Validation:** I verified that the selected features mapped perfectly to our leakage check, explicitly making sure target variables and `next_compound` were excluded.
**Adaptations:** Made sure `handle_unknown='ignore'` was provided to the `OneHotEncoder` to prevent missing categorical failures on the test split.
**Final Decision:** Used the AI-generated preprocessing transformer mapping the 7 safe variables securely into the pipeline.

## Interaction 3

**Context:** Training and predicting using `Ridge` and `Random Forest` regressors against the transformed feature space.
**Prompt:** "Apply Ridge Regression and Random Forest inside an sklearn Pipeline based on the ColumnTransformer. Fit on `X_train`, predict on `X_test`, and print the Test MAE using `RANDOM_SEED = 414`."
**Output:** Produced code for two distinct `sklearn.pipeline.Pipeline` objects, correctly utilizing the preprocessor step, running `.fit()` and `.predict()`, and computing the `mean_absolute_error`.
**Validation:** Examined the `RandomForestRegressor` settings to ensure it used the required `RANDOM_SEED` and checked that Ridge Regression correctly implemented the scaler beforehand.
**Adaptations:** Added a `StandardScaler(with_mean=False)` step explicitly in the Ridge pipeline to normalize inputs while avoiding density issues with sparse categorical matrices produced by the OHE.
**Final Decision:** Used the dual-pipeline approach, evaluating Ridge Regression alongside a Random Forest model (`n_estimators=100`, `max_depth=10`).

## Interaction 4

**Context:** Compiling performance metrics into an easy-to-read tabular format covering baselines and model predictions.
**Prompt:** "Create a comparison table in pandas combining the MAEs of Naive mean, Team mean, Ridge Regression, and Random Forest models."
**Output:** The model generated a dictionary logic merging the earlier baseline MAE variables and the pipeline MAE variables, outputting them into a clean `pd.DataFrame`.
**Validation:** Confirmed that the variables referenced by the tabular block actually matched the variables assigned earlier in the baselines cell.
**Adaptations:** Applied `.sort_values(by='MAE')` directly on the final DataFrame to order the validation results cleanly from best to worst performance, adding a display header.
**Final Decision:** Appended the comparison table output at the end of the script for high readability.
