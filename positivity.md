---
title: Positivity
aliases:
  - overlap
  - common support
  - positivity assumption
tags:
  - causal-inference
  - identification
  - propensity-score
updated: 2026-06-17
---

# Positivity

> [!summary]
> Positivity requires that every unit has a non-zero probability of receiving each treatment level: $0 < P(T=1 \mid X) < 1$ for all $X$. When violated, causal effects are not identified for units with deterministic treatment — weighting blows up and matching has no valid counterfactual.

## Formal statement

$$
0 < e(X) < 1 \quad \text{for all } X \text{ in the support}
$$

Also called the **overlap** or **common support** condition.

## What breaks without positivity

- [[Inverse Probability Weighting (IPW)|IPW]] weights $\to \infty$ when $e(X) \to 0$ or $1$
- [[propensity score matching|Matching]] extrapolates to distant units
- Regression extrapolates beyond the data support
- The causal effect is **undefined** for units that would never/always be treated

## Diagnostics

- Plot PS distributions by treatment group — look for non-overlapping regions
- Check for extreme weights (max weight, effective sample size)
- Trim or winsorize units with $e(X) < \epsilon$ or $e(X) > 1-\epsilon$

## Code

```python
import numpy as np

# Simulated positivity violation
np.random.seed(42)
n = 1000
x = np.random.uniform(-3, 3, n)
# Treatment probability depends deterministically on x
e_x = np.clip(1 / (1 + np.exp(-3 * x)), 0.001, 0.999)
t = np.random.binomial(1, e_x)
# True effect = 1 everywhere
y0 = -x
y1 = y0 + 1
y = t * y1 + (1 - t) * y0

# IPW estimate (will be biased where e(x) ≈ 0 or 1)
ipw_ate = np.mean(y * (t - e_x) / (e_x * (1 - e_x)))
# True ATE = 1.0
```

## Related notes

- [[propensity score]]
- [[Inverse Probability Weighting (IPW)]]
- [[identification]]
- [[Adjusting for Confounding (MOC)]]
