---
title: Instrumental Variables (IV)
aliases:
  - IV
  - instrumental variable
  - 2SLS
  - two-stage least squares
tags:
  - causal-inference
  - iv
  - identification
updated: 2026-06-17
---

# Instrumental Variables (IV)

> [!summary]
> Instrumental variables identify causal effects when treatment is endogenous (confounded by unobservables). An instrument $Z$ affects the outcome $Y$ *only through* treatment $T$. The Wald estimator (reduced form / first stage) recovers the [[Local Average Treatment Effect (LATE)]] for compliers. 2SLS generalizes to multiple instruments and controls.

## When to use IV

When unobserved confounders $U$ affect both $T$ and $Y$:
- DAG: $U \to T$, $U \to Y$, $Z \to T$, $T \to Y$
- OLS on $T$ is biased; but variation in $T$ *induced by $Z$* is exogenous

## Assumptions

1. **[[relevance]]** (first stage): $Z$ predicts $T$ — $\text{Cov}(Z, T) \neq 0$
2. **[[exclusion restriction]]**: $Z$ affects $Y$ only through $T$
3. **Independence**: $Z$ is as-good-as-random (uncorrelated with $U$)
4. **[[monotonicity]]**: No defiers — $T(Z=1) \geq T(Z=0)$ for all units

## Compliance types

| Type | Behavior | Identified by IV? |
|------|----------|-------------------|
| [[compliers]] | Follow assignment: $T(1)=1, T(0)=0$ | Yes (LATE) |
| [[always-takers]] | Always treated: $T(1)=T(0)=1$ | No |
| [[never-takers]] | Never treated: $T(1)=T(0)=0$ | No |
| [[defiers]] | Do opposite: $T(1)=0, T(0)=1$ | Ruled out by monotonicity |

## Wald estimator (LATE)

$$
\text{LATE} = \frac{\text{Reduced Form}}{\text{First Stage}} = \frac{\mathbb{E}[Y \mid Z=1] - \mathbb{E}[Y \mid Z=0]}{\mathbb{E}[T \mid Z=1] - \mathbb{E}[T \mid Z=0]}
$$

## Two-Stage Least Squares (2SLS)

**Stage 1**: $\hat{T} = \hat{\pi}_0 + \hat{\pi}_1 Z + X'\hat{\gamma}$

**Stage 2**: $Y = \alpha + \tau \hat{T} + X'\beta + \varepsilon$

> [!warning]
> Manual 2SLS (fitting stage 1 then plugging $\hat{T}$ into stage 2) produces **incorrect standard errors**. Use a proper IV estimator.

## SE inflation from non-compliance

$$
\text{SE}_{\text{IV}} \approx \frac{\text{SE}_{\text{OLS}}}{\text{compliance rate}}
$$

Sample size requirement: $N_{\text{IV}} / N_{\text{OLS}} \approx 1 / \text{compliance}^2$

## Code

```python
from linearmodels import IV2SLS
import statsmodels.formula.api as smf

# Reduced form (Z → Y)
rf = smf.ols("pv ~ prime_eligible", data=df).fit()

# First stage (Z → T)
fs = smf.ols("prime_card ~ prime_eligible", data=df).fit()

# Wald estimate
late = rf.params["prime_eligible"] / fs.params["prime_eligible"]

# Proper 2SLS with correct SEs
iv_model = IV2SLS.from_formula(
    "pv ~ 1 + [prime_card ~ prime_eligible]", data=df
).fit(cov_type="unadjusted")
```

## Related notes

- [[Regression Discontinuity Design (RDD)]]
- [[Local Average Treatment Effect (LATE)]]
- [[noncompliance]]
- [[weak instruments]]
- [[Design-Based Identification (MOC)]]
