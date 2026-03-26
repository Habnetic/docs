# RTM — Phase 2: Hazard Perturbation

## Objective

Assess whether decision stability persists under uncertainty in the hazard proxy.

---

## Approach

- Introduce Gaussian perturbations to standardized hazard:
  H_std + ε, ε ~ Normal(0, σ)


- Evaluate impact on decision metrics:
- Top-k membership probability
- Borderline share

Tested noise levels:
- σ = 0.10
- σ = 0.20
- σ = 0.30

---

## Key Result

> Decision instability remains confined to a narrow boundary (~1.5–1.7% of assets) under moderate hazard perturbations.

The ranking structure is robust to input noise.

---

## Interpretation

- Stability is not an artifact of precise hazard values
- Uncertainty is localized, not systemic
- The decision boundary appears to be a structural property of the model

---

## Evidence

- See experiment:
- `docs/experiments/phase2_hazard_noise.md`

---

## Implication for Next Phases

- Supports moving to:
- Outcome sensitivity (multiple scenarios)
- Cross-city validation (Phase 3)

- Latent hazard modeling (Phase 4) becomes a refinement, not a correction

---

## Status

✅ Phase 2 validated (hazard robustness)