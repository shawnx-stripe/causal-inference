---
title: Linear regression for causal inference
aliases:
  - OLS for causal effects
  - regression adjustment
  - causal regression
tags:
  - causal-inference
  - identification
  - regression
updated: 2026-06-17
---

# Linear regression for causal inference

> [!summary]
> OLS regression can identify causal effects when the right covariates are included: it adjusts for confounders by partialling out their influence. The coefficient on treatment is causal under [[Unconfoundedness]], but wrong controls can introduce bias (Simpson's paradox, [[collider bias]]).

> [!tip] Sources
> - [04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb)
> - [Ch 5: The Unreasonable Effectiveness of Linear Regression](https://matheusfacure.github.io/python-causality-handbook/05-The-Unreasonable-Effectiveness-of-Linear-Regression.html)
> - [Ch 6: Grouped and Dummy Regression](https://matheusfacure.github.io/python-causality-handbook/06-Grouped-and-Dummy-Regression.html)
> - Data: `risk_data.csv` (credit limit → default, confounded by credit scores and wage), `rec_ab_test.csv` (A/B test baseline)

## Two interpretations of causal adjustment

| | RCT (severing) | Regression (freezing) |
|---|---|---|
| Mechanism | Removes all arrows *into* T — nothing causes T | Holds confounders constant (freezes W=w) |
| Graphically | Severs edges into treatment node | Conditions on confounder nodes |
| Result | T is independent of everything | T is independent of Y *given* W |

Both achieve identification but via different operations on the DAG.

## When OLS is causal

Under CIA + linearity, regressing $Y$ on $T$ and confounders $X$:
$$
Y_i = \alpha + \tau T_i + X_i'\beta + \varepsilon_i
$$

$\hat{\tau}$ estimates the ATE if:
1. All backdoor paths are blocked by $X$ ([[identification]])
2. No post-treatment variables in $X$ (no [[bad controls]])
3. [[positivity]] holds

## Simpson's paradox

Including vs. omitting a confounder can flip the sign of $\hat{\tau}$. This is not a paradox — it's omitted variable bias at work.

## Omitted variable bias

If a confounder $Z$ is omitted:
$$
\text{Bias}(\hat{\tau}) = \gamma \cdot \delta
$$
where $\gamma$ = effect of $Z$ on $Y$, $\delta$ = association of $Z$ with $T$ after partialling out included $X$.

## Code

```python
import statsmodels.formula.api as smf

# Biased: omit confounders
biased = smf.ols("default ~ credit_limit", data=risk_data).fit()

# Adjusted: include confounders (blocks backdoor paths)
adjusted = smf.ols(
    "default ~ credit_limit + wage + credit_score1 + credit_score2",
    data=risk_data
).fit()

# In A/B tests, OLS recovers the same ATE as difference-in-means
ab_model = smf.ols("watch_time ~ C(recommender)", data=ab_data).fit()
```

## Related notes

- [[Frisch-Waugh-Lovell theorem]]
- [[identification]]
- [[propensity score]]
- [[Adjusting for Confounding (MOC)]]
