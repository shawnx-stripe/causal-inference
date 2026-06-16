---
title: Double machine learning
aliases:
  - DML
  - debiased ML
  - R-learner
  - Robinson decomposition
  - orthogonal ML
tags:
  - causal-inference
  - heterogeneity
  - meta-learners
  - doubly-robust
updated: 2026-06-17
---

# Double machine learning

> [!summary]
> Double/Debiased Machine Learning (DML) uses the [[Frisch-Waugh-Lovell theorem]] with ML: residualize both treatment and outcome against confounders using flexible learners, then regress residuals on residuals. Cross-fitting prevents overfitting. Extends naturally to CATE estimation via the R-learner objective.

## ATE estimation (Robinson decomposition)

**Step 1 — Debias**: $\tilde{T}_i = T_i - \hat{\mathbb{E}}[T \mid X_i]$ (treatment residual)

**Step 2 — Denoise**: $\tilde{Y}_i = Y_i - \hat{\mathbb{E}}[Y \mid X_i]$ (outcome residual)

**Step 3 — Estimate**: OLS of $\tilde{Y}$ on $\tilde{T}$:
$$
\hat{\tau}_{\text{DML}} = \frac{\sum_i \tilde{T}_i \, \tilde{Y}_i}{\sum_i \tilde{T}_i^2}
$$

## Cross-fitting

> [!tip]
> Use out-of-fold predictions for residuals to avoid overfitting bias. `sklearn.model_selection.cross_val_predict` with `cv=5` computes leave-fold-out predictions in one call.

## CATE extension (R-learner)

Transform the ATE residuals into a weighted regression problem:
$$
Y^* = \frac{\tilde{Y}_i}{\tilde{T}_i}, \quad w_i = \tilde{T}_i^2
$$

Fit any ML model on $(X, Y^*)$ with sample weights $w$ to get $\hat{\tau}(x)$.

## Code

```python
from sklearn.model_selection import cross_val_predict
from lightgbm import LGBMRegressor
import statsmodels.api as sm

# Cross-fitted residuals
debias_m = LGBMRegressor(n_estimators=100)
denoise_m = LGBMRegressor(n_estimators=100)

t_hat = cross_val_predict(debias_m, train[X], train[T], cv=5)
y_hat = cross_val_predict(denoise_m, train[X], train[Y], cv=5)

t_res = train[T].values - t_hat
y_res = train[Y].values - y_hat

# ATE via OLS on residuals
ate_model = sm.OLS(y_res, t_res).fit()
ate_dml = ate_model.params[0]

# CATE via weighted regression (R-learner)
y_star = y_res / t_res
weights = t_res ** 2

cate_model = LGBMRegressor(n_estimators=100)
cate_model.fit(train[X], y_star, sample_weight=weights)
cate_dml = cate_model.predict(test[X])
```

## Why DML works

- **Neyman orthogonality**: the score is insensitive to small errors in nuisance models
- **Cross-fitting**: prevents the nuisance ML from overfitting to the estimation sample
- Combines the debiasing insight of [[Frisch-Waugh-Lovell theorem|FWL]] with the flexibility of ML

## Related notes

- [[Frisch-Waugh-Lovell theorem]]
- [[S-learner]]
- [[CATE]]
- [[evaluating CATE]]
- [[cross-fitting]]
- [[Effect Heterogeneity (MOC)]]
