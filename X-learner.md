---
title: X-learner
aliases:
  - cross-learner
tags:
  - causal-inference
  - heterogeneity
  - meta-learners
updated: 2026-06-17
---

# X-learner

> [!summary]
> The X-learner improves on the [[T-learner]] for imbalanced treatment groups by cross-imputing pseudo treatment effects, then combining estimates via propensity-score weighting. Particularly effective when one arm is much larger than the other.

> [!tip] Sources
> - [07-Meta-Learners.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/07-Meta-Learners.ipynb) — "X-Learner" section
> - [Ch 21: Meta Learners](https://matheusfacure.github.io/python-causality-handbook/21-Meta-Learners.html)
> - Data: `email_obs_data.csv` / `email_rnd_data.csv` — evaluated via `fklearn` relative cumulative gain AUC

## Two-stage procedure

**Stage 1** — Fit outcome models (same as T-learner):
- $\hat{\mu}_0(x)$ on controls, $\hat{\mu}_1(x)$ on treated

**Stage 2** — Impute pseudo individual treatment effects:
- For controls: $\hat{\tau}_0^{(i)} = \hat{\mu}_1(X_i) - Y_i$
- For treated: $\hat{\tau}_1^{(i)} = Y_i - \hat{\mu}_0(X_i)$

Fit models on the imputed effects:
- $\hat{\mu}_{\tau_0}(x)$ trained on control pseudo-effects
- $\hat{\mu}_{\tau_1}(x)$ trained on treated pseudo-effects

**Final CATE** — Propensity-weighted combination:
$$
\hat{\tau}(x) = \hat{e}(x) \cdot \hat{\mu}_{\tau_0}(x) + (1 - \hat{e}(x)) \cdot \hat{\mu}_{\tau_1}(x)
$$

The weighting down-weights the estimate from the smaller arm (where PS → 0 or 1).

## Code

```python
from lightgbm import LGBMRegressor
from sklearn.linear_model import LogisticRegression

# Stage 1: T-learner models
m0 = LGBMRegressor().fit(train.query(f"{T}==0")[X], train.query(f"{T}==0")[Y])
m1 = LGBMRegressor().fit(train.query(f"{T}==1")[X], train.query(f"{T}==1")[Y])

# Stage 2: Pseudo-effects
tau_0 = m1.predict(train.query(f"{T}==0")[X]) - train.query(f"{T}==0")[Y].values
tau_1 = train.query(f"{T}==1")[Y].values - m0.predict(train.query(f"{T}==1")[X])

# Fit CATE models on imputed effects
mu_tau0 = LGBMRegressor().fit(train.query(f"{T}==0")[X], tau_0)
mu_tau1 = LGBMRegressor().fit(train.query(f"{T}==1")[X], tau_1)

# Propensity weighting
ps_model = LogisticRegression(penalty="none", max_iter=1000)
ps_model.fit(train[X], train[T])
ps = ps_model.predict_proba(test[X])[:, 1]

# Final CATE
cate_x_learner = ps * mu_tau0.predict(test[X]) + (1 - ps) * mu_tau1.predict(test[X])
```

## Related notes

- [[T-learner]]
- [[S-learner]]
- [[propensity score]]
- [[CATE]]
- [[Effect Heterogeneity (MOC)]]
