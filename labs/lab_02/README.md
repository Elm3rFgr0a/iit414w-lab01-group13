# iit414w-lab01-group13 - Reproducibility Runbook

| Field | Value |
|---|---|
| **Full name** | Benjamin Ignacio Pinto Faúndez & Adrean David Torres Fonseca |
| **GitHub username** | Elm3rFgr0a & Mrnooqmal|
| **Course** | IIT414W |
| **Date** | March 11, 2026 |

---
  
## System info

| Property | Value |
|---|---|
| Operating system | Windows 11 (Build 22631) |
| Python version | 3.10 (managed by conda) |
| Conda version | 24.1.1 |

---

# Lab 2: Feature Engineering

This lab extends the baseline from Lab 1 by introducing engineered features based on F1 domain knowledge.

## How to Reproduce

1.  Ensure you have run the Lab 1 `baseline.ipynb` to generate the processed data `data/processed/results_2022_2024.csv`.
2.  Open `lab02_feature_engineering.ipynb`.
3.  Run all cells.

## Features Added

1.  `prev_feature_position`: Finishing position in the previous race (Lag).
    *   *Justification:* Recent form is a strong predictor of future performance.
2.  `avg_pos_last_3`: Average finishing position in the last 3 races (Rolling).
    *   *Justification:* Smoothes out single-race variance to capture underlying pace.
3.  `constructor_tier`: Tiered ranking (1-3) of constructors based on prior season performance (Categorical).
    *   *Justification:* Car performance is the dominant factor in F1; knowing the car's tier sets the baseline expectation.

## Results

See `comparison_table.md` for a side-by-side comparison with Lab 1.


## Setup instructions

Follow these steps in order. Commands are written for **Anaconda Prompt** or any terminal where `conda` is available.

**1. Clone the repository (if you haven't already)**
```bash
git clone https://github.com/Elm3rFgr0a/iit414w-lab01-group13
cd iit414w-lab01-group13
```

**2. Create the conda environment from the provided file**
```bash
conda env create -f environment.yml
```
This installs Python 3.10, JupyterLab, NumPy, pandas, scikit-learn, matplotlib, seaborn, requests, FastF1, and ipykernel — all at pinned versions.

**3. Activate the environment**
```bash
conda activate iit414w
```
You should see `(iit414w)` in your prompt. All subsequent commands must be run inside this environment.

**4. Register the kernel with Jupyter (one-time setup)**
```bash
python -m ipykernel install --user --name iit414w --display-name "Python (iit414w)"
```

**5. Launch JupyterLab**
```bash
jupyter lab
```

---

## Run the notebooks with this environment

Use the same activated environment (`iit414w`) to run both notebooks:

```bash
conda activate iit414w
jupyter lab02_feature_engineering.ipynb
```

Then run the notebook cells in order:

1. Open `lab02_feature_engineering.ipynb` and execute all cells (`Run All`).

If Jupyter asks for a kernel, select **Python (iit414w)**.

---

Lab 2 (feature engineering) specific notes:

- Open [lab02_feature_engineering.ipynb](lab02_feature_engineering.ipynb) and run all cells. The notebook expects the processed CSV `data/processed/results_2022_2024.csv` to exist (created by Lab 1 notebook). If it is missing, run `labs/lab_01/baseline.ipynb` to fetch and cache the data first.

- After running, update `comparison_table.md` with the Decision Tree validation F1 reported by the notebook to complete the comparison.