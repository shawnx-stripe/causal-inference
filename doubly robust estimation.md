---
title: Doubly robust estimation
aliases:
  - DR estimation
  - AIPW
  - augmented IPW
  - doubly robust
tags:
  - causal-inference
  - propensity-score
  - doubly-robust
updated: 2026-06-17
---

# Doubly robust estimation

> [!summary]
> Doubly robust (DR) estimators combine an outcome model and a [[propensity score]] model. The estimate is consistent if *either* model is correctly specified — hence "doubly" robust. The canonical form is Augmented IPW (AIPW), which augments [[Inverse Probability Weighting (IPW)|IPW]] with an outcome regression correction.

> [!tip] Sources
> - [Ch 12: Doubly Robust Estimation](https://matheusfacure.github.io/python-causality-handbook/12-Doubly-Robust-Estimation.html)
> - Related to the DR-DiD in [08-Difference-in-Differences.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/08-Difference-in-Differences.ipynb) — "Doubly Robust Diff-in-Diff" section

## Core idea

The AIPW estimator for ATE:
$$
\hat{\tau}_{\text{DR}} = \frac{1}{n}\sum_i \left[\hat{\mu}_1(X_i) - \hat{\mu}_0(X_i) + \frac{T_i(Y_i - \hat{\mu}_1(X_i))}{\hat{e}(X_i)} - \frac{(1-T_i)(Y_i - \hat{\mu}_0(X_i))}{1-\hat{e}(X_i)}\right]
$$

- If $\hat{\mu}$ is correct: the IPW corrections are mean-zero → consistent
- If $\hat{e}$ is correct: the outcome model errors are properly reweighted → consistent
- Both correct: semiparametrically efficient

## When the outcome model is easy

If $\hat{\mu}_1(x)$ and $\hat{\mu}_0(x)$ are well-specified, the DR estimator reduces to:
$$
\hat{\tau} \approx \frac{1}{n}\sum_i [\hat{\mu}_1(X_i) - \hat{\mu}_0(X_i)]
$$

The IPW correction terms average to zero.

## When the PS model is easy

If $\hat{e}(x)$ is correct but $\hat{\mu}$ is misspecified, the IPW corrections dominate and rescue the estimate.

## Code

```python
from sklearn.linear_model import LogisticRegression, LinearRegression
import numpy as np

# Fit nuisance models
ps_model = LogisticRegression(max_iter=1000).fit(data[X], data[T])
ps = ps_model.predict_proba(data[X])[:, 1]

mu1_model = LinearRegression().fit(data.query(f"{T}==1")[X], data.query(f"{T}==1")[Y])
mu0_model = LinearRegression().fit(data.query(f"{T}==0")[X], data.query(f"{T}==0")[Y])

mu1 = mu1_model.predict(data[X])
mu0 = mu0_model.predict(data[X])

# AIPW / Doubly Robust ATE
t = data[T].values
y = data[Y].values
dr_ate = np.mean(
    (mu1 - mu0)
    + t * (y - mu1) / ps
    - (1 - t) * (y - mu0) / (1 - ps)
)
```

## Related notes

- [[Inverse Probability Weighting (IPW)]]
- [[propensity score]]
- [[double machine learning]]
- [[Difference-in-Differences (DiD)]]
- [[Adjusting for Confounding (MOC)]]
