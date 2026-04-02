---
title: "RTM — Phase 2: Hazard Perturbation"
author: "Habnetic — Resilient Housing Bayes"
date: "2026"
geometry: margin=2.5cm
fontsize: 11pt
mainfont: Times New Roman
---

# RTM — Phase 2: Hazard Perturbation

## Objective

Assess whether decision stability persists under uncertainty in the hazard proxy.

Specifically:

> Does the narrow decision boundary observed in Phase 1 remain stable when hazard inputs are perturbed?

---

## Approach

### Hazard perturbation

Gaussian noise is introduced to the standardized hazard proxy:

\[
H_{\text{std}}^{\text{perturbed}} = H_{\text{std}} + \varepsilon, \quad \varepsilon \sim \mathcal{N}(0, \sigma)
\]

Perturbation is applied **after standardization**, so that \(\sigma\) is directly interpretable in standard deviation units.

### Evaluation metrics

Decision stability is evaluated using:

- **Top-k membership probability** (\(k = 1000\))
- **Borderline share**:

\[
0.2 < P(i \in \text{Top-}k) < 0.8
\]

Tested noise levels:

- \(\sigma = 0.00\) (baseline)
- \(\sigma = 0.10\)
- \(\sigma = 0.20\)
- \(\sigma = 0.30\)

---

## Key Result

> Decision instability remains confined to a narrow boundary (~1.5–1.7% of assets) under moderate hazard perturbations.

The prioritization structure is robust to input noise in the hazard proxy.

---

## Interpretation

- Stability is not an artifact of precise hazard values  
- Uncertainty is localized rather than systemic  
- The decision boundary appears to be a structural property of the model and data  

---

## Evidence

Detailed experiment:
docs/experiments/phase2_hazard_noise.md


Results are consistent across runs and computational environments, with only minor numerical variation that does not affect the qualitative structure of the decision boundary.

---

## Implications for Next Phases

- Supports transition to:
  - Outcome sensitivity (multiple synthetic scenarios)
  - Cross-city validation (Phase 3)

- Positions latent hazard modeling (Phase 4) as a refinement rather than a correction

---

## Status

**Phase 2 complete — hazard robustness validated**

---

## Interpretation (Short Version)

> The decision boundary is narrow, stable, and robust to hazard uncertainty.