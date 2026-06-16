---
title: Potential outcomes
aliases:
  - Neyman-Rubin causal model
  - Rubin causal model
  - counterfactual outcomes
  - PO framework
tags:
  - causal-inference
  - identification
  - potential-outcomes
updated: 2026-06-17
---

# Potential outcomes

> [!summary]
> The potential outcomes framework defines causal effects as contrasts between outcomes a unit *would* experience under different treatments. Only one potential outcome is ever observed; the other is the counterfactual. This is the fundamental problem of causal inference.

> [!tip] Sources
> - [01-Introduction-To-Causal-Inference.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/01-Introduction-To-Causal-Inference.ipynb)
> - [Ch 1: Introduction To Causality](https://matheusfacure.github.io/python-causality-handbook/01-Introduction-To-Causality.html)
> - Data: `xmas_sales.csv` (true ATE = 50) — see [[simulated data]]

## Core idea

For unit $i$ with binary treatment $T_i \in \{0, 1\}$:

- $Y_i(1)$ — outcome if treated
- $Y_i(0)$ — outcome if not treated
- Individual treatment effect: $\tau_i = Y_i(1) - Y_i(0)$

**Consistency** (observed outcome):
$$
Y_i = T_i \cdot Y_i(1) + (1 - T_i) \cdot Y_i(0)
$$

We never observe both $Y_i(1)$ and $Y_i(0)$ for the same unit — the counterfactual is always missing.

## Causal estimands

$$
\text{ATE} = \mathbb{E}[Y(1) - Y(0)]
$$
$$
\text{ATT} = \mathbb{E}[Y(1) - Y(0) \mid T = 1]
$$
$$
\text{CATE} = \mathbb{E}[Y(1) - Y(0) \mid X = x]
$$

## SUTVA

[[Stable Unit Treatment Value Assumption (SUTVA)]]:
1. **No interference** — one unit's treatment doesn't affect another's outcome
2. **No hidden versions** — treatment is well-defined (single version)

## Code

```python
import pandas as pd
import numpy as np

# God's-eye view: all potential outcomes known
df = pd.DataFrame({
    "unit": range(6),
    "y0": [3, 5, 4, 7, 2, 6],
    "y1": [5, 5, 8, 8, 4, 9],
    "t":  [1, 0, 1, 0, 0, 1]
})

# Individual treatment effect (only visible in simulation)
df["te"] = df["y1"] - df["y0"]

# Observed outcome (consistency assumption)
df["y"] = np.where(df["t"] == 1, df["y1"], df["y0"])

# ATE (true, from God's-eye view)
ate_true = df["te"].mean()

# Naive comparison (biased)
ate_naive = df.loc[df["t"]==1, "y"].mean() - df.loc[df["t"]==0, "y"].mean()
```

## Related notes

- [[bias equation]]
- [[identification]]
- [[randomized experiments]]
- [[Foundations and Frameworks (MOC)]]
