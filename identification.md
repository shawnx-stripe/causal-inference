---
title: Identification
aliases:
  - causal identification
  - adjustment formula
  - conditional independence assumption
  - CIA
tags:
  - causal-inference
  - identification
  - dags
updated: 2026-06-17
---

# Identification

> [!summary]
> Identification asks: can the causal effect be expressed as a function of observable data? Under the conditional independence assumption (CIA) and positivity, the adjustment formula recovers causal effects by stratifying on confounders and averaging.

## Conditional Independence Assumption (CIA)

$$
(Y(1), Y(0)) \perp T \mid X
$$

Treatment is as-good-as-random *within strata of $X$*. Also called [[Unconfoundedness]] or selection on observables.

## Positivity (overlap)

$$
0 < P(T = 1 \mid X = x) < 1 \quad \text{for all } x
$$

Every unit must have a non-zero chance of receiving either treatment level. See [[positivity]].

## Adjustment formula

Under CIA + positivity:
$$
\mathbb{E}[Y \mid do(T=t)] = \sum_x P(X=x) \cdot \mathbb{E}[Y \mid T=t, X=x]
$$

This is the **stratified estimator**: compute the treatment effect within each stratum of $X$, then average over the population distribution of $X$.

## From DAGs to identification

1. Draw the [[causal DAGs|DAG]] encoding assumed causal structure
2. Find all backdoor paths from $T$ to $Y$
3. Choose a conditioning set $Z$ satisfying the [[back-door criterion]]
4. Apply the adjustment formula conditioning on $Z$

## Code

```python
import pandas as pd

df = pd.read_csv("./data/consultancy.csv")

# Naive (biased) estimator
naive_ate = (df.query("consultancy==1")["profits_next_6m"].mean()
           - df.query("consultancy==0")["profits_next_6m"].mean())

# Stratified (adjusted) estimator via groupby
avg = df.groupby(["consultancy", "profits_prev_6m"])["profits_next_6m"].mean()
# Effect within each stratum
effects = avg.loc[1] - avg.loc[0]
# Weighted average over strata (adjustment formula)
weights = df["profits_prev_6m"].value_counts(normalize=True)
adjusted_ate = (effects * weights).sum()
```

## Related notes

- [[causal DAGs]]
- [[potential outcomes]]
- [[positivity]]
- [[Unconfoundedness]]
- [[Foundations and Frameworks (MOC)]]
