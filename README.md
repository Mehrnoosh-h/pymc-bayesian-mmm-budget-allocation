📓 **Notebook:** [MMM_Budget_Allocation.ipynb](notebooks/MMM_Budget_Allocation.ipynb)

This repository is a portfolio-style implementation of a **Bayesian Marketing Mix Model (MMM)** using **PyMC-Marketing**. It follows the official MMM budget-allocation workflow end-to-end: exploratory data checks, Bayesian model fitting, posterior diagnostics, contribution/ROAS interpretation, and **budget allocation optimization**.

---

## What this project does

### Goals
- Model the relationship between weekly sales and:
  - **Paid media channels** (spend)
  - **Control variables** (events, trend/time index, etc.)
  - **Seasonality** (yearly seasonality terms)
- Quantify:
  - **Channel contributions** over time
  - **Diminishing returns (saturation)** per channel
  - **ROAS** with Bayesian uncertainty
- Use the fitted model to run:
  - **Budget optimization / allocation** and compare against a baseline (“naive”) allocation

---

## Key outputs 

### Data & split
- Sales time series
- Total media spend + per-channel spend
- Train/test split visualization

### Bayesian checks
- Prior predictive checks
- Trace (sampling diagnostics)
- Posterior predictive checks + error plots

### Interpretation
- Components/contribution decomposition
- Saturation / response curves
- Channel contribution share
- ROAS and ROAS vs contribution share

### Performance & generalization
- Out-of-sample predictions
- CRPS score
- Out-of-sample channel contributions

### Optimization
- Budget allocation comparison (original vs optimized)
- Uplift in contributions after optimization
- Optimized vs naive sales comparison

---

## Repository structure
```
.
├── notebooks/
│   └── MMM_Budget_Allocation.ipynb
├── data/
│   └── (optional) dataset files if included
├── results/
│   └── (optional) exported figures/tables
└── README.md
```

---

## Data notes

This notebook uses the **multidimensional mock dataset** shipped with PyMC-Marketing (same as the official example).
If you see `FileNotFoundError: data/multidimensional_mock_data.csv`, it usually means the notebook is looking for a local `./data/` folder.
Recommended approach: load the packaged dataset directly from PyMC-Marketing.

```python
from pymc_marketing.paths import data_dir as pmcm_data_dir
import pandas as pd

data_path = pmcm_data_dir / "multidimensional_mock_data.csv"
df = pd.read_csv(data_path, parse_dates=["date"], index_col=0)
```

### Optional
Add a quick check:
```python
print(data_path)
print(data_path.exists())
```
---

## How to run

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

pip install -U pip
pip install pymc-marketing arviz numpy pandas matplotlib jupyter
```

### Launch notebook
```bash
jupyter lab
# or
jupyter notebook
```

Open: `notebooks/mmm_case_study.ipynb`

---

## Modeling choices
- **Bayesian regression backbone** with explicit uncertainty for parameters and predictions
- **Adstock** to capture carryover effects of advertising over time
- **Saturation** to capture diminishing returns as spend increases
- **Seasonality** to capture repeated yearly patterns
- **Controls** to isolate non-media effects (events/trend proxies)

---

## Results summary

- **Out-of-sample evaluation:** Posterior predictive checks show the model tracks the holdout trend reasonably well; CRPS is reported in the notebook evaluation section.
- **Optimization (media-only uplift):** The constrained optimizer improves aggregate channel contribution; the posterior uplift distribution has an expected increase of ~+11% with ~90.7% probability of being > 0 (94% HDI ≈ [-3.8%, +26%]).
- **Total sales uplift:** When baseline + seasonality are included, the uplift is near zero (mean ≈ -0.27%) with wide uncertainty (94% HDI ≈ [-27%, +25%]) and ~52% probability of being > 0.
- **Allocation shifts:** Budget is rebalanced away from lower marginal-return channels and toward channels with higher remaining marginal return, subject to per-channel spend bounds (±50%).


**Interpretation:** The optimized plan improves **media-driven contribution efficiency** more clearly than **total sales** in the holdout window, since total sales variability is often dominated by **baseline (intercept)** and **seasonality**, and practical constraints limit how aggressively budgets can shift.

### Practical takeaways
- **Constraints matter:** Per-channel bounds (e.g., ±50% around baseline spend) prevent unrealistic “all-in” allocations and align recommendations with stakeholder expectations.
- **Use the right KPI for uplift:** Media-only contribution uplift can be a cleaner signal of optimization gains when baseline/seasonality dominate total variance.
- **Uncertainty is a feature:** Posterior distributions enable probability statements (e.g., **P(uplift > 0)**) rather than single-point claims.
---

## Tech stack
- Python
- PyMC-Marketing (MMM), PyMC, ArviZ
- Pandas / NumPy
- Matplotlib

---
## References

This implementation follows the structure and ideas from the official PyMC-Marketing MMM budget allocation materials:
PyMC-Marketing documentation + MMM budget allocation example notebook:
https://www.pymc-marketing.io/en/stable/notebooks/mmm/mmm_budget_allocation_example.html

---
## License
- MIT 
:contentReference[oaicite:0]{index=0}
::contentReference[oaicite:1]{index=1}
