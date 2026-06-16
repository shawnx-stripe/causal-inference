---
title: Synthetic Control
aliases:
  - SC
  - SCM
  - synthetic control method
tags:
  - causal-inference
  - synthetic-control
  - panel-data
updated: 2026-06-17
---

# Synthetic Control

> [!summary]
> Synthetic control constructs a weighted combination of untreated units that best approximates the treated unit's pre-treatment trajectory. The post-treatment gap between the treated unit and its synthetic counterfactual estimates the causal effect. Ideal when few units are treated and [[parallel trends]] is too strong.

> [!tip] Sources
> - [09-Synthetic-Control.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/09-Synthetic-Control.ipynb)
> - [Ch 15: Synthetic Control](https://matheusfacure.github.io/python-causality-handbook/15-Synthetic-Control.html)
> - [Appendix: Conformal Inference for Synthetic Controls](https://matheusfacure.github.io/python-causality-handbook/Conformal-Inference-for-Synthetic-Control.html)
> - Data: `online_mkt.csv` (50 Brazilian cities, 3 treated starting 2022-05-01 with 40-60% lifts), `online_mkt_cov.csv` (adds competitor downloads as covariate)

## Core idea

Find non-negative weights $w$ summing to 1 such that the weighted control units match the treated unit pre-treatment:
$$
\min_w \| Y_{\text{pre}}^{\text{control}} \cdot w - Y_{\text{pre}}^{\text{treated}} \|^2 \quad \text{s.t.} \quad \sum w_j = 1, \; w_j \geq 0
$$

Post-treatment effect:
$$
\hat{\tau}_t = Y_t^{\text{treated}} - Y_t^{\text{control}} \cdot \hat{w}
$$

## Estimation approaches

1. **Horizontal regression** (unconstrained) — OLS without intercept on transposed data. Intuition builder but can overfit.
2. **Canonical SC** (Abadie et al.) — Convex-constrained optimization via `cvxpy`.
3. **With covariates** — Nested optimization: outer loop finds variable importance weights $v$, inner loop finds unit weights $w$.
4. **Debiased SC** — Cross-validation on pre-treatment blocks estimates and subtracts bias.

## Inference

K-fold SE from block cross-validation:
$$
\text{SE} = \sqrt{1 + \frac{K \cdot \text{block\_size}}{T_{\text{post}}}} \cdot \frac{\text{sd}(\hat{\tau}_k)}{\sqrt{K}}
$$

CI via $t$-distribution with $K-1$ degrees of freedom.

## Code

```python
import cvxpy as cp
import numpy as np

def synthetic_control(y_pre_control, y_pre_treated):
    """Canonical SC: constrained convex optimization."""
    n_donors = y_pre_control.shape[1]
    w = cp.Variable(n_donors)
    objective = cp.Minimize(cp.sum_squares(y_pre_control @ w - y_pre_treated))
    constraints = [cp.sum(w) == 1, w >= 0]
    problem = cp.Problem(objective, constraints)
    problem.solve()
    return w.value

# Fit SC
w_hat = synthetic_control(Y_pre_co, y_pre_tr)

# Post-treatment effect
y0_hat = Y_post_co @ w_hat
effect = y_post_tr - y0_hat
```

## Related notes

- [[Difference-in-Differences (DiD)]]
- [[synthetic difference-in-differences]]
- [[geo experiment]]
- [[Panel Data and DiD (MOC)]]
