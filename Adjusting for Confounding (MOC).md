---
title: Adjusting for Confounding (MOC)
aliases:
  - Confounding Adjustment
  - Selection on Observables
  - Propensity Score Methods
tags:
  - moc
  - causal-inference
  - propensity-score
  - identification
updated: 2026-06-16
---

# Adjusting for Confounding (MOC)

> [!summary]
> Methods for estimating causal effects when treatment assignment is confounded but selection is on observables. Covers regression, propensity scores, weighting, matching, and doubly robust estimation. Corresponds to Part II of Facure's "Causal Inference in Python" (Chapters 4–5).

---

## Linear regression for causal inference

- [[linear regression for causal inference]] — when and why OLS identifies causal effects
- [[FWL theorem]] — Frisch-Waugh-Lovell, orthogonalization, partialling out
- [[omitted variable bias]] — direction and magnitude of confounding
- [[saturated regression]] — fully interacted models
- [[fixed effects]] — de-meaning and within-unit variation

---

## Propensity score

- [[propensity score]] — P(D=1|X), dimension reduction for confounders
- [[propensity score estimation]] — logistic regression, flexible models
- [[Overlap]] — positivity diagnostics, common support
- [[covariate balance]] — standardized mean differences, Love plots

---

## Matching

- [[matching]] — pairing treated/control on covariates or propensity
- [[Propensity Score Matching (PSM)]] — nearest-neighbor, caliper
- [[Coarsened Exact Matching (CEM)]] — binning covariates
- [[entropy balancing]] — moment-matching weights

---

## Inverse probability weighting

- [[Inverse Probability Weighting (IPW)]] — Horvitz-Thompson for causal effects
- [[stabilized weights]] — reducing variance of IPW
- [[weight trimming and caps]] — handling extreme weights
- [[overlap weights]] — targeting the overlap population

---

## Doubly robust estimation

- [[doubly robust estimation]] — consistency if either PS or outcome model correct
- [[Augmented Inverse Probability Weighting (AIPW)]] — augmented IPW
- [[Targeted Maximum Likelihood Estimation (TMLE)]] — substitution-based DR
- [[double machine learning]] — Neyman orthogonality, cross-fitting
- [[cross-fitting]] — sample splitting for valid ML-based inference
- [[Super Learner]] — ensemble nuisance estimation

> [!tip] Handbook chapters
> - [Ch 5: Linear Regression](https://matheusfacure.github.io/python-causality-handbook/05-The-Unreasonable-Effectiveness-of-Linear-Regression.html)
> - [Ch 10: Matching](https://matheusfacure.github.io/python-causality-handbook/10-Matching.html)
> - [Ch 11: Propensity Score](https://matheusfacure.github.io/python-causality-handbook/11-Propensity-Score.html)
> - [Ch 12: Doubly Robust Estimation](https://matheusfacure.github.io/python-causality-handbook/12-Doubly-Robust-Estimation.html)

---

## Diagnostics

- [[covariate balance]] — pre/post-weighting balance checks
- [[Love plot]] — visualizing standardized differences
- [[Overlap]] — propensity score overlap histograms
- [[effective sample size]] — weight concentration diagnostics

---

## Related notes

- [[Causal Inference (MOC)]]
- [[Foundations and Frameworks (MOC)]]
- [[Effect Heterogeneity (MOC)]]
- [[Sensitivity and Robustness (MOC)]]
