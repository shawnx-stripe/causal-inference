---
title: CATE
aliases:
  - conditional average treatment effect
  - heterogeneous treatment effects
  - individualized treatment effect
tags:
  - causal-inference
  - heterogeneity
  - meta-learners
updated: 2026-06-17
---

# CATE

> [!summary]
> The Conditional Average Treatment Effect $\tau(x) = \mathbb{E}[Y(1) - Y(0) \mid X = x]$ captures how treatment effects vary with observable characteristics. Moving from ATE to CATE enables personalized targeting — treating only those who benefit.

> [!tip] Sources
> - [06-Effect-Heterogeneity.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/06-Effect-Heterogeneity.ipynb)
> - [Ch 18: Heterogeneous Treatment Effects and Personalization](https://matheusfacure.github.io/python-causality-handbook/18-Heterogeneous-Treatment-Effects-and-Personalization.html)
> - [Appendix: When Prediction Fails](https://matheusfacure.github.io/python-causality-handbook/When-Prediction-Fails.html)
> - Data: `daily_restaurant_sales.csv` (randomized discounts with heterogeneous effects by context)

## Core idea

The ATE can hide important heterogeneity. CATE reveals *who* benefits and by how much:
$$
\tau(x) = \mathbb{E}[Y(1) - Y(0) \mid X = x]
$$

## Why prediction ≠ causation

> [!warning]
> An ML model that predicts $Y$ well does NOT estimate treatment effects. A model predicting high $Y$ may identify units with high baseline outcomes, not units responsive to treatment. Targeting decisions require CATE models, not outcome models.

## CATE via interacted regression

The simplest approach: OLS with full treatment-by-covariate interactions.

$$
Y_i = \alpha + \tau_0 T_i + X_i'\beta + T_i \cdot X_i'\gamma + \varepsilon_i
$$

CATE = predicted outcome under $T+1$ minus predicted outcome under $T$:

```python
import statsmodels.formula.api as smf

X = ["month", "weekday", "is_holiday", "competitors_price"]
formula = f"sales ~ discounts*({'+'.join(X)})"
model = smf.ols(formula, data=train).fit()

# CATE = counterfactual prediction difference
cate = (model.predict(test.assign(discounts=test["discounts"] + 1))
      - model.predict(test))
```

## From CATE to decisions

- **Treat if** $\hat{\tau}(x) > \text{cost}$
- **Rank units** by $\hat{\tau}(x)$ descending → treat top-k for budget-constrained targeting
- See [[evaluating CATE]] for validation methods, [[policy learning]] for optimal rules

## Related notes

- [[evaluating CATE]]
- [[T-learner]]
- [[X-learner]]
- [[S-learner]]
- [[double machine learning]]
- [[Effect Heterogeneity (MOC)]]
