# Causal Inference Knowledge Vault

An Obsidian knowledge vault for causal inference methods, organized following [Causal Inference in Python](https://www.oreilly.com/library/view/causal-inference-in/9781098140243/) (Facure, O'Reilly) and enriched with content from [Causal Inference for the Brave and True](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) (25-chapter online companion).

## Structure

- **7 MOC (Map of Content) pages** — structural hubs organizing topics by theme
- **33 concept pages** — individual notes with formulas, Python code, and source backlinks
- **1 simulated data page** — all 22 datasets with download URLs and DGP descriptions

## Source mapping

### Part I — Foundations (Handbook Ch 1–4)

| Handbook Chapter | Note(s) |
|-----------------|---------|
| [01 Introduction to Causality](https://matheusfacure.github.io/python-causality-handbook/01-Introduction-To-Causality.html) | `potential outcomes.md`, `bias equation.md` |
| [02 Randomised Experiments](https://matheusfacure.github.io/python-causality-handbook/02-Randomised-Experiments.html) | `randomized experiments.md` |
| [03 Stats Review](https://matheusfacure.github.io/python-causality-handbook/03-Stats-Review-The-Most-Dangerous-Equation.html) | `randomized experiments.md` |
| [04 Graphical Causal Models](https://matheusfacure.github.io/python-causality-handbook/04-Graphical-Causal-Models.html) | `causal DAGs.md`, `identification.md` |

### Part I — Adjusting for Bias (Handbook Ch 5–12)

| Handbook Chapter | Note(s) |
|-----------------|---------|
| [05 Linear Regression](https://matheusfacure.github.io/python-causality-handbook/05-The-Unreasonable-Effectiveness-of-Linear-Regression.html) | `linear regression for causal inference.md`, `Frisch-Waugh-Lovell theorem.md` |
| [06 Grouped and Dummy Regression](https://matheusfacure.github.io/python-causality-handbook/06-Grouped-and-Dummy-Regression.html) | `grouped and dummy regression.md` |
| [07 Beyond Confounders](https://matheusfacure.github.io/python-causality-handbook/07-Beyond-Confounders.html) | `good and bad controls.md` |
| [08 Instrumental Variables](https://matheusfacure.github.io/python-causality-handbook/08-Instrumental-Variables.html) | `Instrumental Variables (IV).md` |
| [09 Non-Compliance and LATE](https://matheusfacure.github.io/python-causality-handbook/09-Non-Compliance-and-LATE.html) | `Instrumental Variables (IV).md` |
| [10 Matching](https://matheusfacure.github.io/python-causality-handbook/10-Matching.html) | `matching.md`, `propensity score matching.md` |
| [11 Propensity Score](https://matheusfacure.github.io/python-causality-handbook/11-Propensity-Score.html) | `propensity score.md`, `Inverse Probability Weighting (IPW).md`, `positivity.md` |
| [12 Doubly Robust Estimation](https://matheusfacure.github.io/python-causality-handbook/12-Doubly-Robust-Estimation.html) | `doubly robust estimation.md` |

### Part I — Panel and Design-Based (Handbook Ch 13–16)

| Handbook Chapter | Note(s) |
|-----------------|---------|
| [13 Difference-in-Differences](https://matheusfacure.github.io/python-causality-handbook/13-Difference-in-Differences.html) | `Difference-in-Differences (DiD).md` |
| [14 Panel Data and Fixed Effects](https://matheusfacure.github.io/python-causality-handbook/14-Panel-Data-and-Fixed-Effects.html) | `fixed effects.md` |
| [15 Synthetic Control](https://matheusfacure.github.io/python-causality-handbook/15-Synthetic-Control.html) | `Synthetic Control.md`, `synthetic difference-in-differences.md` |
| [16 Regression Discontinuity](https://matheusfacure.github.io/python-causality-handbook/16-Regression-Discontinuity-Design.html) | `Regression Discontinuity Design (RDD).md` |

### Part II — Heterogeneity and Personalization (Handbook Ch 17–25)

| Handbook Chapter | Note(s) |
|-----------------|---------|
| [17 Predictive Models 101](https://matheusfacure.github.io/python-causality-handbook/17-Predictive-Models-101.html) | `prediction vs causation.md` |
| [18 HTE and Personalization](https://matheusfacure.github.io/python-causality-handbook/18-Heterogeneous-Treatment-Effects-and-Personalization.html) | `CATE.md` |
| [19 Evaluating Causal Models](https://matheusfacure.github.io/python-causality-handbook/19-Evaluating-Causal-Models.html) | `evaluating CATE.md` |
| [20 Plug-and-Play Estimators](https://matheusfacure.github.io/python-causality-handbook/20-Plug-and-Play-Estimators.html) | `target transformation.md` |
| [21 Meta-Learners](https://matheusfacure.github.io/python-causality-handbook/21-Meta-Learners.html) | `T-learner.md`, `X-learner.md`, `S-learner.md` |
| [22 Debiased/Orthogonal ML](https://matheusfacure.github.io/python-causality-handbook/22-Debiased-Orthogonal-Machine-Learning.html) | `double machine learning.md` |
| [23 Challenges with HTE](https://matheusfacure.github.io/python-causality-handbook/23-Challenges-with-Effect-Heterogeneity-and-Nonlinearity.html) | `binary outcome nonlinearity.md` |
| [24 The DiD Saga](https://matheusfacure.github.io/python-causality-handbook/24-The-Diff-in-Diff-Saga.html) | `staggered adoption.md` |
| [25 Synthetic DiD](https://matheusfacure.github.io/python-causality-handbook/25-Synthetic-Diff-in-Diff.html) | `synthetic difference-in-differences.md` |

### Appendices

| Appendix | Note(s) |
|----------|---------|
| [Debiasing with Orthogonalization](https://matheusfacure.github.io/python-causality-handbook/Debiasing-with-Orthogonalization.html) | `Frisch-Waugh-Lovell theorem.md`, `double machine learning.md` |
| [Debiasing with Propensity Score](https://matheusfacure.github.io/python-causality-handbook/Debiasing-with-Propensity-Score.html) | `propensity score.md`, `Inverse Probability Weighting (IPW).md` |
| [When Prediction Fails](https://matheusfacure.github.io/python-causality-handbook/When-Prediction-Fails.html) | `prediction vs causation.md` |
| [Prediction Metrics for Causal Models](https://matheusfacure.github.io/python-causality-handbook/Prediction-Metrics-For-Causal-Models.html) | `evaluating CATE.md` |
| [Conformal Inference for SC](https://matheusfacure.github.io/python-causality-handbook/Conformal-Inference-for-Synthetic-Control.html) | `Synthetic Control.md` |

### Code Repository

| Notebook | Note(s) |
|----------|---------|
| [01-Introduction-To-Causal-Inference.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/01-Introduction-To-Causal-Inference.ipynb) | `potential outcomes.md`, `bias equation.md` |
| [02-Randomised-Experiments-and-Stats-Review.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/02-Randomised-Experiments-and-Stats-Review.ipynb) | `randomized experiments.md` |
| [03-Graphical-Models.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/03-Graphical-Models.ipynb) | `causal DAGs.md`, `identification.md` |
| [04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/04-The-Unreasonable-Effectiveness-of-Linear-Regression.ipynb) | `linear regression for causal inference.md`, `Frisch-Waugh-Lovell theorem.md` |
| [05-Propensity-Score.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/05-Propensity-Score.ipynb) | `propensity score.md`, `IPW.md`, `propensity score matching.md`, `positivity.md` |
| [06-Effect-Heterogeneity.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/06-Effect-Heterogeneity.ipynb) | `CATE.md`, `evaluating CATE.md` |
| [07-Meta-Learners.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/07-Meta-Learners.ipynb) | `T-learner.md`, `X-learner.md`, `S-learner.md`, `double machine learning.md` |
| [08-Difference-in-Differences.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/08-Difference-in-Differences.ipynb) | `Difference-in-Differences (DiD).md` |
| [09-Synthetic-Control.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/09-Synthetic-Control.ipynb) | `Synthetic Control.md`, `synthetic difference-in-differences.md` |
| [10-Geo-and-Switchback-Experiments.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/10-Geo-and-Switchback-Experiments.ipynb) | `geo experiment.md`, `switchback experiment.md` |
| [11-Non-Compliance-and-Instruments.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/11-Non-Compliance-and-Instruments.ipynb) | `Instrumental Variables (IV).md`, `Regression Discontinuity Design (RDD).md` |
| [Simulated-Data.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/Simulated-Data.ipynb) | `simulated data.md` |

## MOC hierarchy

```
Causal Inference (MOC)
├── Foundations and Frameworks (MOC)
├── Adjusting for Confounding (MOC)
├── Effect Heterogeneity (MOC)
├── Panel Data and DiD (MOC)
├── Design-Based Identification (MOC)
└── Sensitivity and Robustness (MOC)
```

## Usage

Open this directory as an Obsidian vault. All wikilinks (`[[Note Name]]`) will resolve to files within the vault. Notes that are linked but don't yet exist represent planned expansion areas.

## References

- Facure, M. *Causal Inference in Python* (O'Reilly, 2023)
- Facure, M. [*Causal Inference for the Brave and True*](https://matheusfacure.github.io/python-causality-handbook/landing-page.html) (online, 25 chapters)
- [Code repository](https://github.com/matheusfacure/causal-inference-in-python-code) (11 notebooks + data generation)
