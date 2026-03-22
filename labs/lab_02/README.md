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
jupyter lab baseline.ipynb eda.ipynb
```

Then run the notebook cells in order:

1. Open `eda.ipynb` and execute all cells (`Run All`).
2. Open  `baseline.ipynb` and execute all cells (`Run All`).

If Jupyter asks for a kernel, select **Python (iit414w)**.