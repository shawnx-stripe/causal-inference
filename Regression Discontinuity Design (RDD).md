---
title: Regression Discontinuity Design (RDD)
aliases:
  - RDD
  - regression discontinuity
  - discontinuity design
tags:
  - causal-inference
  - rdd
  - identification
updated: 2026-06-17
---

# Regression Discontinuity Design (RDD)

> [!summary]
> RDD exploits a sharp cutoff in a continuous running variable that determines treatment assignment. Units just above and below the cutoff are nearly identical on all characteristics, creating a local quasi-experiment. RDD can be viewed as [[Instrumental Variables (IV)|IV]] with a threshold instrument.

> [!tip] Sources
> - [11-Non-Compliance-and-Instruments.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/11-Non-Compliance-and-Instruments.ipynb) — "Discontinuity Design" section
> - [Ch 16: Regression Discontinuity Design](https://matheusfacure.github.io/python-causality-handbook/16-Regression-Discontinuity-Design.html)
> - Data: `prime_card_discontinuity.csv` — cutoff at balance=5000, running variable centered at cutoff, estimates local ITT via interaction regression

## Core idea

Treatment assigned by a rule: $T_i = \mathbf{1}(R_i > c)$ where $R$ is the running variable and $c$ is the cutoff.

Local causal effect at the cutoff:
$$
\tau_{\text{RD}} = \lim_{r \downarrow c} \mathbb{E}[Y \mid R=r] - \lim_{r \uparrow c} \mathbb{E}[Y \mid R=r]
$$

## Sharp vs. fuzzy

- **Sharp RDD**: Treatment is deterministic at cutoff ($T = \mathbf{1}(R > c)$ exactly)
- **Fuzzy RDD**: Treatment probability jumps at cutoff but isn't 0→1. Estimated as local Wald ratio (IV at the cutoff):
$$
\tau_{\text{fuzzy}} = \frac{\Delta Y \text{ at } c}{\Delta T \text{ at } c}
$$

## Assumptions

1. **Continuity**: Potential outcomes are continuous at the cutoff (no other discontinuity)
2. **No manipulation**: Units cannot precisely sort around the cutoff
3. **Excludability**: The cutoff only affects $Y$ through treatment (for fuzzy)

## Kernel weighting

The triangular kernel gives higher weight to observations near the cutoff:
$$
K(R, c, h) = \mathbf{1}\{|R-c| \leq h\} \cdot \left(1 - \frac{|R-c|}{h}\right)
$$

Applied as WLS weights in the local linear regression.

## Estimation

Local linear regression with treatment indicator and running variable interaction:
$$
Y_i = \alpha + \tau \cdot T_i + \beta_1 (R_i - c) + \beta_2 \cdot T_i \cdot (R_i - c) + \varepsilon_i
$$

## Diagnostics

- **Density test** ([[McCrary test]]): Check for bunching at the cutoff
- **Covariate continuity**: Pre-treatment variables should be smooth at $c$
- **Bandwidth sensitivity**: Results should be robust across bandwidth choices
- **RD plot**: Visual check for the discontinuity

## Code

```python
import statsmodels.formula.api as smf
import numpy as np

# Triangular kernel for local weighting
def kernel(R, c, h):
    indicator = (np.abs(R - c) <= h).astype(float)
    return indicator * (1 - np.abs(R - c) / h)

# Center running variable at cutoff
df["balance_centered"] = df["balance"] - 5000
df["above_cutoff"] = (df["balance_centered"] > 0).astype(int)

# Sharp RDD with kernel weighting (WLS)
weights = kernel(df["balance_centered"], c=0, h=1000)
rdd_model = smf.wls(
    "pv ~ above_cutoff * balance_centered",
    data=df, weights=weights
).fit()
itt = rdd_model.params["above_cutoff"]

# Simple OLS version (no kernel)
rdd_simple = smf.ols("pv ~ above_cutoff * balance_centered", data=df).fit()

# Fuzzy RDD via IV
from linearmodels import IV2SLS
fuzzy = IV2SLS.from_formula(
    "pv ~ 1 + balance_centered + [prime_card ~ above_cutoff]",
    data=df
).fit()
```

## Connection to IV

RDD is a special case of [[Instrumental Variables (IV)|IV]] where:
- Instrument $Z = \mathbf{1}(R > c)$
- Local to the cutoff (effect for compliers at the margin)
- Validity comes from continuity rather than exclusion restriction arguments

## Related notes

- [[Instrumental Variables (IV)]]
- [[Local Average Treatment Effect (LATE)]]
- [[geo experiment]]
- [[Design-Based Identification (MOC)]]
