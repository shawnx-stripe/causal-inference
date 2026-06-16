---
title: Binary outcome nonlinearity
aliases:
  - curvature dominance
  - logistic CATE challenge
  - 50% rule
tags:
  - causal-inference
  - heterogeneity
  - evaluation
updated: 2026-06-17
---

# Binary outcome nonlinearity

> [!summary]
> With binary outcomes (conversion, churn), the S-shaped logistic curve means treatment effects are mechanically largest near 50% baseline probability and smallest near 0% or 100%. This creates spurious "heterogeneity" — features that predict baseline conversion *appear* to moderate treatment effects even when they don't on the latent scale.

> [!tip] Sources
> - [Ch 23: Challenges with Effect Heterogeneity and Nonlinearity](https://matheusfacure.github.io/python-causality-handbook/23-Challenges-with-Effect-Heterogeneity-and-Nonlinearity.html)

## The mechanism

Latent outcome model:
$$
Y^*_i = \alpha + \beta X_i + \tau T_i + \varepsilon_i
$$
$$
Y_i = \mathbf{1}\{Y^*_i > 0\}
$$

The observable treatment effect $P(Y=1|T=1,X) - P(Y=1|T=0,X)$ depends on *where on the logistic curve* the unit sits — even if $\tau$ is constant for everyone on the latent scale.

## Curvature dominance

Features that predict baseline $P(Y=1|T=0)$ will appear to drive CATE heterogeneity on the observed probability scale, because:
- Units at 50% baseline → large marginal effect from $\tau$
- Units at 5% or 95% baseline → small marginal effect from same $\tau$

> [!warning]
> A CATE model may correctly identify that low-baseline units have smaller *observable* effects, but this is a mathematical artifact of the logistic link function, not genuine heterogeneity in the causal mechanism.

## The 50% rule

For targeting: focus on units whose baseline conversion is closest to 50% — they sit at the inflection point where marginal effects are largest.

## Reversed heterogeneity

Direction depends on the regime:
- **Low average conversion** (left side of curve): high-baseline units have *larger* effects
- **High average conversion** (right side of curve): high-baseline units have *smaller* effects

The sign of apparent heterogeneity can flip depending on where the population sits on the curve.

## Code

```python
import numpy as np

np.random.seed(42)
n = 10000

income = np.random.gamma(20, 2, n) * 100
nudge = np.random.binomial(1, 0.5, n)
age = np.random.gamma(10, 4, n)

# Latent model (constant treatment effect on latent scale)
latent = np.random.normal(
    -4.5 + income * 0.001 + nudge + nudge * age * 0.01
)

# Binary outcome at different thresholds (regimes)
conversion_medium = (latent > 0).astype(int)    # ~50% conversion
conversion_low = (latent > 2).astype(int)       # ~12% conversion
conversion_high = (latent > -2).astype(int)     # ~93% conversion

# The SAME latent effect produces different observable heterogeneity patterns
# depending on the conversion threshold!
```

## Implications for CATE modeling

1. Always check baseline conversion rate before interpreting CATE heterogeneity
2. Consider modeling on the latent/log-odds scale rather than probability scale
3. The "best targeting" unit may shift depending on the population's baseline rate
4. Report whether heterogeneity is "mechanical" (curvature) or "structural" (true interaction)

## Related notes

- [[CATE]]
- [[evaluating CATE]]
- [[prediction vs causation]]
- [[Effect Heterogeneity (MOC)]]
