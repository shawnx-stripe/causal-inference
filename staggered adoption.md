---
title: Staggered adoption
aliases:
  - staggered DiD
  - TWFE bias
  - Goodman-Bacon decomposition
  - forbidden comparison
  - DiD saga
tags:
  - causal-inference
  - did
  - panel-data
updated: 2026-06-17
---

# Staggered adoption

> [!summary]
> When units adopt treatment at different times, standard TWFE is biased because it uses already-treated units as controls (the "forbidden comparison"). Modern estimators (Callaway-Sant'Anna, Sun-Abraham, cohort×time interactions) fix this by estimating group-time effects separately before aggregating.

> [!tip] Sources
> - [Ch 24: The Difference-in-Differences Saga](https://matheusfacure.github.io/python-causality-handbook/24-The-Diff-in-Diff-Saga.html)
> - [08-Difference-in-Differences.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/08-Difference-in-Differences.ipynb)
> - Data: `offline_mkt_staggered.csv` (3 cohorts, heterogeneous ramping effects)

## The problem with TWFE

Goodman-Bacon (2021) decomposition:
$$
\hat{\tau}^{\text{TWFE}} = \text{VWATT} - \Delta\text{ATT}
$$

Where $\Delta\text{ATT}$ captures how much the treatment effect changes over time. TWFE is biased when effects are heterogeneous across cohorts or dynamic over time.

## The forbidden comparison

When early adopters are used as "controls" for later adopters, their *changing treatment effect* contaminates the DiD slope. If effects grow over time, this biases TWFE downward (potentially reversing the sign).

## Direction of bias

- Effects **increase** over time (maturing) → TWFE **underestimates** (biased down)
- Effects **decrease** over time (decaying) → TWFE **overestimates** (biased up)

## The fix: cohort×time interactions

Estimate separate effects for each cohort-time pair:
$$
Y_{it} = \sum_{g}\sum_{t} \tau_{gt} D_{it} + \gamma_i + \theta_t + \varepsilon_{it}
$$

## Code

```python
import statsmodels.formula.api as smf

# Naive TWFE (biased with staggered adoption)
naive = smf.ols("installs ~ treat + C(unit) + C(date)", data=df).fit()

# Fix: cohort × date interactions
df["cohort"] = df["cohort"].astype(str)
df["date"] = df["date"].astype(str)
formula = "installs ~ treat:C(cohort):C(date) + C(unit) + C(date)"
fixed = smf.ols(formula, data=df).fit()

# Extract event-study coefficients
effects = (fixed.params[fixed.params.index.str.contains("treat")]
           .reset_index()
           .rename(columns={0: "effect"}))

# Counterfactual predictions (set treat=0)
df["y_hat_0"] = fixed.predict(df.assign(treat=0))
df["effect_hat"] = df["installs"] - df["y_hat_0"]
```

## Event study specification

Use relative-time dummies (leads and lags), normalizing at period $t = -1$:

```python
# Create relative time
df["relative_time"] = (pd.to_datetime(df["date"]) - df["cohort_date"]).dt.days

# Event study regression
formula = "installs ~ C(relative_time) + C(unit) + C(date)"
es_model = smf.ols(formula, data=df.query("relative_time != -1")).fit()
```

## Related notes

- [[Difference-in-Differences (DiD)]]
- [[fixed effects]]
- [[synthetic difference-in-differences]]
- [[Panel Data and DiD (MOC)]]
