# Phase 2 — Hazard Perturbation Experiment

## Objective

Test whether the decision-stability result from Phase 1 persists under controlled perturbations of the hazard proxy.

Specifically:
> Does the narrow decision boundary remain stable when hazard is uncertain?

---

## Experimental Setup

- Dataset: Rotterdam building-level dataset (~221k assets, subsampled to 5000)
- Exposure: `E_hat_v0` (standardized)
- Hazard: `H_pluvial_v1_mm` (standardized → `H_std`)
- Outcome: synthetic (`Y_damage_v1b`, p=0.05 scenario)
- Model: Bayesian logistic regression (PyMC)

### Perturbation

Hazard was perturbed as:

H_std_perturbed = H_std + ε
ε ~ Normal(0, σ)


Tested values:

- σ = 0.00 (baseline)
- σ = 0.10
- σ = 0.20
- σ = 0.30

---

## Metrics

Primary metric:

- **Top-k membership probability** (k = 1000)
- **Borderline share**:
0.2 < P(top-k membership) < 0.8


This represents assets whose prioritization is uncertain.

---

## Results

| Sigma | Borderline Share |
|------|------------------|
| 0.00 | 0.0158 |
| 0.10 | 0.0148 |
| 0.20 | 0.0172 |
| 0.30 | 0.0174 |

---

## Key Observations

### 1. Stability under perturbation

- Borderline share remains within a narrow range (~1.5–1.7%)
- No evidence of instability propagation

### 2. Saturation behavior

- Increase from σ=0.10 → 0.20 is modest
- Increase from σ=0.20 → 0.30 is negligible

This suggests:
> the system reaches a stable uncertainty boundary early

### 3. Structural interpretation

- ~98% of assets have stable prioritization
- Uncertainty is confined to a small, persistent boundary
- This boundary is not driven by noise but by model/data structure

---

## Conclusion

> Decision instability remains concentrated under moderate hazard perturbations.

The prioritization structure is robust to Gaussian noise in the hazard proxy (σ up to 0.30), indicating that results are not artifacts of precise hazard inputs.

---

## Limitations

- Synthetic outcome (no observed damage data)
- Single-city evaluation (Rotterdam)
- Deterministic hazard proxy (no latent hazard model)

---

## Next Steps

- Outcome sensitivity (p02, p10 scenarios)
- Cross-city validation (Phase 3)
- Latent hazard modeling (Phase 4)

---

## Reproducibility

Notebook:
notebooks/15_phase2_hazard_noise.ipynb


Key parameters:
- seed = 42
- N = 5000
- k = 1000
- PyMC sampling: 2 chains, 500 draws

---

## Interpretation (short version)

> The decision boundary is narrow, stable, and robust to hazard uncertainty.