---
title: Frisch-Waugh-Lovell theorem
aliases:
  - FWL theorem
  - FWL
  - orthogonalization
  - partialling out
  - debiasing step
tags:
  - causal-inference
  - regression
  - identification
updated: 2026-06-17
---

# Frisch-Waugh-Lovell theorem

> [!summary]
> The FWL theorem states that the coefficient on a variable of interest in a multivariate regression equals the coefficient from a bivariate regression of residualized $Y$ on residualized $T$ — both residualized against the other covariates. This "debiasing" intuition is the foundation of [[double machine learning]].

> [!tip] Sources
> - [04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb) — "Frisch-Waugh-Lovell Theorem and Orthogonalization" section
> - [Appendix: Debiasing with Orthogonalization](https://matheusfacure.github.io/python-causality-handbook/Debiasing-with-Orthogonalization.html)
> - Data: `risk_data.csv` — debiasing credit_limit against wage + credit_scores recovers the same coefficient as multivariate OLS

## Core idea

In the regression $Y = \tau T + X\beta + \varepsilon$:

1. **Debias treatment**: Regress $T$ on $X$, get residuals $\tilde{T} = T - \hat{E}[T \mid X]$
2. **Denoise outcome**: Regress $Y$ on $X$, get residuals $\tilde{Y} = Y - \hat{E}[Y \mid X]$
3. **Final estimate**: Regress $\tilde{Y}$ on $\tilde{T}$ → coefficient = $\hat{\tau}$

The debiasing step removes confounding; the denoising step reduces variance. Both together equal the multivariate OLS coefficient.

## Why it matters for causal inference

- Justifies residualization as a valid identification strategy
- The "debias" step is exactly what [[propensity score]] orthogonalization does
- Generalizes to ML: replace OLS residualization with flexible learners → [[double machine learning]]

## Code

```python
import statsmodels.formula.api as smf

# Full multivariate regression (baseline)
full_model = smf.ols(
    "default ~ credit_limit + wage + credit_score1 + credit_score2",
    data=risk_data
).fit()

# FWL: Step 1 — debias treatment
debiasing_model = smf.ols(
    "credit_limit ~ wage + credit_score1 + credit_score2",
    data=risk_data
).fit()

# FWL: Step 2 — regress Y on residualized T
risk_data["credit_limit_res"] = debiasing_model.resid
fwl_model = smf.ols("default ~ credit_limit_res", data=risk_data).fit()

# fwl_model coefficient on credit_limit_res == full_model coefficient on credit_limit
```

## Related notes

- [[linear regression for causal inference]]
- [[double machine learning]]
- [[propensity score]]
- [[Adjusting for Confounding (MOC)]]
