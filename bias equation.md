---
title: Bias equation
aliases:
  - selection bias decomposition
  - bias decomposition
tags:
  - causal-inference
  - identification
  - potential-outcomes
updated: 2026-06-17
---

# Bias equation

> [!summary]
> The bias equation decomposes the naive associational estimate (difference in group means) into the true causal effect plus systematic bias from non-random treatment assignment. Understanding this decomposition motivates every causal identification strategy.

> [!tip] Sources
> - [01-Introduction-To-Causal-Inference.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/01-Introduction-To-Causal-Inference.ipynb)
> - [Ch 1: Introduction To Causality](https://matheusfacure.github.io/python-causality-handbook/01-Introduction-To-Causality.html)
> - Data: `xmas_sales.csv` — naive comparison is biased because `unit_fe` and `avg_week_sales` confound treatment

## Core idea

The naive estimator compares treated vs. control means:
$$
\mathbb{E}[Y \mid T=1] - \mathbb{E}[Y \mid T=0]
$$

This equals:
$$
\underbrace{\mathbb{E}[Y(1) - Y(0) \mid T=1]}_{\text{ATT}} + \underbrace{\mathbb{E}[Y(0) \mid T=1] - \mathbb{E}[Y(0) \mid T=0]}_{\text{Selection Bias}}
$$

**Selection bias** = baseline difference between groups *absent treatment*. If treated units would have had higher outcomes anyway, the naive estimate is biased upward.

## Visual intuition

- If $\text{Bias} > 0$: treatment group had higher baseline → overestimates effect
- If $\text{Bias} < 0$: treatment group had lower baseline → underestimates effect
- If $\text{Bias} = 0$: groups are comparable → naive estimate is causal (as in [[randomized experiments]])

## Eliminating bias

| Strategy | How it removes bias |
|----------|-------------------|
| [[randomized experiments]] | Makes $T \perp Y(0)$ by design |
| [[identification]] (conditioning) | Blocks confounding paths via adjustment |
| [[Instrumental Variables (IV)]] | Uses exogenous variation in $T$ |
| [[Difference-in-Differences (DiD)]] | Differences out time-invariant bias |

## Code

```python
import pandas as pd
import numpy as np

df = pd.read_csv("./data/xmas_sales.csv")

# Naive comparison (ATE + bias)
naive = (df.loc[df["is_on_sale"]==1, "weekly_amount_sold"].mean()
       - df.loc[df["is_on_sale"]==0, "weekly_amount_sold"].mean())

# If we had potential outcomes (simulation):
# true_att = df.loc[df["t"]==1, "te"].mean()
# selection_bias = naive - true_att
```

## Related notes

- [[potential outcomes]]
- [[selection bias]]
- [[identification]]
- [[Foundations and Frameworks (MOC)]]
