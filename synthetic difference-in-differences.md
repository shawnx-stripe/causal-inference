---
title: Synthetic difference-in-differences
aliases:
  - synth-DiD
  - SDID
  - synthetic DiD
tags:
  - causal-inference
  - synthetic-control
  - did
  - panel-data
updated: 2026-06-17
---

# Synthetic difference-in-differences

> [!summary]
> Synthetic DiD combines [[Synthetic Control|SC unit weights]] with time weights to produce a doubly-weighted DiD estimator. Unit weights make the control group match the treated unit's trajectory; time weights make pre-treatment periods informative of the post-treatment counterfactual. More robust than either DiD or SC alone.

> [!tip] Sources
> - [09-Synthetic-Control.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/09-Synthetic-Control.ipynb) — "Synthetic Difference-in-Differences" section
> - [Ch 25: Synthetic Difference-in-Differences](https://matheusfacure.github.io/python-causality-handbook/25-Synthetic-Diff-in-Diff.html)
> - Data: `online_mkt.csv` — same as SC but combines unit weights with time weights estimated from transposed SC problem

## Core idea

Two sets of weights:
1. **Unit weights** ($\hat{w}$) — from standard [[Synthetic Control]] (fit control units to treated pre-treatment)
2. **Time weights** ($\hat{\lambda}$) — from a *transposed* SC problem (fit pre-treatment periods to approximate post-treatment control means)

Final estimate: weighted DiD regression using both weight vectors.

## Time weights estimation

Solve a transposed SC problem — control periods weighted to match post-treatment control outcome level:
$$
\min_\lambda \| Y_{\text{pre}}^{\text{control}T} \cdot \lambda - \bar{Y}_{\text{post}}^{\text{control}} \|^2 \quad \text{s.t.} \quad \sum \lambda_t = 1, \; \lambda_t \geq 0
$$

(Intercept can be included to absorb level shifts.)

## Combined estimator

Run weighted least squares with observation weight = unit_weight × time_weight:

$$
Y_{it} = \alpha + \beta \cdot \text{treated}_i \times \text{post}_t + \gamma_i + \delta_t + \varepsilon_{it}
$$

with WLS weight $= \hat{w}_i \cdot \hat{\lambda}_t$.

## Code

```python
import statsmodels.formula.api as smf
import pandas as pd

# Assume unit_w and time_w are DataFrames with weights
panel = panel.merge(unit_w, on="city").merge(time_w, on="date")
panel["weight"] = panel["unit_weight"] * panel["time_weight"]

# Synthetic DiD as weighted OLS
sdid = smf.wls(
    "downloads ~ treated * post",
    data=panel,
    weights=panel["weight"]
).fit()

att_sdid = sdid.params["treated:post"]
```

## L2 regularization (SDID-specific)

Unlike standard SC, SDID adds an L2 penalty to the unit weight optimization:
$$
\min_{w_0, w} \|\bar{Y}_{\text{pre,tr}} - (Y_{\text{pre,co}} \cdot w + w_0)\|^2 + \zeta^2 T_{\text{pre}} \|w\|_2^2
$$

subject to $\sum w_i = 1$, $w_i \geq 0$.

The regularization parameter:
$$
\zeta = (N_{\text{tr}} \cdot T_{\text{post}})^{1/4} \cdot \sigma(\Delta_{it})
$$

where $\Delta_{it} = Y_{it} - Y_{i,t-1}$ is the first difference of control outcomes. The intercept $w_0$ allows level differences (unlike standard SC which requires level matching).

## Placebo SE estimation

Since WLS standard errors don't account for weight estimation uncertainty, inference uses placebo permutation:

```python
from joblib import Parallel, delayed

def make_placebo(data, state_col, treat_col):
    control = data.query(f"~{treat_col}")
    placebo_state = np.random.choice(control[state_col].unique())
    return control.assign(**{treat_col: control[state_col] == placebo_state})

effects = Parallel(n_jobs=4)(
    delayed(sdid_estimate)(make_placebo(df, "city", "treated"))
    for _ in range(400)
)
se = np.std(effects)
```

## Why SDID improves on SC and DiD

- SC alone: can fail if pre-treatment fit is poor (biased counterfactual)
- DiD alone: requires strict [[parallel trends]] everywhere
- SDID: unit weights handle cross-sectional heterogeneity; time weights handle time-series non-stationarity. Doubly robust in the sense that it works if *either* set of weights is correct.

## Related notes

- [[Synthetic Control]]
- [[Difference-in-Differences (DiD)]]
- [[Panel Data and DiD (MOC)]]
