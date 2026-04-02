---
title: "Phase 2 — Hazard Perturbation Experiment"
author: "Habnetic — Resilient Housing Bayes"
date: "2026"
geometry: margin=2.5cm
fontsize: 11pt
mainfont: Times New Roman
---

# Phase 2 — Hazard Perturbation Experiment

## Objective

The objective of Phase 2 is to test whether the decision-stability result obtained in Phase 1 persists under controlled perturbations of the hazard proxy.

Specifically:

> Does the narrow decision boundary remain stable when hazard is uncertain?

---

## Experimental Setup

- **Dataset**: Rotterdam building-level dataset (~221,000 assets), subsampled to \(N = 5000\)
- **Exposure**: \(E_{\text{hat\_v0}}\) (standardized)
- **Hazard**: \(H_{\text{pluvial\_v1\_mm}}\) (standardized to \(H_{\text{std}}\))
- **Outcome**: synthetic binary variable \(Y_{\text{damage\_v1b}}\), target baseline rate ≈ 5%
- **Model**: Bayesian logistic regression (PyMC)

### Generative Structure

The outcome is generated according to:

\[
\text{logit}(p_i) = \alpha + \beta_E \cdot E_{\text{std}, i} + \beta_H \cdot H_{\text{std}, i}
\]

\[
Y_i \sim \text{Bernoulli}(p_i)
\]

This provides a minimal controlled setting to evaluate inference and decision stability.

---

## Hazard Perturbation

The standardized hazard proxy is perturbed as:

\[
H_{\text{std}}^{\text{perturbed}} = H_{\text{std}} + \varepsilon
\]

\[
\varepsilon \sim \mathcal{N}(0, \sigma)
\]

Perturbation is applied **after standardization**, so that \(\sigma\) is directly interpretable in standard deviation units.

### Tested Noise Levels

\[
\sigma \in \{0.00, 0.10, 0.20, 0.30\}
\]

---

## Metrics

The primary decision metric is:

### Top-k Membership Probability

For each asset \(i\):

\[
P(i \in \text{Top-}k \mid \text{posterior draws})
\]

with \(k = 1000\).

### Borderline Share

Assets with uncertain prioritization are defined as:

\[
0.2 < P(i \in \text{Top-}k) < 0.8
\]

The **borderline share** is the proportion of such assets.

---

## Results

| \(\sigma\) | Borderline Share |
|-----------|------------------|
| 0.00      | 0.0158           |
| 0.10      | 0.0148           |
| 0.20      | 0.0172           |
| 0.30      | 0.0174           |

This corresponds to approximately **75–85 buildings** in the 5000-asset inference sample.

---

## Key Observations

### 1. Stability Under Perturbation

- Borderline share remains within a narrow range (~1.5–1.7%)
- No evidence that instability propagates beyond a narrow decision boundary

### 2. Saturation Behavior

- Increase from \(\sigma = 0.10 \rightarrow 0.20\) is modest
- Increase from \(\sigma = 0.20 \rightarrow 0.30\) is negligible

This suggests:

> The system reaches a stable uncertainty boundary early.

### 3. Structural Interpretation

- Approximately 98% of assets exhibit stable prioritization
- Uncertainty is confined to a small, persistent boundary region
- This boundary is driven by model structure rather than noise realization

---

## Conclusion

> Decision instability remains concentrated under moderate hazard perturbations.

The prioritization structure is robust to Gaussian noise in the hazard proxy (\(\sigma \leq 0.30\)), indicating that results are not artifacts of precise hazard inputs.

---

## Robustness Across Runs

Results are stable across runs and computational environments, with only minor numerical variation that does not affect the qualitative structure of the decision boundary.

---

## Limitations

- Synthetic outcome (no observed damage data)
- Single-city evaluation (Rotterdam)
- Deterministic hazard proxy (no latent hazard model)

---

## Next Steps

- Outcome sensitivity analysis (e.g. \(p = 0.02, 0.10\))
- Cross-city validation (Phase 3)
- Latent hazard modeling (Phase 4)

---

## Reproducibility

**Notebook**
notebooks/15_phase2_hazard_noise.ipynb

**Key parameters**

- Random seed: 42  
- Sample size: \(N = 5000\)  
- Top-k: \(k = 1000\)  
- PyMC sampling: 2 chains, 500 draws  

---

## Interpretation (Short Version)

> The decision boundary is narrow, stable, and robust to hazard uncertainty.