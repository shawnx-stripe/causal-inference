---
title: Simulated data
aliases:
  - data generation
  - DGP
  - simulated datasets
  - book datasets
tags:
  - causal-inference
  - data
  - simulation
updated: 2026-06-17
---

# Simulated data

> [!summary]
> Data generation processes (DGPs) for all datasets in "Causal Inference in Python" (Facure). Each dataset embeds a known causal structure — true effects, confounders, and compliance types — enabling validation of causal estimators against ground truth.

> [!tip] Source
> [Simulated-Data.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/Simulated-Data.ipynb)

---

## Dataset index

| Dataset | Treatment | Outcome | True Effect | Design | Used in |
|---------|-----------|---------|-------------|--------|---------|
| `xmas_sales.csv` | `is_on_sale` | `weekly_amount_sold` | +50 (constant) | Confounded observational | [[potential outcomes]], [[bias equation]] |
| `cross_sell_email.csv` | `cross_sell_email` (3-arm) | `conversion` | Randomized | A/B test | [[randomized experiments]] |
| `risk_data.csv` | `credit_limit` | `default` | +0.005 | Confounded | [[linear regression for causal inference]], [[Frisch-Waugh-Lovell theorem]] |
| `management_training.csv` | `intervention` | `engagement_score` | Confounded | Observational PS | [[propensity score]], [[Inverse Probability Weighting (IPW)]], [[propensity score matching]] |
| `interest_rate.csv` | `interest` | `duration` | -0.8 (linear) | Confounded continuous | [[S-learner]], [[double machine learning]] |
| `daily_restaurant_sales.csv` | `discounts` | `sales` | Heterogeneous | Randomized continuous | [[CATE]], [[evaluating CATE]] |
| `email_rnd_data.csv` | `mkt_email` | `next_mnth_pv` | Heterogeneous CATE | Randomized (test set) | [[T-learner]], [[X-learner]] |
| `email_obs_data.csv` | `mkt_email` | `next_mnth_pv` | Same CATE, biased PS | Observational (train) | [[T-learner]], [[X-learner]] |
| `short_offline_mkt_south.csv` | `treated` | `downloads` | Ramping ATT | DiD (south only) | [[Difference-in-Differences (DiD)]] |
| `offline_mkt_staggered.csv` | `treated` | `downloads` | Heterogeneous, staggered | Staggered DiD | [[Difference-in-Differences (DiD)]] |
| `online_mkt.csv` | marketing (3 cities) | `app_download` | 40-60% lift | SC/Geo | [[Synthetic Control]], [[synthetic difference-in-differences]] |
| `sb_exp_every.csv` | `d` | `delivery_time` | +6 (carryover [3,2,1]) | Switchback | [[switchback experiment]] |
| `prime_card.csv` | `prime_card` | `pv` | LATE=700 (compliers) | IV with compliance | [[Instrumental Variables (IV)]] |
| `prime_card_discontinuity.csv` | `prime_card` | `pv` | Local at cutoff | RDD | [[Regression Discontinuity Design (RDD)]] |

---

## Key DGPs

### Confounded observational (xmas_sales)

```python
import numpy as np
import pandas as pd

np.random.seed(1)
units = range(1, 501)

data = pd.DataFrame({
    "store": np.repeat(list(units), 4),
    "unit_fe": np.repeat(np.random.normal(0, 5, 500), 4),
    "weeks_to_xmas": np.tile([3,2,1,0], 500),
    "avg_week_sales": np.repeat(np.random.gamma(20, 1, 500), 4).round(2),
}).assign(
    # Selection: stores with high unit_fe, high sales, or near xmas → more likely on sale
    is_on_sale=lambda d: (
        (d["unit_fe"] > 0) * np.random.binomial(1, 0.2, d.shape[0])
        | (d["avg_week_sales"] > np.random.gamma(25, 1, d.shape[0]))
        | (np.random.normal(d["weeks_to_xmas"], 2) > 2) * np.random.binomial(1, 0.7, d.shape[0])
    ) * 1,
    y0=lambda d: -10 + 10*d["unit_fe"] + d["avg_week_sales"] + 2*d["avg_week_sales"]*d["weeks_to_xmas"],
).assign(
    y1=lambda d: d["y0"] + 50,  # TRUE ATE = 50
    weekly_amount_sold=lambda d: np.random.normal(
        np.where(d["is_on_sale"]==1, d["y1"], d["y0"]), 10
    ).clip(0).round(2)
)
```

### IV with compliance types (prime_card)

```python
np.random.seed(123)
n = 10000

age = 18 + np.random.normal(24, 4, n).round(1)
income = 500 + np.random.gamma(1, age * 100, n).round(0)
u = np.random.uniform(-1, 1, n)

# Compliance determined by unobservable u
from scipy.special import expit
e = expit(u - 0.05*age + 0.01*credit_score)
cust_categ = np.where(e > 0.6, "complier", "never-taker")

# Instrument: random eligibility
prime_eligible = np.random.binomial(1, 0.5, n)
# Only compliers respond to instrument
prime_card = ((cust_categ == "complier") & (prime_eligible == 1)).astype(int)

# Outcome with group-specific effects
# Compliers: effect = 700, Never-takers: effect = 200 (never realized)
group_effect = np.where(cust_categ == "complier", 700, 200)
pv_0 = np.random.normal(200 + 0.4*income + 10*age - 500*u, 200)
pv = pv_0 + group_effect * prime_card
# LATE = 700 (complier effect)
```

### Staggered DiD (offline_mkt_staggered)

```python
np.random.seed(123)
date = pd.date_range("2021-05-01", "2021-07-31", freq="D")
cohorts = pd.to_datetime(["2021-05-15", "2021-06-04", "2021-06-20"]).date
units = range(1, 201)

# Region-based treatment probability
reg_ps = {"S": 0.3, "N": 0.6, "W": 0.7, "E": 0.8}
# Region-specific trends (potential parallel trends violation)
reg_trend = {"S": 0, "N": 0.2, "W": 0.4, "E": 0.6}

# Effect ramps up over time, heterogeneous across units
# tau = min(0.2 * days_since_treatment, 1) * eff_heter * 2
# where eff_heter ~ Exp(1)
```

---

## Related notes

- [[potential outcomes]]
- [[Causal Inference (MOC)]]
- [[randomized experiments]]
- [[Instrumental Variables (IV)]]
