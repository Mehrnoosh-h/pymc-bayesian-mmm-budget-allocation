📓 **Notebook:** [MMM_Budget_Allocation.ipynb](notebooks/MMM_Budget_Allocation.ipynb)

This repository is a portfolio-style implementation of a **Bayesian Marketing Mix Model (MMM)** using **PyMC-Marketing**. It follows the official MMM budget-allocation workflow end-to-end: exploratory data checks, Bayesian model fitting, posterior diagnostics, contribution/ROAS interpretation, and **budget allocation optimization**.

---

## What this project does

### Goals
- Model the relationship between **weekly sales** and:
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

This project uses the **multidimensional mock dataset** from PyMC-Marketing.  
If the notebook raises a `FileNotFoundError` for `data/multidimensional_mock_data.csv`, use one of these approaches:

### Option 1 — Load from the PyMC-Marketing packaged data directory (recommended)
```python
from pymc_marketing.paths import data_dir
import pandas as pd

data_path = data_dir / "multidimensional_mock_data.csv"
df = pd.read_csv(data_path, parse_dates=["date"], index_col=0)

---

## How to run

### Option A — pip + venv
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

### Out-of-sample performance
- **CRPS** is computed in the notebook for the holdout window (see the Evaluation section output).
- Out-of-sample **posterior predictive** plots are used to visually assess fit and uncertainty calibration.

### Channel efficiency and allocation insights
- The fitted response curves show **diminishing returns (saturation)** and different **marginal ROAS** across channels.
- Under realistic business constraints (per-channel upper/lower bounds), the optimizer recommends **rebalancing spend** away from channels with lower marginal return and toward channels with higher remaining marginal return.

### Optimization outcome (what the plots show)
- **Aggregated channel contributions (media-only uplift):**
  - Expected improvement of **~+11%** (mean ≈ **0.11**)
  - **~90.7%** probability that uplift is **> 0**
  - 94% HDI roughly **-3.8% to +26%**
- **Total sales uplift (including intercept + seasonality):**
  - Uplift is near zero (mean ≈ **-0.0027**)
  - Probability of improvement is close to a coin flip (**~52.4% > 0**)
  - 94% HDI approximately **-27% to +25%**

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
