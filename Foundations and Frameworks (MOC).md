---
title: Foundations and Frameworks (MOC)
aliases:
  - Causal Foundations
  - Potential Outcomes Framework
  - Graphical Causal Models
tags:
  - moc
  - causal-inference
  - potential-outcomes
  - dags
  - identification
updated: 2026-06-16
---

# Foundations and Frameworks (MOC)

> [!summary]
> Core frameworks for defining and identifying causal effects: potential outcomes (Rubin), graphical models (Pearl), and the assumptions that connect design to estimands. Corresponds to Part I of Facure's "Causal Inference in Python" (Chapters 1–3).

---

## Potential outcomes framework

- [[potential outcomes]] — Neyman-Rubin model, counterfactuals
- [[individual treatment effect]] — fundamental problem of causal inference
- [[Average Treatment Effect (ATE)]] · [[Average Treatment Effect on the Treated (ATT)]] · [[Average Treatment Effect on the Untreated (ATU)]]
- [[consistency]] — well-defined treatment versions
- [[Stable Unit Treatment Value Assumption (SUTVA)]] — no interference, no hidden versions

---

## Causal quantities and estimands

- [[Average Treatment Effect (ATE)]] — population-level
- [[Average Treatment Effect on the Treated (ATT)]] — effect on those who received treatment
- [[Local Average Treatment Effect (LATE)]] — complier-specific (IV setting)
- [[Intent-to-Treat (ITT)]] — effect of assignment regardless of compliance
- [[Treatment-on-the-Treated (TOT)]] — compliance-adjusted

---

## Graphical causal models

- [[causal DAGs]] — directed acyclic graphs, Pearl's framework
- [[d-separation]] — conditional independence reading from graphs
- [[back-door criterion]] — sufficient adjustment sets
- [[front-door criterion]] — identification through mediators
- [[collider bias]] — conditioning on common effects
- [[bad controls]] — post-treatment conditioning, mediator bias

---

## Identification

- [[identification]] — mapping assumptions to estimands via observables
- [[Unconfoundedness]] — conditional independence of treatment and potential outcomes
- [[Overlap]] — positivity, common support
- [[independence assumption]] — exchangeability

---

## Bias

- [[bias equation]] — decomposing observed associations
- [[selection bias]] — systematic differences between treated and control
- [[confounding]] — open back-door paths
- [[omitted variable bias]] — regression perspective on confounding

---

## Randomized experiments

- [[randomized controlled trial (RCT)]] — brute-force independence
- [[hypothesis testing]] · [[p-value]] · [[confidence interval]]
- [[power]] · [[sample size calculation]] · [[Minimum Detectable Effect (MDE)]]
- [[standard error]] — precision of estimates

---

## Related notes

- [[Causal Inference (MOC)]]
- [[Adjusting for Confounding (MOC)]]
- [[Design-Based Identification (MOC)]]
- [[Sensitivity and Robustness (MOC)]]
