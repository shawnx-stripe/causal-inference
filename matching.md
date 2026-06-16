---
title: Matching
aliases:
  - covariate matching
  - nearest-neighbor matching
  - bias-corrected matching
tags:
  - causal-inference
  - matching
  - identification
updated: 2026-06-17
---

# Matching

> [!summary]
> Matching estimates causal effects by pairing treated units with similar control units on observed covariates. Unlike [[propensity score matching]], covariate matching uses the raw feature space directly. Bias-corrected matching (Abadie-Imbens) adjusts for imperfect matches using a regression correction.

> [!tip] Sources
> - [Ch 10: Matching](https://matheusfacure.github.io/python-causality-handbook/10-Matching.html)

## Subclassification estimator

Partition covariate space into cells, compute within-cell treatment effects, average:
$$
\widehat{\text{ATE}} = \sum_k \frac{N_k}{N} (\bar{Y}_{k,1} - \bar{Y}_{k,0})
$$

## Nearest-neighbor matching on covariates

For each treated unit $i$, find the nearest control on standardized $X$:
$$
\widehat{\text{ATE}} = \frac{1}{N} \sum_{i=1}^{N} (2T_i - 1)(Y_i - Y_{j(i)})
$$

> [!warning]
> Matching bias doesn't vanish asymptotically — $\sqrt{N}$ grows faster than match quality improves. This is why bias correction matters.

## Bias-corrected matching (Abadie-Imbens)

Subtract a regression-based correction for the match discrepancy:
$$
\widehat{\text{ATT}}_{\text{bc}} = \frac{1}{N_1}\sum_{i:T=1} \Big[(Y_i - Y_{j(i)}) - (\hat{\mu}_0(X_i) - \hat{\mu}_0(X_{j(i)}))\Big]
$$

## Curse of dimensionality

With $k$ covariates binned into $m$ categories: $m^k$ cells. Match quality degrades exponentially with dimension → motivates [[propensity score]] (scalar summary).

## Code

```python
from sklearn.neighbors import KNeighborsRegressor
import numpy as np

# Standardize features first
X_std = (data[X] - data[X].mean()) / data[X].std()

# K-NN matching on raw covariates
mt0 = KNeighborsRegressor(n_neighbors=1)
mt0.fit(X_std[data[T]==0], data.loc[data[T]==0, Y])
y0_hat = mt0.predict(X_std[data[T]==1])

mt1 = KNeighborsRegressor(n_neighbors=1)
mt1.fit(X_std[data[T]==1], data.loc[data[T]==1, Y])
y1_hat = mt1.predict(X_std[data[T]==0])

ate = np.mean(np.concatenate([
    data.loc[data[T]==1, Y].values - y0_hat,
    y1_hat - data.loc[data[T]==0, Y].values
]))

# Bias-corrected matching via causalinference package
from causalinference import CausalModel
cm = CausalModel(Y=data[Y].values, D=data[T].values, X=data[X].values)
cm.est_via_matching(matches=1, bias_adj=True)
print(cm.estimates)
```

## Matching vs. regression weighting

- Regression weights cells by $N_k \cdot p_k(1-p_k)$ (sample size × treatment variance)
- Matching/subclassification weights cells by $N_k$ only
- They target the same estimand but with different weighting → can give different answers

## Related notes

- [[propensity score matching]]
- [[propensity score]]
- [[positivity]]
- [[Adjusting for Confounding (MOC)]]
