---
title: T-learner
aliases:
  - two-model learner
tags:
  - causal-inference
  - heterogeneity
  - meta-learners
updated: 2026-06-17
---

# T-learner

> [!summary]
> The T-learner fits separate outcome models for treated ($\mu_1$) and control ($\mu_0$) units, then estimates CATE as the difference: $\hat{\tau}(x) = \hat{\mu}_1(x) - \hat{\mu}_0(x)$. Simple but can fail when one arm has limited data.

## Estimation

1. Fit $\hat{\mu}_0(x)$ on control data: $(X_i, Y_i)$ where $T_i = 0$
2. Fit $\hat{\mu}_1(x)$ on treated data: $(X_i, Y_i)$ where $T_i = 1$
3. CATE: $\hat{\tau}(x) = \hat{\mu}_1(x) - \hat{\mu}_0(x)$

## Code

```python
from lightgbm import LGBMRegressor

m0 = LGBMRegressor(n_estimators=100)
m1 = LGBMRegressor(n_estimators=100)

m0.fit(train.query(f"{T}==0")[X], train.query(f"{T}==0")[Y])
m1.fit(train.query(f"{T}==1")[X], train.query(f"{T}==1")[Y])

cate_t_learner = m1.predict(test[X]) - m0.predict(test[X])
```

## Failure mode

> [!warning]
> When treated sample is small, $\hat{\mu}_1$ is poorly estimated — high variance in sparse regions. The T-learner effectively halves the available data for each model. The [[X-learner]] addresses this by borrowing strength across arms.

## Related notes

- [[X-learner]]
- [[S-learner]]
- [[CATE]]
- [[evaluating CATE]]
- [[Effect Heterogeneity (MOC)]]
