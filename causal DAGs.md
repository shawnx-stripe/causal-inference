---
title: Causal DAGs
aliases:
  - directed acyclic graphs
  - DAGs
  - graphical causal models
  - causal graphs
tags:
  - causal-inference
  - dags
  - identification
updated: 2026-06-17
---

# Causal DAGs

> [!summary]
> Directed acyclic graphs (DAGs) represent causal structure: nodes are variables, directed edges are causal relationships. DAGs formalize which variables to condition on (and which to avoid) for valid causal identification. The three building blocks — chains, forks, and colliders — determine how association flows.

> [!tip] Sources
> - [03-Graphical-Models.ipynb](https://github.com/matheusfacure/causal-inference-in-python-code/blob/main/causal-inference-in-python/03-Graphical-Models.ipynb)
> - [Ch 4: Graphical Causal Models](https://matheusfacure.github.io/python-causality-handbook/04-Graphical-Causal-Models.html)

## Three building blocks

### Chain (mediation): $T \to M \to Y$
- Association flows from $T$ to $Y$ through $M$
- **Conditioning on $M$ blocks the path** (removes the causal channel)

### Fork (confounding): $T \leftarrow X \rightarrow Y$
- Common cause $X$ creates spurious association between $T$ and $Y$
- **Conditioning on $X$ blocks the spurious path** (removes confounding)

### Collider: $T \to X \leftarrow Y$
- $T$ and $Y$ both cause $X$
- Path is **blocked by default**
- **Conditioning on $X$ opens the path** (introduces collider bias)

> [!warning]
> Conditioning on a collider (or its descendant) creates spurious association. This is [[collider bias]].

## Surrogate confounders

A **surrogate confounder** is a cause or effect of an unmeasured confounder $U$. Conditioning on a surrogate partially closes the backdoor path through $U$ — it reduces bias without eliminating it. Examples: SAT scores as a proxy for unobserved ability, parental education as a proxy for socioeconomic background.

## Descendant-of-collider conditioning

> [!warning]
> Conditioning on a **descendant** of a collider also opens the spurious path — it acts as a partial proxy for the collider itself. This is the more subtle, often-missed case of collider bias.

Example: $T \to C \leftarrow Y$, $C \to D$. Conditioning on $D$ (descendant of collider $C$) partially opens the $T - C - Y$ path.

## d-separation

Two sets of nodes $A$ and $B$ are d-separated given conditioning set $Z$ if every path between them is blocked. Blocked means:
- A chain or fork has a node in $Z$ on the path, OR
- A collider on the path has no descendant in $Z$

## Identification via DAGs

- **[[back-door criterion]]**: A set $Z$ satisfies the back-door criterion relative to $(T, Y)$ if (1) no node in $Z$ is a descendant of $T$, and (2) $Z$ blocks every path between $T$ and $Y$ that has an arrow into $T$.
- **Adjustment formula**: $\mathbb{E}[Y \mid do(T=t)] = \sum_x \mathbb{E}[Y \mid T=t, X=x] \cdot P(X=x)$

## Code

```python
import graphviz as gr
import networkx as nx

# Draw a DAG with graphviz
g = gr.Digraph(graph_attr={"rankdir": "LR"})
g.edge("X", "T")
g.edge("X", "Y")
g.edge("T", "Y")
g  # renders the confounding DAG

# Test d-separation with networkx
model = nx.DiGraph([("X", "T"), ("X", "Y"), ("T", "Y")])
# Is T independent of Y given X?
nx.d_separated(model, {"T"}, {"Y"}, {"X"})  # True (backdoor blocked)
# Is T independent of Y unconditionally?
nx.d_separated(model, {"T"}, {"Y"}, set())  # False (confounding path open)
```

## Related notes

- [[identification]]
- [[collider bias]]
- [[back-door criterion]]
- [[potential outcomes]]
- [[Foundations and Frameworks (MOC)]]
