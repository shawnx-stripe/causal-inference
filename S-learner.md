---
title: S-learner
aliases:
  - single-model learner
tags:
  - causal-inference
  - heterogeneity
  - meta-learners
updated: 2026-06-17
---

# S-learner

> [!summary]
> The S-learner fits a single model with treatment as a feature: $\hat{\mu}(X, T)$. CATE is extracted by counterfactual prediction: $\hat{\tau}(x) = \hat{\mu}(x, t+1) - \hat{\mu}(x, t)$. Natural for continuous treatments but can underestimate effects due to regularization shrinking the treatment coefficient.

> [!tip] Sources
> - [07-Meta-Learners.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/07-Meta-Learners.ipynb) — "S-Learner" section
> - [Ch 20: Plug-and-Play Estimators](https://matheusfacure.github.io/python-causality-handbook/20-Plug-and-Play-Estimators.html)
> - Data: `discount_data.csv` (continuous treatment: discounts → sales). Simulation in notebook shows ATE underestimation across 500 runs due to LightGBM regularization.

## Estimation

1. Fit a single model: $\hat{\mu}(X, T)$ on all data
2. Predict under counterfactual treatment levels:
$$
\hat{\tau}(x) = \hat{\mu}(x, t+1) - \hat{\mu}(x, t)
$$

For binary treatment: $\hat{\tau}(x) = \hat{\mu}(x, 1) - \hat{\mu}(x, 0)$

## Code

```python
from lightgbm import LGBMRegressor
import numpy as np

# Fit single model with treatment as feature
features = X + [T]  # treatment included as a column
s_learner = LGBMRegressor(n_estimators=100)
s_learner.fit(train[features], train[Y])

# CATE via counterfactual grid (continuous treatment)
cate = (s_learner.predict(test.assign(**{T: test[T] + 1})[features])
      - s_learner.predict(test[features]))
```

## Regularization bias

> [!warning]
> Regularized learners (L1/L2, tree depth limits) tend to shrink the treatment coefficient toward zero — especially when treatment has small marginal predictive value relative to other features. This causes systematic **underestimation** of the ATE/CATE. The [[double machine learning|R-learner/DML]] approach avoids this by residualizing treatment.

## When to use

- Continuous treatments (natural parameterization)
- Large effects relative to noise (less regularization pressure)
- When combined with methods that penalize treatment less (e.g., forced first split on $T$)

## Related notes

- [[T-learner]]
- [[X-learner]]
- [[double machine learning]]
- [[CATE]]
- [[Effect Heterogeneity (MOC)]]
