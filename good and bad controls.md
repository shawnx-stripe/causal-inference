---
title: Good and bad controls
aliases:
  - bad controls
  - good controls
  - control variable selection
  - beyond confounders
tags:
  - causal-inference
  - identification
  - dags
  - regression
updated: 2026-06-17
---

# Good and bad controls

> [!summary]
> Not all covariates should be included in a causal regression. The effect on bias AND variance depends on the variable's causal role. Confounders reduce bias; good controls reduce SE; harmful controls inflate SE; bad controls (mediators/colliders) introduce bias.

> [!tip] Sources
> - [Ch 7: Beyond Confounders](https://matheusfacure.github.io/python-causality-handbook/07-Beyond-Confounders.html)

## The four-category framework

| Category | Causal role | Effect of including | Recommendation |
|----------|------------|--------------------|----|
| **Confounder** | Causes both T and Y | Removes bias | Always include |
| **Good control** | Causes Y only (not T) | Reduces residual variance → smaller SE | Include |
| **Harmful control** | Causes T only (not Y) | Reduces treatment variance → larger SE | Avoid |
| **Bad control (mediator)** | T → X → Y | Blocks causal path, induces bias | Never include |
| **Bad control (collider)** | T → X ← Y | Opens non-causal path, induces bias | Never include |

## Why good controls help (even in RCTs)

Adding covariates that predict $Y$ (but not $T$) reduces $\text{Var}(\varepsilon)$ in:
$$
\text{Var}(\hat{\tau}) = \frac{\sigma^2_\varepsilon}{\sum(T_i - \bar{T})^2}
$$

The numerator shrinks while the denominator stays the same → tighter CIs.

## Why harmful controls hurt

Adding predictors of $T$ (not $Y$) reduces $\text{Var}(\tilde{T})$ after partialling out:
$$
\text{Var}(\hat{\tau}) = \frac{\sigma^2_\varepsilon}{\sum(\tilde{T}_i)^2}
$$

Smaller denominator → larger SE → less power.

## Bad COP (Conditional on Positives)

Decomposing a zero-inflated outcome into $P(Y>0|T) \times E[Y|Y>0,T]$ and estimating COP separately is biased *even under randomization* because $Y>0$ is a collider between $T$ and unobserved determinants of $Y$.

$$
E[Y|Y>0,T=1] - E[Y|Y>0,T=0] = \underbrace{E[Y_1-Y_0|Y_1>0]}_{\text{causal}} + \underbrace{E[Y_0|Y_1>0] - E[Y_0|Y_0>0]}_{\text{selection bias}}
$$

## Code

```python
import statsmodels.formula.api as smf

# Confounder (reduces bias) — always include
smf.ols("payments ~ email + credit_limit + risk_score", data=data).fit()

# Good control (reduces SE) — include
smf.ols("payments ~ email + credit_limit + risk_score + age", data=data).fit()

# Harmful control (inflates SE) — avoid
# adding a strong predictor of email that doesn't affect payments
smf.ols("payments ~ email + credit_limit + risk_score + marketing_segment", data=data).fit()

# Bad control (mediator) — NEVER include
# app_opens is caused by email and causes payments
smf.ols("payments ~ email + app_opens", data=data).fit()  # BIASED
```

## Related notes

- [[causal DAGs]]
- [[identification]]
- [[linear regression for causal inference]]
- [[Frisch-Waugh-Lovell theorem]]
- [[Adjusting for Confounding (MOC)]]
