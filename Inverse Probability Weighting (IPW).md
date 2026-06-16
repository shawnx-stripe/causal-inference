---
title: Inverse Probability Weighting (IPW)
aliases:
  - IPW
  - inverse propensity weighting
  - Horvitz-Thompson estimator
tags:
  - causal-inference
  - propensity-score
  - weighting
updated: 2026-06-17
---

# Inverse Probability Weighting (IPW)

> [!summary]
> IPW reweights observed outcomes by the inverse of the [[propensity score]] to create a pseudo-population where treatment is independent of confounders. The ATE-IPW estimator weights treated units by $1/e(X)$ and controls by $1/(1-e(X))$.

> [!tip] Sources
> - [05-Propensity-Score.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/05-Propensity-Score.ipynb) — "Inverse Propensity Weighting" section
> - [Ch 11: Propensity Score](https://matheusfacure.github.io/python-causality-handbook/11-Propensity-Score.html)
> - Data: `management_training.csv` — IPW recovers ATE≈0.27 matching the adjusted OLS benchmark

## ATE estimator

$$
\widehat{\text{ATE}}_{\text{IPW}} = \frac{1}{n} \sum_i \frac{T_i \cdot Y_i}{\hat{e}(X_i)} - \frac{(1-T_i) \cdot Y_i}{1 - \hat{e}(X_i)}
$$

Equivalently (compact form):
$$
\widehat{\text{ATE}} = \frac{1}{n} \sum_i Y_i \cdot \frac{T_i - \hat{e}(X_i)}{\hat{e}(X_i)(1 - \hat{e}(X_i))}
$$

## Stabilized weights

Unstabilized weights inflate the pseudo-population size. **Stabilized weights** preserve original group proportions:
- Treated: $w_i = P(T=1) / \hat{e}(X_i)$
- Control: $w_i = P(T=0) / (1 - \hat{e}(X_i))$

Same ATE point estimate, lower variance.

## Pseudo-populations

IPW creates a "pseudo-population" where the PS distributions of treated and control overlap fully — treatment assignment is effectively randomized in this weighted population.

## Weight diagnostics

> [!warning] Weight clipping rule of thumb
> Weights exceeding ~20 (corresponding to $e(X) < 0.05$ or $> 0.95$) signal practical [[positivity]] violations. Options:
> - **Clip/trim**: cap weights at a maximum (e.g., 20) — trades small bias for large variance reduction
> - **Overlap weights**: $w(x) \propto e(x)(1-e(x))$ — focuses on well-supported region
> - **Report effective sample size**: $\text{ESS} = (\sum w_i)^2 / \sum w_i^2$ — low ESS means a few units dominate

## Bootstrap inference

```python
from joblib import Parallel, delayed
import numpy as np

def ipw_ate(data, X, T, Y):
    from sklearn.linear_model import LogisticRegression
    ps_model = LogisticRegression(penalty="none", max_iter=1000)
    ps_model.fit(data[X], data[T])
    ps = ps_model.predict_proba(data[X])[:, 1]
    return (data[Y] * (data[T] - ps) / (ps * (1 - ps))).mean()

# Bootstrap confidence interval
boots = Parallel(n_jobs=4)(
    delayed(ipw_ate)(data.sample(frac=1, replace=True), X, T, Y)
    for _ in range(500)
)
ci = np.percentile(boots, [2.5, 97.5])
```

## Related notes

- [[propensity score]]
- [[propensity score matching]]
- [[positivity]]
- [[stabilized weights]]
- [[Adjusting for Confounding (MOC)]]
