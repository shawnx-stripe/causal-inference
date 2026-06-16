---
title: Switchback experiment
aliases:
  - switchback design
  - temporal experiment
  - time-series experiment
tags:
  - causal-inference
  - experimentation
  - interference
updated: 2026-06-17
---

# Switchback experiment

> [!summary]
> Switchback experiments rotate treatment on/off over time periods within the same unit, enabling causal estimation when spatial randomization is impractical. The key challenge is **carryover effects** — treatment in period $t$ may affect outcomes in $t+1, t+2, \ldots$ — which biases naive comparisons.

> [!tip] Sources
> - [10-Geo-and-Switchback-Experiments.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/10-Geo-and-Switchback-Experiments.ipynb) — "Switchback Experiment" section
> - Data: `sb_exp_every.csv` (random switching every period), `sb_exp_opt.csv` (optimized blocks of length m=2)

## Potential outcomes with carryover

With carryover of order $p$, outcome depends on the treatment *sequence*:
$$
Y_t = f(D_t, D_{t-1}, \ldots, D_{t-p}) + \varepsilon_t
$$

In the book's DGP: effect of NOT being treated carries over as [3, 2, 1] for 3 periods. Total treatment effect = 6.

## Design choices

| Design | Switching | Carryover handling |
|--------|-----------|-------------------|
| Random every period | $D_t \sim \text{Bernoulli}(0.5)$ each $t$ | Carryover contaminates estimates |
| Optimal block design | Switch only at intervals of $m$ periods | Longer blocks → less carryover contamination |

## Estimating carryover order

Test different carryover orders $p$ by including $p$ lags in the model and checking when additional lags become insignificant.

## Code

```python
import numpy as np
import pandas as pd

# Generate potential outcomes with carryover
def y_given_d(d, effect_params=[3, 2, 1], T=120, seed=None):
    np.random.seed(seed)
    x = np.arange(1, T+1)
    return (np.log(x+1)
            + 2*np.sin(x * 2*np.pi / 24)  # seasonality
            + np.convolve(~d.astype(bool), effect_params)[:-(len(effect_params)-1)]
            + np.random.normal(0, 1, T)
    ).round(2)

# Optimal design: switch only every m periods
def gen_d(rand_points, p=0.5):
    """Treatment assignment that only changes at randomization points."""
    result = [np.random.binomial(1, p)]
    for t in rand_points[1:]:
        result.append(np.random.binomial(1, p)*t + (1-t)*result[-1])
    return np.array(result)

m = 2  # block length
T = 120
rand_points = np.isin(np.arange(1, T+1), [1] + [i*m+1 for i in range(2, T//m - 1)])
d_opt = gen_d(rand_points)
y_opt = y_given_d(d_opt, T=T, seed=1)
```

## Related notes

- [[geo experiment]]
- [[Synthetic Control]]
- [[Design-Based Identification (MOC)]]
