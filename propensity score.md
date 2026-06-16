---
title: Propensity score
aliases:
  - PS
  - e(X)
  - treatment propensity
tags:
  - causal-inference
  - propensity-score
  - identification
updated: 2026-06-17
---

# Propensity score

> [!summary]
> The propensity score $e(X) = P(T=1 \mid X)$ is the probability of receiving treatment given covariates. Under [[Unconfoundedness]], adjusting for the scalar $e(X)$ is sufficient to remove confounding — it collapses high-dimensional confounders into a single number.

## Definition

$$
e(X) = P(T = 1 \mid X)
$$

**Balancing property**: Within strata of $e(X)$, the distribution of $X$ is independent of $T$. This means treated and control units with the same propensity score are comparable.

## Ways to use propensity scores

| Method | How it uses PS |
|--------|---------------|
| [[propensity score matching]] | Match treated/control units with similar $e(X)$ |
| [[Inverse Probability Weighting (IPW)]] | Weight by $1/e(X)$ to create pseudo-population |
| Stratification | Bin units by $e(X)$ quantiles, estimate within strata |
| Orthogonalization | Include $e(X)$ as a covariate in OLS |
| Doubly robust | Combine PS model with outcome model ([[AIPW]]) |

## Estimation

Typically via logistic regression, but any classifier works:

```python
from sklearn.linear_model import LogisticRegression

ps_model = LogisticRegression(penalty="none", max_iter=1000)
ps_model.fit(data[X], data[T])
ps = ps_model.predict_proba(data[X])[:, 1]
```

## PS orthogonalization (cheapest use)

Add PS as a control variable in OLS — approximates full covariate adjustment:

```python
import statsmodels.formula.api as smf

data["ps"] = ps
model = smf.ols(f"{Y} ~ {T} + ps", data=data).fit()
```

## Related notes

- [[Inverse Probability Weighting (IPW)]]
- [[propensity score matching]]
- [[positivity]]
- [[Frisch-Waugh-Lovell theorem]]
- [[Adjusting for Confounding (MOC)]]
