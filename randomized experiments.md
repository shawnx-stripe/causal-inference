---
title: Randomized experiments
aliases:
  - RCT
  - A/B test
  - randomized controlled trial
  - random assignment
tags:
  - causal-inference
  - identification
  - experimentation
updated: 2026-06-17
---

# Randomized experiments

> [!summary]
> Randomization makes treatment assignment independent of all confounders — observed and unobserved — by design. This eliminates [[bias equation|selection bias]], making the simple difference in means a valid causal estimator.

## Why randomization works

By random assignment: $T \perp (Y(1), Y(0))$

Therefore:
$$
\text{ATE} = \mathbb{E}[Y \mid T=1] - \mathbb{E}[Y \mid T=0]
$$

No adjustment needed. The naive estimator *is* the causal estimator.

## Balance checking

Randomization should produce comparable groups on pre-treatment covariates. **Normalized difference** as a balance diagnostic:
$$
\text{ND} = \frac{\bar{X}_T - \bar{X}_C}{\sqrt{(\sigma_T^2 + \sigma_C^2) / 2}}
$$

Rule of thumb: $|\text{ND}| < 0.1$ indicates good balance.

## The most dangerous equation

de Moivre's equation: $\text{SE} = \sigma / \sqrt{n}$

> [!warning]
> Small samples produce high-variance estimates that look extreme but are unreliable. Top-performing units in small groups are often sampling artifacts, not genuinely exceptional.

## Code

```python
import pandas as pd
import numpy as np

df = pd.read_csv("./data/cross_sell_email.csv")

# ATE by treatment arm
ate_by_group = df.groupby("cross_sell_email")["conversion"].mean()

# Normalized difference for balance check
def normalized_diff(data, covariate, treatment):
    treated = data[data[treatment] == 1][covariate]
    control = data[data[treatment] == 0][covariate]
    return (treated.mean() - control.mean()) / np.sqrt(
        (treated.var() + control.var()) / 2
    )

nd_age = normalized_diff(df, "age", "cross_sell_email")
```

## Related notes

- [[potential outcomes]]
- [[bias equation]]
- [[identification]]
- [[Foundations and Frameworks (MOC)]]
