---
title: Causal Inference Knowledge Vault
aliases:
  - Home
  - Index
tags:
  - moc
  - causal-inference
updated: 2026-06-17
---

# Causal Inference Knowledge Vault

> [!summary]
> Personal Obsidian vault for causal inference — methods, formulas, Python code, and practical intuition. Built from [Causal Inference for the Brave and True](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) (Facure) with code from the [companion repo](https://github.com/matheusfacure/causal-inference-in-python-code).

---

## Quick start

Start at [[Causal Inference (MOC)]] — it links to everything else.

For data: [[simulated data]] has download URLs for all 22 datasets with known causal structures.

---

## Vault structure

> [!tip] MOC hierarchy
> - [[Causal Inference (MOC)]] — master hub
>   - [[Foundations and Frameworks (MOC)]] — potential outcomes, DAGs, identification, bias
>   - [[Adjusting for Confounding (MOC)]] — regression, propensity score, IPW, DR, matching
>   - [[Effect Heterogeneity (MOC)]] — CATE, meta-learners, DML, policy learning
>   - [[Panel Data and DiD (MOC)]] — DiD, fixed effects, staggered, synthetic control
>   - [[Design-Based Identification (MOC)]] — IV, RDD, geo experiments, switchback
>   - [[Sensitivity and Robustness (MOC)]] — bounds, sensitivity analysis, spillovers

42 notes total. Every concept page has:
- `> [!summary]` with one-line definition
- Key formulas in LaTeX
- Python code (statsmodels, sklearn, lightgbm, cvxpy, linearmodels)
- `> [!tip] Sources` with backlinks to original notebook/chapter
- Wikilinks to related notes

---

## Source mapping

> [!note] Handbook → Vault notes
> Each chapter of [Causal Inference for the Brave and True](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) maps to one or more vault notes.

### Part I — Foundations

| Ch | Handbook Chapter | Vault Note(s) |
|----|-----------------|---------------|
| 01 | Introduction to Causality | [[potential outcomes]], [[bias equation]] |
| 02 | Randomised Experiments | [[randomized experiments]] |
| 03 | Stats Review | [[randomized experiments]] |
| 04 | Graphical Causal Models | [[causal DAGs]], [[identification]] |

### Part I — Adjusting for Bias

| Ch | Handbook Chapter | Vault Note(s) |
|----|-----------------|---------------|
| 05 | Linear Regression | [[linear regression for causal inference]], [[Frisch-Waugh-Lovell theorem]] |
| 06 | Grouped/Dummy Regression | [[grouped and dummy regression]] |
| 07 | Beyond Confounders | [[good and bad controls]] |
| 08 | Instrumental Variables | [[Instrumental Variables (IV)]] |
| 09 | Non-Compliance/LATE | [[Instrumental Variables (IV)]] |
| 10 | Matching | [[matching]], [[propensity score matching]] |
| 11 | Propensity Score | [[propensity score]], [[Inverse Probability Weighting (IPW)]], [[positivity]] |
| 12 | Doubly Robust | [[doubly robust estimation]] |

### Part I — Panel and Design-Based

| Ch | Handbook Chapter | Vault Note(s) |
|----|-----------------|---------------|
| 13 | Difference-in-Differences | [[Difference-in-Differences (DiD)]] |
| 14 | Panel Data/Fixed Effects | [[fixed effects]] |
| 15 | Synthetic Control | [[Synthetic Control]], [[synthetic difference-in-differences]] |
| 16 | Regression Discontinuity | [[Regression Discontinuity Design (RDD)]] |

### Part II — Heterogeneity and Personalization

| Ch | Handbook Chapter | Vault Note(s) |
|----|-----------------|---------------|
| 17 | Predictive Models 101 | [[prediction vs causation]] |
| 18 | HTE & Personalization | [[CATE]] |
| 19 | Evaluating Causal Models | [[evaluating CATE]] |
| 20 | Plug-and-Play Estimators | [[target transformation]] |
| 21 | Meta-Learners | [[T-learner]], [[X-learner]], [[S-learner]] |
| 22 | Debiased/Orthogonal ML | [[double machine learning]] |
| 23 | Challenges with HTE | [[binary outcome nonlinearity]] |
| 24 | The DiD Saga | [[staggered adoption]] |
| 25 | Synthetic DiD | [[synthetic difference-in-differences]] |

### Code Notebooks

> [!note] [Code repository](https://github.com/matheusfacure/causal-inference-in-python-code) → Vault notes

| Notebook | Vault Note(s) |
|----------|---------------|
| 01-Introduction | [[potential outcomes]], [[bias equation]] |
| 02-Experiments | [[randomized experiments]] |
| 03-Graphical-Models | [[causal DAGs]], [[identification]] |
| 04-Linear-Regression | [[linear regression for causal inference]], [[Frisch-Waugh-Lovell theorem]] |
| 05-Propensity-Score | [[propensity score]], [[Inverse Probability Weighting (IPW)]], [[propensity score matching]], [[positivity]] |
| 06-Effect-Heterogeneity | [[CATE]], [[evaluating CATE]] |
| 07-Meta-Learners | [[T-learner]], [[X-learner]], [[S-learner]], [[double machine learning]] |
| 08-DiD | [[Difference-in-Differences (DiD)]] |
| 09-Synthetic-Control | [[Synthetic Control]], [[synthetic difference-in-differences]] |
| 10-Geo-Switchback | [[geo experiment]], [[switchback experiment]] |
| 11-IV-and-RDD | [[Instrumental Variables (IV)]], [[Regression Discontinuity Design (RDD)]] |
| Simulated-Data | [[simulated data]] |

---

## Data

All 22 simulated datasets with known causal structures — see [[simulated data]] for the full index, `pd.read_csv()` download URLs, and DGP code.

---

## References

- Facure, [*Causal Inference for the Brave and True*](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) — primary source (25 chapters + appendices)
- Facure, *Causal Inference in Python* (O'Reilly, 2023) — the published book
- [Code repository](https://github.com/matheusfacure/causal-inference-in-python-code) — 11 notebooks + data generation
- Angrist & Pischke, *Mostly Harmless Econometrics*
- Cunningham, *Causal Inference: The Mixtape*

---

## Changelog

> [!example] Recent changes

**2026-06-17** — Initial vault creation
- 7 MOC pages following Facure's 5-part structure
- 33 concept pages covering all 25 handbook chapters + appendices
- Python code from notebooks with source backlinks in every page
- [[simulated data]] page with download URLs for all 22 datasets
- Gap analysis against full handbook; filled Ch 6, 7, 10, 14, 17, 20, 23, 24
- Enriched [[Instrumental Variables (IV)]] (unit-conversion framing), [[Regression Discontinuity Design (RDD)]] (kernel weighting), [[double machine learning]] (overfitting risks, monotonic constraints)
- Added remaining details: expit DGP, SE of difference, surrogate confounders, weight clipping, SDID regularization
