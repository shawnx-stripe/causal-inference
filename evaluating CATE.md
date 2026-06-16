---
title: Evaluating CATE
aliases:
  - CATE evaluation
  - cumulative gain curve
  - effect by quantile
  - uplift evaluation
tags:
  - causal-inference
  - heterogeneity
  - evaluation
updated: 2026-06-17
---

# Evaluating CATE

> [!summary]
> Since individual treatment effects are never observed, CATE models cannot be evaluated by standard prediction metrics. Instead, we use indirect methods: effect-by-quantile plots, cumulative effect curves, and cumulative gain curves that test whether the model correctly *ranks* units by treatment responsiveness.

## Challenge

We never observe $\tau_i = Y_i(1) - Y_i(0)$ — so we can't compute MSE against ground truth. Evaluation asks: does ranking by $\hat{\tau}(x)$ actually separate high-responders from low-responders?

## Effect by model quantile

Bin units into deciles by predicted $\hat{\tau}(x)$, then estimate the actual treatment effect within each bin:

```python
from toolz import curry

@curry
def effect(data, y, t):
    """Bivariate OLS slope: cov(y,t)/var(t)"""
    return (
        ((data[t] - data[t].mean()) * data[y]).sum()
        / ((data[t] - data[t].mean()) ** 2).sum()
    )

def effect_by_quantile(df, pred, y, t, q=10):
    df["quantile"] = pd.qcut(df[pred], q=q, duplicates="drop")
    return df.groupby("quantile").apply(effect(y=y, t=t))
```

If the model is informative, top quantiles should have higher effects than bottom quantiles.

## Cumulative effect curve

Sort units by $\hat{\tau}(x)$ descending. At each step $k$, compute the ATE among the top-$k$ units:

```python
def cumulative_effect_curve(df, prediction, y, t, steps=100):
    df_sorted = df.sort_values(prediction, ascending=False)
    effects = []
    for frac in np.linspace(0.01, 1, steps):
        subset = df_sorted.head(int(len(df_sorted) * frac))
        effects.append(effect(subset, y=y, t=t))
    return effects
```

A good model: effect starts high (best responders) and decreases toward ATE.

## Cumulative gain curve

Weights the cumulative effect by population fraction — shows total "gain" from treating top-$k\%$:
$$
\text{Gain}(k) = (\text{effect on top-}k - \text{ATE}) \times \frac{k}{N}
$$

The area under the normalized gain curve (analogous to AUC) provides a scalar metric for comparing CATE models.

## Related notes

- [[CATE]]
- [[T-learner]]
- [[X-learner]]
- [[double machine learning]]
- [[Effect Heterogeneity (MOC)]]
