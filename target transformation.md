---
title: Target transformation
aliases:
  - F-learner
  - plug-and-play estimator
  - modified outcome
tags:
  - causal-inference
  - heterogeneity
  - meta-learners
updated: 2026-06-17
---

# Target transformation

> [!summary]
> Target transformation (F-learner) constructs a modified outcome $Y^*$ such that $E[Y^*|X] = \tau(x)$, the CATE. Any regression model trained on $Y^*$ directly estimates heterogeneous treatment effects without special causal machinery. High variance but works at scale.

> [!tip] Sources
> - [Ch 20: Plug-and-Play Estimators](https://matheusfacure.github.io/python-causality-handbook/20-Plug-and-Play-Estimators.html)

## Core idea

Transform the outcome so its conditional expectation equals the CATE:
$$
E[Y^* \mid X] = \tau(X) = E[Y(1) - Y(0) \mid X]
$$

Then fit any ML model: `model.fit(X, y_star)` → predictions are CATE estimates.

## Formulas

**Binary treatment** (with propensity score):
$$
Y^*_i = Y_i \cdot \frac{T_i - e(X_i)}{e(X_i)(1 - e(X_i))}
$$

**Continuous treatment** (randomized):
$$
Y^*_i = (Y_i - \bar{Y}) \cdot \frac{T_i - \bar{T}}{\sigma^2_T}
$$

**Continuous treatment** (observational — requires debiasing):
$$
Y^*_i = (Y_i - \bar{Y}) \cdot \frac{T_i - \hat{M}(T_i)}{(T_i - \hat{M}(T_i))^2}
$$

where $\hat{M}(T_i) = \hat{E}[T \mid X_i]$ from a cross-fitted model.

## Variance is the price

> [!warning]
> $Y^*$ is a very noisy individual-level estimate of $\tau_i$. This method works best with large datasets (>1M rows) where the noise averages out. For smaller samples, [[double machine learning]] or [[X-learner]] may be more stable.

## For ranking only

If you only need to *rank* units by treatment sensitivity (not estimate magnitudes), you can drop normalizing denominators:
$$
Y^*_{\text{rank}} \propto (T_i - \bar{T})(Y_i - \bar{Y})
$$

## Code

```python
from lightgbm import LGBMRegressor
import numpy as np

# Binary treatment (randomized)
ps = train[T].mean()  # constant under randomization
y_star = train[Y] * (train[T] - ps) / (ps * (1 - ps))

cate_model = LGBMRegressor(max_depth=3, min_child_samples=300)
cate_model.fit(train[X], y_star)
cate_hat = cate_model.predict(test[X])

# Continuous treatment (randomized, ordering only)
y_star_cont = ((train[T] - train[T].mean())
             * (train[Y] - train[Y].mean()))

cate_model.fit(train[X_no_treatment], y_star_cont)
```

## Related notes

- [[CATE]]
- [[S-learner]]
- [[double machine learning]]
- [[evaluating CATE]]
- [[Effect Heterogeneity (MOC)]]
