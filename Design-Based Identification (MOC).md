---
title: Design-Based Identification (MOC)
aliases:
  - IV and RDD
  - Instrumental Variables and Discontinuity
  - Natural Experiments
tags:
  - moc
  - causal-inference
  - iv
  - rdd
  - identification
updated: 2026-06-16
---

# Design-Based Identification (MOC)

> [!summary]
> Identification strategies that exploit specific sources of quasi-random variation: instrumental variables, regression discontinuity, and spatial/temporal experimental designs. Corresponds to Part V of Facure's "Causal Inference in Python" (Chapters 10–11).

---

## Instrumental variables

- [[Instrumental Variables (IV)]] — using exogenous variation to identify causal effects
- [[Two-Stage Least Squares (2SLS)]] — standard IV estimator
- [[Wald estimator]] — ratio of reduced form to first stage
- [[Local Average Treatment Effect (LATE)]] — effect on compliers

---

## IV assumptions

- [[exclusion restriction]] — instrument affects outcome only through treatment
- [[relevance]] — instrument predicts treatment (strong first stage)
- [[monotonicity]] — no defiers
- [[independence]] — instrument is as-good-as-random

---

## Compliance types

- [[noncompliance]] — when assignment ≠ treatment received
- [[compliers]] — those who follow assignment
- [[always-takers]] · [[never-takers]] · [[defiers]]
- [[Intent-to-Treat (ITT)]] — effect of assignment
- [[Treatment-on-the-Treated (TOT)]] — compliance-adjusted effect

---

## IV diagnostics and extensions

- [[weak instruments]] — bias toward OLS when first stage is weak
- [[first stage]] — F-statistic, Stock-Yogo, Montiel Olea-Pflueger
- [[Anderson–Rubin]] — weak-instrument-robust inference
- [[Local IV]] · [[marginal treatment effect (MTE)]] — continuous instruments
- [[control function]] — alternative to 2SLS

---

## Regression discontinuity

- [[Regression Discontinuity Design (RDD)]] — exploiting threshold rules
- [[sharp RDD]] — deterministic treatment at cutoff
- [[fuzzy RDD]] — probabilistic jump at cutoff (IV interpretation)
- [[running variable]] · [[cutoff]]

---

## RDD estimation

- [[local linear regression]] — nonparametric estimation at boundary
- [[local polynomial regression]] — higher-order fits
- [[bandwidth selection]] — MSE-optimal, bias-corrected
- [[rdrobust]] — robust RD inference package

---

## RDD diagnostics

- [[McCrary test]] · [[density test]] — manipulation of running variable
- [[RD plot]] — visual discontinuity check
- [[donut RDD]] — excluding observations near cutoff
- [[covariate continuity]] — balance at the threshold

---

## Geo and switchback experiments

- [[geo experiment]] — geographic treatment assignment, SC-based design optimization
- [[switchback experiment]] — temporal rotation of treatment, carryover effects
- [[stepped-wedge design]] — sequential rollout across clusters
- [[buffer zone design]] — spatial separation to control spillovers
- [[randomized saturation design]] — cluster-level treatment intensity variation

> [!tip] Handbook chapters
> - [Ch 8: Instrumental Variables](https://matheusfacure.github.io/python-causality-handbook/08-Instrumental-Variables.html)
> - [Ch 9: Non Compliance and LATE](https://matheusfacure.github.io/python-causality-handbook/09-Non-Compliance-and-LATE.html)
> - [Ch 16: Regression Discontinuity Design](https://matheusfacure.github.io/python-causality-handbook/16-Regression-Discontinuity-Design.html)

---

## Related notes

- [[Causal Inference (MOC)]]
- [[Foundations and Frameworks (MOC)]]
- [[Panel Data and DiD (MOC)]]
- [[Sensitivity and Robustness (MOC)]]
