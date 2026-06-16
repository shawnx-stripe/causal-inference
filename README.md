# Causal Inference Knowledge Vault

Personal Obsidian vault for causal inference — methods, formulas, Python code, and practical intuition. Built from [Causal Inference for the Brave and True](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) (Facure) with code from the [companion repo](https://github.com/matheusfacure/causal-inference-in-python-code).

## Quick start

Open this folder as an Obsidian vault. Start at `Causal Inference (MOC).md` — it links to everything else.

## What's in here

```
Causal Inference (MOC)          ← start here
├── Foundations and Frameworks  ← potential outcomes, DAGs, identification, bias
├── Adjusting for Confounding   ← regression, propensity score, IPW, DR, matching
├── Effect Heterogeneity        ← CATE, meta-learners, DML, policy learning
├── Panel Data and DiD          ← DiD, fixed effects, staggered, synthetic control
├── Design-Based Identification ← IV, RDD, geo experiments, switchback
└── Sensitivity and Robustness  ← bounds, sensitivity analysis, spillovers
```

42 notes total. Every concept page has:
- Summary callout with one-line definition
- Key formulas in LaTeX
- Python code you can copy (statsmodels, sklearn, lightgbm, cvxpy, linearmodels)
- Source backlinks to the original notebook/chapter
- Wikilinks to related notes

## Source mapping

<details>
<summary>Handbook chapters → vault notes (click to expand)</summary>

| Ch | Title | Note(s) |
|----|-------|---------|
| 01 | [Introduction to Causality](https://matheusfacure.github.io/python-causality-handbook/01-Introduction-To-Causality.html) | `potential outcomes`, `bias equation` |
| 02 | [Randomised Experiments](https://matheusfacure.github.io/python-causality-handbook/02-Randomised-Experiments.html) | `randomized experiments` |
| 03 | [Stats Review](https://matheusfacure.github.io/python-causality-handbook/03-Stats-Review-The-Most-Dangerous-Equation.html) | `randomized experiments` |
| 04 | [Graphical Causal Models](https://matheusfacure.github.io/python-causality-handbook/04-Graphical-Causal-Models.html) | `causal DAGs`, `identification` |
| 05 | [Linear Regression](https://matheusfacure.github.io/python-causality-handbook/05-The-Unreasonable-Effectiveness-of-Linear-Regression.html) | `linear regression for causal inference`, `Frisch-Waugh-Lovell theorem` |
| 06 | [Grouped/Dummy Regression](https://matheusfacure.github.io/python-causality-handbook/06-Grouped-and-Dummy-Regression.html) | `grouped and dummy regression` |
| 07 | [Beyond Confounders](https://matheusfacure.github.io/python-causality-handbook/07-Beyond-Confounders.html) | `good and bad controls` |
| 08 | [Instrumental Variables](https://matheusfacure.github.io/python-causality-handbook/08-Instrumental-Variables.html) | `Instrumental Variables (IV)` |
| 09 | [Non-Compliance/LATE](https://matheusfacure.github.io/python-causality-handbook/09-Non-Compliance-and-LATE.html) | `Instrumental Variables (IV)` |
| 10 | [Matching](https://matheusfacure.github.io/python-causality-handbook/10-Matching.html) | `matching`, `propensity score matching` |
| 11 | [Propensity Score](https://matheusfacure.github.io/python-causality-handbook/11-Propensity-Score.html) | `propensity score`, `Inverse Probability Weighting (IPW)`, `positivity` |
| 12 | [Doubly Robust](https://matheusfacure.github.io/python-causality-handbook/12-Doubly-Robust-Estimation.html) | `doubly robust estimation` |
| 13 | [Difference-in-Differences](https://matheusfacure.github.io/python-causality-handbook/13-Difference-in-Differences.html) | `Difference-in-Differences (DiD)` |
| 14 | [Panel Data/Fixed Effects](https://matheusfacure.github.io/python-causality-handbook/14-Panel-Data-and-Fixed-Effects.html) | `fixed effects` |
| 15 | [Synthetic Control](https://matheusfacure.github.io/python-causality-handbook/15-Synthetic-Control.html) | `Synthetic Control`, `synthetic difference-in-differences` |
| 16 | [Regression Discontinuity](https://matheusfacure.github.io/python-causality-handbook/16-Regression-Discontinuity-Design.html) | `Regression Discontinuity Design (RDD)` |
| 17 | [Predictive Models 101](https://matheusfacure.github.io/python-causality-handbook/17-Predictive-Models-101.html) | `prediction vs causation` |
| 18 | [HTE & Personalization](https://matheusfacure.github.io/python-causality-handbook/18-Heterogeneous-Treatment-Effects-and-Personalization.html) | `CATE` |
| 19 | [Evaluating Causal Models](https://matheusfacure.github.io/python-causality-handbook/19-Evaluating-Causal-Models.html) | `evaluating CATE` |
| 20 | [Plug-and-Play Estimators](https://matheusfacure.github.io/python-causality-handbook/20-Plug-and-Play-Estimators.html) | `target transformation` |
| 21 | [Meta-Learners](https://matheusfacure.github.io/python-causality-handbook/21-Meta-Learners.html) | `T-learner`, `X-learner`, `S-learner` |
| 22 | [Debiased/Orthogonal ML](https://matheusfacure.github.io/python-causality-handbook/22-Debiased-Orthogonal-Machine-Learning.html) | `double machine learning` |
| 23 | [Challenges with HTE](https://matheusfacure.github.io/python-causality-handbook/23-Challenges-with-Effect-Heterogeneity-and-Nonlinearity.html) | `binary outcome nonlinearity` |
| 24 | [The DiD Saga](https://matheusfacure.github.io/python-causality-handbook/24-The-Diff-in-Diff-Saga.html) | `staggered adoption` |
| 25 | [Synthetic DiD](https://matheusfacure.github.io/python-causality-handbook/25-Synthetic-Diff-in-Diff.html) | `synthetic difference-in-differences` |

</details>

<details>
<summary>Code notebooks → vault notes (click to expand)</summary>

| Notebook | Note(s) |
|----------|---------|
| [01-Introduction](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/01-Introduction-To-Causal-Inference.ipynb) | `potential outcomes`, `bias equation` |
| [02-Experiments](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/02-Randomised-Experiments-and-Stats-Review.ipynb) | `randomized experiments` |
| [03-Graphical-Models](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/03-Graphical-Models.ipynb) | `causal DAGs`, `identification` |
| [04-Linear-Regression](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb) | `linear regression for causal inference`, `Frisch-Waugh-Lovell theorem` |
| [05-Propensity-Score](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/05-Propensity-Score.ipynb) | `propensity score`, `IPW`, `propensity score matching`, `positivity` |
| [06-Effect-Heterogeneity](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/06-Effect-Heterogeneity.ipynb) | `CATE`, `evaluating CATE` |
| [07-Meta-Learners](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/07-Meta-Learners.ipynb) | `T-learner`, `X-learner`, `S-learner`, `double machine learning` |
| [08-DiD](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/08-Difference-in-Differences.ipynb) | `Difference-in-Differences (DiD)` |
| [09-Synthetic-Control](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/09-Synthetic-Control.ipynb) | `Synthetic Control`, `synthetic difference-in-differences` |
| [10-Geo-Switchback](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/10-Geo-and-Switchback-Experiments.ipynb) | `geo experiment`, `switchback experiment` |
| [11-IV-and-RDD](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/11-Non-Compliance-and-Instruments.ipynb) | `Instrumental Variables (IV)`, `Regression Discontinuity Design (RDD)` |
| [Simulated-Data](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/Simulated-Data.ipynb) | `simulated data` |

</details>

## Data

All 22 simulated datasets can be loaded directly — see `simulated data.md` for the full index with `pd.read_csv()` URLs and true causal structures.

## References

- Facure, [*Causal Inference for the Brave and True*](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) — primary source (25 chapters + appendices)
- Facure, *Causal Inference in Python* (O'Reilly, 2023) — the published book version
- [Code repo](https://github.com/matheusfacure/causal-inference-in-python-code) — 11 notebooks + data generation
- Angrist & Pischke, *Mostly Harmless Econometrics*
- Cunningham, *Causal Inference: The Mixtape*

---

## Changelog

**2026-06-17** — Initial vault creation
- Created 7 MOC pages following Facure's 5-part structure
- Added 33 concept pages covering all 25 handbook chapters + appendices
- Each page has formulas, Python code from notebooks, and source backlinks
- Added `simulated data.md` with download URLs for all 22 datasets
- Gap analysis against full handbook; filled Ch 6, 7, 10, 14, 17, 20, 23, 24 content
- Enriched IV (unit-conversion framing), RDD (kernel weighting), DML (overfitting risks)
