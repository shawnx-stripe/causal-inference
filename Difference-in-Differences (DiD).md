---
title: Difference-in-Differences (DiD)
aliases:
  - DiD
  - diff-in-diff
  - difference in differences
tags:
  - causal-inference
  - did
  - panel-data
updated: 2026-06-17
---

# Difference-in-Differences (DiD)

> [!summary]
> DiD estimates causal effects by comparing the change in outcomes over time between treated and control groups. The key identifying assumption is [[parallel trends]]: absent treatment, both groups would have evolved similarly. Removes time-invariant confounders by differencing.

## Canonical 2×2 DiD

$$
\hat{\tau}_{\text{DiD}} = (\bar{Y}_T^{\text{post}} - \bar{Y}_T^{\text{pre}}) - (\bar{Y}_C^{\text{post}} - \bar{Y}_C^{\text{pre}})
$$

Equivalently (outcome growth form):
$$
\hat{\tau} = \mathbb{E}[\Delta Y \mid \text{treated}] - \mathbb{E}[\Delta Y \mid \text{control}]
$$

## Identification assumptions

- **[[parallel trends]]**: $\mathbb{E}[Y(0)_{\text{post}} - Y(0)_{\text{pre}} \mid T=1] = \mathbb{E}[Y(0)_{\text{post}} - Y(0)_{\text{pre}} \mid T=0]$
- **No anticipation**: Treatment doesn't affect outcomes before it's implemented
- **SUTVA**: No [[spillovers]] across groups
- **Stable composition**: Same units in both periods (no differential attrition)

## OLS implementation

DiD as an interaction term:
$$
Y_{it} = \alpha + \beta_1 \cdot \text{treated}_i + \beta_2 \cdot \text{post}_t + \tau \cdot (\text{treated}_i \times \text{post}_t) + \varepsilon_{it}
$$

$\hat{\tau}$ = the DiD estimate.

## Fixed effects formulation

With unit and time fixed effects (equivalent for balanced panels):
$$
Y_{it} = \alpha_i + \gamma_t + \tau \cdot (\text{treated}_i \times \text{post}_t) + \varepsilon_{it}
$$

## Code

```python
import statsmodels.formula.api as smf

# Interaction form
did_model = smf.ols("downloads ~ treated * post", data=panel).fit()
att = did_model.params["treated:post"]

# Fixed effects form
fe_model = smf.ols(
    "downloads ~ treated:post + C(city) + C(date)",
    data=panel
).fit()

# Manual computation
means = panel.groupby(["treated", "post"])["downloads"].mean()
did_manual = (means[1,1] - means[1,0]) - (means[0,1] - means[0,0])
```

## Related notes

- [[parallel trends]]
- [[Synthetic Control]]
- [[synthetic difference-in-differences]]
- [[staggered adoption]]
- [[Panel Data and DiD (MOC)]]
