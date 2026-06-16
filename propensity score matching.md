---
title: Propensity score matching
aliases:
  - PS matching
  - PSM
  - nearest-neighbor matching
tags:
  - causal-inference
  - propensity-score
  - matching
updated: 2026-06-17
---

# Propensity score matching

> [!summary]
> Match each treated unit to the control unit with the most similar [[propensity score]], then estimate the ATE as the average difference in outcomes between matched pairs. Simple but can be high-variance with poor overlap.

> [!tip] Sources
> - [05-Propensity-Score.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/05-Propensity-Score.ipynb) — "Propensity Score Matching" section
> - [Ch 10: Matching](https://matheusfacure.github.io/python-causality-handbook/10-Matching.html)
> - Data: `management_training.csv` — 1-NN matching gives ATE≈0.29

## Core idea

For each treated unit $i$, find the nearest control unit $j$ in PS space:
$$
j(i) = \arg\min_{k: T_k=0} |\hat{e}(X_i) - \hat{e}(X_k)|
$$

ATE from matched pairs:
$$
\widehat{\text{ATE}} = \frac{1}{n} \sum_i \left[ T_i(Y_i - Y_{j(i)}) + (1-T_i)(Y_{j(i)} - Y_i) \right]
$$

## Code

```python
from sklearn.neighbors import KNeighborsRegressor
from sklearn.linear_model import LogisticRegression
import numpy as np

# Estimate propensity scores
ps_model = LogisticRegression(penalty="none", max_iter=1000)
ps_model.fit(data[X], data[T])
data["ps"] = ps_model.predict_proba(data[X])[:, 1]

# 1-NN matching on propensity score
nn = KNeighborsRegressor(n_neighbors=1)

# Match treated → find counterfactual from controls
nn.fit(data.query(f"{T}==0")[["ps"]], data.query(f"{T}==0")[Y])
matched_y0 = nn.predict(data.query(f"{T}==1")[["ps"]])

# Match controls → find counterfactual from treated
nn.fit(data.query(f"{T}==1")[["ps"]], data.query(f"{T}==1")[Y])
matched_y1 = nn.predict(data.query(f"{T}==0")[["ps"]])

# ATE
ate = np.mean(
    np.concatenate([
        data.query(f"{T}==1")[Y].values - matched_y0,
        matched_y1 - data.query(f"{T}==0")[Y].values
    ])
)
```

## Limitations

- With poor [[positivity]], matches are distant → bias
- 1-NN is noisy; caliper matching or k-NN can help
- Discards information from unmatched units

## Related notes

- [[propensity score]]
- [[Inverse Probability Weighting (IPW)]]
- [[positivity]]
- [[Adjusting for Confounding (MOC)]]
