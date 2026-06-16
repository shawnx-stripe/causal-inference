---
title: Prediction vs causation
aliases:
  - prediction is not causation
  - predictive models for causal tasks
  - policy vs prediction
tags:
  - causal-inference
  - heterogeneity
  - evaluation
updated: 2026-06-17
---

# Prediction vs causation

> [!summary]
> A model that predicts $Y$ well does NOT estimate the causal effect of $T$ on $Y$. Predictive models identify units with high *levels* of outcome, not units *responsive* to treatment. Targeting decisions require CATE models ([[CATE]]), not outcome models. This distinction is the bridge from ML (Part I) to causal ML (Part II).

> [!tip] Sources
> - [Ch 17: Predictive Models 101](https://matheusfacure.github.io/python-causality-handbook/17-Predictive-Models-101.html)
> - [Appendix: When Prediction Fails](https://matheusfacure.github.io/python-causality-handbook/When-Prediction-Fails.html)
> - [Appendix: Why Prediction Metrics are Dangerous For Causal Models](https://matheusfacure.github.io/python-causality-handbook/Prediction-Metrics-For-Causal-Models.html)

## The distinction

| | Prediction | Causation |
|---|---|---|
| Target | $E[Y \mid X]$ | $E[Y(1) - Y(0) \mid X]$ |
| Question | What will happen? | What would change if we intervene? |
| Evaluation | MSE, AUC | [[evaluating CATE\|Cumulative gain]], BLP, GATES |
| Use case | Forecasting, scoring | Targeting, policy |

## Policy progression

1. **Treat everyone** (no model needed)
2. **Segment by one feature** (manual rule)
3. **Predictive model policy** — treat if $\hat{E}[Y \mid X] > \text{threshold}$ (WRONG for causation)
4. **CATE model policy** — treat if $\hat{\tau}(x) > \text{cost}$ (CORRECT)

## Why prediction fails for targeting

A high-$\hat{Y}$ unit may have:
- High baseline $Y(0)$ → would be fine without treatment
- Low $\tau(x)$ → treatment doesn't help them

The units who *benefit most* from treatment may have low predicted $Y$ but high $\tau(x)$.

## Code pattern: prediction vs. sensitivity

```python
from sklearn.ensemble import GradientBoostingRegressor

# Predictive model (predicts Y level — WRONG for targeting)
pred_model = GradientBoostingRegressor()
pred_model.fit(train[X + [T]], train[Y])
predictions = pred_model.predict(test[X + [T]])

# Sensitivity model (predicts treatment EFFECT — CORRECT)
def pred_sensitivity(model, df, t="price"):
    """Numerical derivative: how much does Y change per unit of T?"""
    return (model.predict(df.assign(**{t: df[t] + 1}))
          - model.predict(df))

sensitivity = pred_sensitivity(pred_model, test)
# These rank DIFFERENTLY from predictions!
```

## Related notes

- [[CATE]]
- [[evaluating CATE]]
- [[target transformation]]
- [[Effect Heterogeneity (MOC)]]
