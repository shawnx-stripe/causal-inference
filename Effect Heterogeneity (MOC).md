---
title: Effect Heterogeneity (MOC)
aliases:
  - Treatment Effect Heterogeneity
  - CATE Methods
  - Heterogeneous Treatment Effects
  - HTE
tags:
  - moc
  - causal-inference
  - heterogeneity
  - meta-learners
updated: 2026-06-16
---

# Effect Heterogeneity (MOC)

> [!summary]
> Methods for discovering and estimating how treatment effects vary across individuals and contexts. From subgroup analysis to ML-based CATE estimation, meta-learners, and optimal policy learning. Corresponds to Part III of Facure's "Causal Inference in Python" (Chapters 6–7).

---

## From ATE to CATE

- [[treatment effect heterogeneity]] — why averages can mislead
- [[CATE]] — conditional average treatment effect τ(x)
- [[quantile treatment effects]] — effects across the outcome distribution
- [[marginal treatment effect (MTE)]] — selection-margin heterogeneity

---

## Meta-learners

- [[meta-learners]] — framework for CATE estimation
- [[S-learner]] — single model, predict under treatment/control
- [[T-learner]] — separate models per treatment arm
- [[X-learner]] — cross-imputation, good for imbalanced treatment
- [[R-learner]] — residual-on-residual, Robinson decomposition

---

## ML-based CATE estimation

- [[double machine learning]] — orthogonalized estimation with cross-fitting
- [[causal forests]] — adaptive partitioning for heterogeneous effects
- [[Generalized Random Forests (GRF)]] — local moment-based forest framework
- [[honest inference]] — sample splitting for valid CIs on τ(x)
- [[cross-fitting]] — avoiding overfitting in nuisance estimation

---

## Evaluating heterogeneity

- [[best linear predictor]] — BLP calibration test (β=1 ↔ well-calibrated)
- [[GATES]] — sorted group average treatment effects
- [[cumulative gain curve]] — targeting efficiency visualization
- [[uplift metrics]] — Qini coefficient, AUUC

---

## Policy learning and targeting

- [[policy learning]] — optimal treatment assignment rules
- [[policy tree]] — interpretable decision rules
- [[policy value]] — welfare from a treatment rule
- [[uplift]] — ranking individuals by predicted benefit
- [[off-policy evaluation]] — estimating policy value from logged data (IPS, DR, SNIPS)
- [[targeting with constraints]] — budget and fairness constraints

---

## Related notes

- [[Causal Inference (MOC)]]
- [[Adjusting for Confounding (MOC)]]
- [[Foundations and Frameworks (MOC)]]
- [[Sensitivity and Robustness (MOC)]]
