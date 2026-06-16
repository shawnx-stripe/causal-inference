---
title: Causal Inference (MOC)
aliases:
  - Causal Inference
  - CI hub
  - Causal Inference (MOC)
tags:
  - moc
  - causal-inference
updated: 2026-06-16
---

# Causal Inference (MOC)

> [!summary]
> Master hub for causal inference methods. Organized following the pedagogical arc: foundations → confounding adjustment → heterogeneity → panel methods → design-based identification, plus cross-cutting sensitivity and robustness tools.

---

## Quick-start workflow

1. Define the causal question and target estimand ([[Average Treatment Effect (ATE)]], [[ATT]], [[LATE]])
2. Choose identification strategy ([[Unconfoundedness]], [[parallel trends]], [[exclusion restriction]], continuity)
3. Check assumptions ([[SUTVA]], [[Overlap]], [[no anticipation]])
4. Select estimator aligned with design
5. Run diagnostics and robustness checks
6. Report with limitations

---

## Part I — Foundations

→ [[Foundations and Frameworks (MOC)]]

- [[potential outcomes]] · [[causal DAGs]] · [[identification]]
- [[SUTVA]] · [[Unconfoundedness]] · [[Overlap]]
- [[selection bias]] · [[collider bias]] · [[bias equation]]
- Randomized experiments · hypothesis testing · power

---

## Part II — Adjusting for confounding

→ [[Adjusting for Confounding (MOC)]]

- [[linear regression for causal inference]] · [[FWL theorem]] · [[omitted variable bias]]
- [[propensity score]] · [[matching]] · [[Inverse Probability Weighting (IPW)]]
- [[Doubly Robust estimators]] · [[AIPW]] · [[TMLE]] · [[double machine learning]]

---

## Part III — Effect heterogeneity

→ [[Effect Heterogeneity (MOC)]]

- [[treatment effect heterogeneity]] · [[CATE]]
- [[meta-learners]] (S, T, X, R) · [[causal forests]] · [[GRF]]
- [[policy learning]] · [[uplift]] · [[off-policy evaluation]]

---

## Part IV — Panel data and DiD

→ [[Panel Data and DiD (MOC)]]

- [[Difference-in-Differences (DiD)]] · [[two-way fixed effects]] · [[event study]]
- [[staggered adoption]] · [[Callaway–Sant'Anna estimator]] · [[Sun–Abraham estimator]]
- [[Synthetic Control]] · [[synth-DiD]]

---

## Part V — Design-based identification

→ [[Design-Based Identification (MOC)]]

- [[Instrumental Variables (IV)]] · [[Two-Stage Least Squares (2SLS)]] · [[LATE]]
- [[Regression Discontinuity Design (RDD)]] · [[sharp RDD]] · [[fuzzy RDD]]
- [[geo experiment]] · [[switchback experiment]]

---

## Cross-cutting — Sensitivity and robustness

→ [[Sensitivity and Robustness (MOC)]]

- [[Manski bounds]] · [[Lee bounds]] · [[E-value]] · [[Rosenbaum sensitivity]]
- [[spillovers]] · [[interference]] · [[Attrition]]
- [[placebo test]] · [[falsification tests]]

---

## Reading list

- Facure, *Causal Inference in Python* (O'Reilly, 2023)
- Angrist & Pischke, *Mostly Harmless Econometrics*
- Imbens & Rubin, *Causal Inference for Statistics, Social, and Biomedical Sciences*
- Hernán & Robins, *Causal Inference: What If*
- Cunningham, *Causal Inference: The Mixtape*
- Athey & Imbens, "Machine Learning Methods Economists Should Know About"

---

## Related notes

- [[Foundations and Frameworks (MOC)]]
- [[Adjusting for Confounding (MOC)]]
- [[Effect Heterogeneity (MOC)]]
- [[Panel Data and DiD (MOC)]]
- [[Design-Based Identification (MOC)]]
- [[Sensitivity and Robustness (MOC)]]
