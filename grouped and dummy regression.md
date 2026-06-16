---
title: Grouped and dummy regression
aliases:
  - WLS
  - weighted least squares
  - interaction terms
  - dummy variables
  - grouped data
tags:
  - causal-inference
  - regression
  - heterogeneity
updated: 2026-06-17
---

# Grouped and dummy regression

> [!summary]
> When data is grouped or treatment effects vary by subgroup, standard OLS needs adaptation: WLS for heteroskedastic grouped data, dummy variables for categorical confounders, and interaction terms for heterogeneous treatment effects. The treatment coefficient in multi-group regression is a variance-weighted average across groups.

> [!tip] Sources
> - [Ch 6: Grouped and Dummy Regression](https://matheusfacure.github.io/python-causality-handbook/06-Grouped-and-Dummy-Regression.html)

## Weighted least squares for grouped data

Groups of different sizes produce heteroskedastic errors (smaller groups → higher variance). WLS with weights = group size corrects this:

```python
import statsmodels.formula.api as smf

# Aggregate to group means first
group_data = (df.assign(count=1)
              .groupby("education")
              .agg({"wage": "mean", "count": "count"})
              .reset_index())

# WLS weighted by group size
smf.wls("wage ~ education", data=group_data, weights=group_data["count"]).fit()
```

> [!warning]
> WLS requires group *means* as the outcome. Not sums, not medians — means.

## Interaction terms for heterogeneous effects

Without interactions (homogeneous effect):
$$
Y_i = \beta_0 + \beta_1 T_i + \beta_2 X_i + \varepsilon_i
$$

With interactions (effect varies by $X$):
$$
Y_i = \beta_0 + \beta_1 T_i + \beta_2 X_i + \beta_3 (T_i \times X_i) + \varepsilon_i
$$

- $\beta_1$ = treatment effect when $X = 0$
- $\beta_3$ = how much $X$ modifies the treatment effect
- CATE at a given $X$: $\tau(X) = \beta_1 + \beta_3 X$

```python
# Non-parallel lines (heterogeneous effect)
smf.ols("wage ~ T * IQ", data=df).fit()
```

## Non-parametric dummy regression

Treating a continuous variable as categorical (`C()`) removes functional form assumptions at the cost of statistical power:

```python
# Fully non-parametric (no linearity assumption)
smf.ols("wage ~ C(education)", data=df).fit()

# Bin continuous variable into quantiles for partial flexibility
df["IQ_bins"] = pd.qcut(df["IQ"], q=4, labels=range(4))
smf.ols("wage ~ C(education) + C(IQ_bins)", data=df).fit()
```

## Variance-weighted treatment effects

With multiple groups, the treatment coefficient is:
$$
\hat{\tau} = \sum_g w_g \cdot \hat{\tau}_g \quad \text{where } w_g \propto N_g \cdot \text{Var}(T \mid g)
$$

Groups where treatment barely varies (everyone treated or no one treated) contribute almost nothing regardless of sample size.

## Related notes

- [[linear regression for causal inference]]
- [[CATE]]
- [[Frisch-Waugh-Lovell theorem]]
- [[Adjusting for Confounding (MOC)]]
