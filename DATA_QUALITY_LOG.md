# Data Quality Log - Lab 1

## Issue 1: grid - Invalid range value
- **What:** There are 4 rows with `grid = 0`, while valid starting positions should be 1 to 20.
- **Classification:** Type error / Invalid value
- **Impact:** Baselines and models that rely on `grid` (like `grid <= 10`) can produce biased predictions if impossible values are treated as real positions.
- **Decision:** Flag and convert `grid = 0` to missing (`NaN`), then handle by drop/impute in the modeling pipeline.
- **Justification:** `grid = 0` is not a valid race start slot; treating it as a true position would distort signal.

## Issue 2: positionText - Mixed encoding (numeric + categorical codes)
- **What:** `positionText` contains non-numeric codes (for example `R` and `W`) in 35 rows, while other rows are numeric-like.
- **Classification:** Type error / Mixed data type
- **Impact:** Numeric conversion can fail or create unintended missing values; aggregations become inconsistent.
- **Decision:** Keep raw `positionText`, create a separate cleaned numeric `position` field, and add a categorical flag for non-finish codes.
- **Justification:** `positionText` carries meaningful outcome information (retired/withdrawn) that should not be lost.

## Issue 3: status - Inconsistent categorical granularity
- **What:** `status` has 18 unique labels, mixing broad outcomes (`Finished`, `Lapped`, `Retired`) with specific failure causes (`Gearbox`, `Cooling system`, etc.).
- **Classification:** Categorical quality issue / Label standardization
- **Impact:** Sparse categories fragment analysis and create unstable or noisy feature representations.
- **Decision:** Map `status` into a smaller standardized taxonomy (for example: Finished, Lapped, Mechanical DNF, Crash DNF, Other DNF).
- **Justification:** Standardized labels improve interpretability and reduce variance while preserving event meaning.

## Issue 4: finished - Survivorship filtering risk
- **What:** The grid-top10 relationship changes when filtering to `finished == True` versus using all rows (Spearman approximately `-0.559` vs `-0.527`).
- **Classification:** MNAR / Selection bias (survivorship bias)
- **Impact:** Finishers-only analysis can bias estimated feature-target relationships and misrepresent real-world predictability.
- **Decision:** Keep all rows in core analysis and include finish-status logic explicitly; avoid finishers-only filtering without justification.
- **Justification:** Non-finish outcomes are informative and are not missing completely at random for race performance.

## Issue 5: points and position - Post-race leakage risk
- **What:** `points` and `position` are post-race variables but appear in candidate-feature exploration for Top-10 prediction.
- **Classification:** Leakage risk / Temporal validity issue
- **Impact:** Using post-race fields in a pre-race model inflates validation metrics and makes the model non-deployable.
- **Decision:** Exclude post-race columns (`position`, `points`, `status`, `top10`) from pre-race feature sets.
- **Justification:** Only pre-race available information should be used for fair, deployable prediction.
