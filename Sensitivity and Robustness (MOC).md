---
title: Sensitivity and Robustness (MOC)
aliases:
  - Robustness Methods
  - Sensitivity Analysis
  - Bounds and Partial Identification
tags:
  - moc
  - causal-inference
  - sensitivity
  - bounds
  - spillovers
updated: 2026-06-16
---

# Sensitivity and Robustness (MOC)

> [!summary]
> Cross-cutting tools for assessing how robust causal conclusions are to assumption violations: bounds and partial identification, sensitivity analyses, interference/spillovers, missing data, and falsification tests. Complements the identification-specific MOCs with threat assessment and diagnostics.

---

## Bounds and partial identification

- [[Manski bounds]] — worst-case bounds under minimal assumptions
- [[Lee bounds]] — trimming bounds under monotone selection
- [[identification region]] — set of parameter values consistent with data + assumptions
- [[MIV]] · [[MTR]] · [[MTS]] — monotone instrumental variables, treatment response/selection

---

## Sensitivity analysis

- [[Rosenbaum sensitivity]] — how much unmeasured confounding would overturn results
- [[E-value]] — minimum confounding strength to explain away effect
- [[Oster's delta]] — coefficient stability under proportional selection
- [[Conley et al. plausibly exogenous]] — relaxing exclusion restriction

---

## Interference and spillovers

- [[interference]] — outcomes depend on others' treatment (SUTVA violation)
- [[spillovers]] · [[No spillovers]] — presence/absence of cross-unit effects
- [[exposure mapping]] — summarizing neighbors' treatment exposure
- [[partial interference]] — interference within but not across clusters
- [[network interference]] — effects through network connections
- [[reflection problem]] — endogeneity in peer effects models
- [[halo test]] — testing for spillovers where none expected

---

## Missing data and attrition

- [[Attrition]] — differential dropout across treatment arms
- [[selection bias]] — systematic differences in who is observed
- [[Inverse Probability of Censoring Weighting (IPCW)]] — reweighting for censoring
- [[Heckman correction]] — selection model approach
- [[truncation by death]] — outcomes undefined for some units

---

## Falsification and placebo tests

- [[placebo test]] — null effects where none expected (placebo outcomes, dates, units)
- [[falsification tests]] — broader category of "should be zero" checks
- [[pre-trends]] — pre-treatment dynamics should be flat (DiD context)
- [[McCrary test]] · [[density test]] — no manipulation (RDD context)
- [[balance check]] — covariate balance across treatment/control

---

## Robustness practices

- [[robustness checks]] — alternative specifications, windows, controls
- [[multiple testing control]] — adjusting for many comparisons
- [[pre-registration]] — committing to analysis ex ante

---

## Related notes

- [[Causal Inference (MOC)]]
- [[Foundations and Frameworks (MOC)]]
- [[Adjusting for Confounding (MOC)]]
- [[Panel Data and DiD (MOC)]]
- [[Design-Based Identification (MOC)]]
