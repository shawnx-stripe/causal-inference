---
title: Geo experiment
aliases:
  - geo-experiment
  - geographic experiment
  - geo test
tags:
  - causal-inference
  - experimentation
  - synthetic-control
updated: 2026-06-17
---

# Geo experiment

> [!summary]
> Geo experiments use geographic units (cities, regions) as randomization units instead of individual users, addressing interference/spillover within markets. Because few geo units are typically available, power is limited — [[Synthetic Control]] methods are used to optimize treatment/control assignment and estimate effects.

> [!tip] Sources
> - [10-Geo-and-Switchback-Experiments.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/10-Geo-and-Switchback-Experiments.ipynb)
> - [Ch 15: Synthetic Control](https://matheusfacure.github.io/python-causality-handbook/15-Synthetic-Control.html)
> - Data: `online_mkt.csv` (50 Brazilian cities, population-weighted)

## Why geo-level randomization

- Individual randomization → interference (users in same city interact)
- Geo randomization → cities are independent units
- Problem: few geos available → severely underpowered for traditional statistics

## Power problem

Standard power formula: $n = \lceil \sigma^2 \cdot 16 / \delta^2 \rceil$

With city-level variance and 5% MDE, typical requirement is ~36K geo units — far more than the ~50 available.

## SC-based design optimization

Instead of random geo assignment, select treatment/control sets to minimize pre-treatment SC fit error:

```python
import cvxpy as cp
import numpy as np

class SyntheticControl:
    def __init__(self, fit_intercept=True):
        self.fit_intercept = fit_intercept

    def fit(self, X, y):
        n = X.shape[1]
        if self.fit_intercept:
            X = np.column_stack([np.ones(X.shape[0]), X])
            n += 1
        w = cp.Variable(n)
        constraints = [cp.sum(w[int(self.fit_intercept):]) == 1,
                      w[int(self.fit_intercept):] >= 0]
        objective = cp.Minimize(cp.sum_squares(X @ w - y))
        cp.Problem(objective, constraints).solve()
        self.w_ = w.value
        return self

# Evaluate a candidate treatment set
def get_sc_loss(treatment_geos, df_sc, y_mean_pre):
    """Fit SC for treatment geos, return fit loss."""
    control_geos = [g for g in df_sc.columns if g not in treatment_geos]
    sc = SyntheticControl(fit_intercept=True)
    sc.fit(df_sc[control_geos].values, df_sc[treatment_geos].mean(axis=1).values)
    y_hat = df_sc[control_geos].values @ sc.w_[1:] + sc.w_[0]
    return np.sum((y_hat - df_sc[treatment_geos].mean(axis=1).values)**2)
```

## Population fraction weighting

Cities have very different sizes (São Paulo dominates). Weight by population fraction:
$$
f_i = \frac{\text{population}_i}{\sum_j \text{population}_j}
$$

## Related notes

- [[Synthetic Control]]
- [[switchback experiment]]
- [[Regression Discontinuity Design (RDD)]]
- [[Design-Based Identification (MOC)]]
