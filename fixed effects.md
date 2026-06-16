---
title: Fixed effects
aliases:
  - panel fixed effects
  - entity fixed effects
  - within estimator
  - FE
  - LSDV
tags:
  - causal-inference
  - panel-data
  - did
updated: 2026-06-17
---

# Fixed effects

> [!summary]
> Fixed effects (FE) remove time-invariant unobserved confounders by exploiting within-unit variation over time. Equivalent to demeaning: subtracting each unit's time-average from all variables, then running OLS on the demeaned data. Cannot estimate effects of time-constant variables (race, education level, etc.).

> [!tip] Sources
> - [Ch 14: Panel Data and Fixed Effects](https://matheusfacure.github.io/python-causality-handbook/14-Panel-Data-and-Fixed-Effects.html)

## The within estimator

Model with unobserved unit heterogeneity $U_i$:
$$
Y_{it} = \beta X_{it} + \gamma U_i + \varepsilon_{it}
$$

After demeaning (subtracting unit means $\bar{Y}_i$, $\bar{X}_i$):
$$
(Y_{it} - \bar{Y}_i) = \beta(X_{it} - \bar{X}_i) + (\varepsilon_{it} - \bar{\varepsilon}_i)
$$

The $\gamma U_i$ term cancels because it's constant within units. This is the **within estimator**.

## What FE can and cannot do

- **Removes**: all time-invariant confounders (observed or not)
- **Cannot remove**: time-varying confounders, reverse causality
- **Cannot estimate**: effects of time-invariant variables (absorbed by unit dummies)

## Connection to DiD

FE is weaker than full [[Difference-in-Differences (DiD)|DiD]]:
- FE assumes no *time-varying* confounders (strict exogeneity)
- DiD (with time FE) only assumes *parallel trends* — a weaker condition
- Two-way FE (entity + time) is equivalent to DiD in balanced panels

## Code

```python
from linearmodels.panel import PanelOLS
import pandas as pd

# MUST set MultiIndex [entity, time]
data = data.set_index(["person_id", "year"])

# Entity fixed effects only
mod = PanelOLS.from_formula(
    "lwage ~ expersq + union + married + hours + EntityEffects",
    data=data
)
result = mod.fit(cov_type="clustered", cluster_entity=True)

# Two-way fixed effects (entity + time)
mod_twfe = PanelOLS.from_formula(
    "lwage ~ expersq + union + married + hours + EntityEffects + TimeEffects",
    data=data
)
result_twfe = mod_twfe.fit(cov_type="clustered", cluster_entity=True)

# Check which variables are time-invariant (zero within-unit variance)
data.groupby("person_id").std().sum()  # columns with 0 → absorbed by FE
```

## Empirical example

Marriage → wages (handbook Ch 14):
- Naive OLS: +0.141 (biased by unobserved ability/ambition)
- Entity FE: +0.115 (removes time-invariant confounders)
- Two-way FE: +0.048 (also removes time trends)

## Related notes

- [[Difference-in-Differences (DiD)]]
- [[Frisch-Waugh-Lovell theorem]]
- [[Panel Data and DiD (MOC)]]
