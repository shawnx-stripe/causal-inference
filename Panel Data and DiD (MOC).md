---
title: Panel Data and DiD (MOC)
aliases:
  - Difference-in-Differences
  - DiD Methods
  - Panel Methods for Causal Inference
tags:
  - moc
  - causal-inference
  - did
  - panel-data
  - synthetic-control
updated: 2026-06-16
---

# Panel Data and DiD (MOC)

> [!summary]
> Causal inference with repeated observations over time: difference-in-differences, event studies, staggered adoption, and synthetic control methods. Corresponds to Part IV of Facure's "Causal Inference in Python" (Chapters 8–9).

---

## Canonical difference-in-differences

- [[Difference-in-Differences (DiD)]] — two-group, two-period design
- [[DiD estimator]] — regression and manual computation
- [[two-way fixed effects]] — unit + time FE specification
- [[Fixed effects]] — within-unit variation, de-meaning

---

## Identification assumptions

- [[parallel trends assumption]] — key identifying assumption
- [[no anticipation]] — treatment doesn't affect pre-period outcomes
- [[composition]] — stable group membership over time
- [[strict exogeneity]] — no feedback from outcomes to future treatment

---

## Event studies and dynamics

- [[event study]] — leads and lags around treatment
- [[pre-trends]] — testing parallel trends via pre-treatment coefficients
- [[Anticipatory effects]] — effects before official treatment date

---

## Staggered adoption and modern estimators

- [[staggered adoption]] — units treated at different times
- [[Goodman–Bacon decomposition]] — what TWFE actually estimates
- [[Callaway–Sant'Anna estimator]] — group-time ATTs, DR variant
- [[Sun–Abraham estimator]] — interaction-weighted estimator
- [[Borusyak–Jaravel–Spiess (imputation)]] — imputation-based DiD
- [[Gardner DID2S]] — two-stage DiD
- [[de Chaisemartin–D'Haultfœuille]] — heterogeneity-robust estimator
- [[group-time average treatment effect]] — disaggregated effects before aggregation

---

## Extensions

- [[Triple Differences (DDD)]] — additional differencing layer
- [[fuzzy DiD]] — non-sharp treatment adoption
- [[Doubly Robust DiD]] — combining PS and outcome models in DiD
- [[drdid]] — doubly robust DiD implementation

---

## Synthetic control

- [[Synthetic Control]] — weighted combination of untreated units
- [[synth-DiD]] — synthetic difference-in-differences hybrid
- [[matrix completion for panels]] — nuclear norm regularization
- [[RMSPE]] — pre-treatment fit quality

---

## Inference in DiD

- [[clustered standard errors]] — clustering at treatment assignment level
- [[few-cluster corrections]] — wild bootstrap, effective clusters
- [[wild cluster bootstrap]] — inference with small number of clusters

---

## Related notes

- [[Causal Inference (MOC)]]
- [[Foundations and Frameworks (MOC)]]
- [[Design-Based Identification (MOC)]]
- [[Sensitivity and Robustness (MOC)]]
