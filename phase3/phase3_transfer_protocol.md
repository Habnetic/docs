# Phase 3 — Transfer Protocol
## Resilient Housing Bayes

**Purpose:**  
Define a strictly controlled experimental protocol for cross-city transfer to test whether posterior decision instability remains structurally concentrated.

This document exists to prevent “small reasonable changes” from quietly invalidating the experiment.

---

## 1. Core Principle

Phase 3 is a **falsifiable structural test**, not a modelling exercise.

> If the model changes per city, the experiment is invalid.

---

## 2. Research Question

Does posterior decision instability remain concentrated near a narrow prioritisation boundary under cross-city transfer?

---

## 3. Cities

- Reference: **RTM (Rotterdam)**
- Targets:  
  - **HAM (Hamburg)**  
  - **DON (Donostia)**  

---

## 4. Fixed Elements (Non-Negotiable)

These must remain identical across all cities:

### Model
- Logistic regression
- Same predictors
- Same link function

### Priors

α ~ Normal(0, 2.5)
β_E ~ Normal(0, 2.5)
β_H ~ Normal(0, 2.5)


### Predictors
- `E_hat_v0`
- `H_pluvial_v1_logrel`

### Feature definitions
- Exposure: water proximity + density (unchanged)
- Hazard: ERA5-derived proxy (same logic)

### Scaling
- **Rotterdam-anchored**
- No per-city re-scaling

### Outcome
- Synthetic `Y_damage`
- Same generation coefficients across cities

### Decision metrics
- Top-k membership probability
- Borderline definition: `0.2 < p < 0.8`
- Rank variability

### Inference
- NUTS sampler
- 4 chains
- 500 tune / 500 draws (unless versioned)

---

## 5. Allowed to Change

These reflect real domain shift:

- Number of buildings
- Spatial structure
- Hydrography
- Raw feature distributions
- Hazard distributions
- Resulting posterior behaviour

---

## 6. Scaling Protocol

### Method: Rotterdam-anchored standardisation

1. Fit scaler on RTM:
   - mean
   - std

2. Save:

outputs/phase3/config/rtm_feature_scaler.json


3. Apply unchanged to:
- HAM
- DON

### Explicitly forbidden
- Per-city standardisation
- Pooled scaling
- Silent re-fitting

---

## 7. Hazard Transformation

To avoid cross-city scale artifacts:


H_rel = H / median(H_RTM)
H_logrel = log(H_rel)


Used in model:


β_H * H_pluvial_v1_logrel


---

## 8. Synthetic Outcome

### Generative form


logit(p_i) = α + β_E * E_hat_v0_i + β_H * H_pluvial_v1_logrel_i
Y_i ~ Bernoulli(p_i)


### Properties

- Same coefficients across cities
- No re-fitting per city
- Controlled signal strength

---

## 9. Outputs per City

Each run must produce:


outputs/phase3/<CITY>/
├── phase3_features_scaled.parquet
├── idata.nc
├── summary.json
├── asset_metrics.parquet
├── decision_metrics.json
├── support_diagnostics.json
├── run_metadata.json


---

## 10. Decision Metrics

For each `k`:

- top-k membership probability
- borderline share
- stable inclusion share
- stable exclusion share
- rank_std distribution

Default k values:

[1000, 2500, 5000]

relative thresholds (optional)

---

## 11. Support Diagnostics

Compare target city vs RTM:

- min / max
- percentiles
- share outside support

Purpose:

> Distinguish transfer from extrapolation.

---

## 12. Interpretation Rules

### Structural robustness

- Low borderline share
- Strong probability polarisation
- Instability near threshold only

### Moderate stress

- Slight widening
- Still localised

### Structural breakdown

- High borderline share
- Diffuse probabilities
- Instability across population

---

## 13. Failure Modes (Common Ways to Ruin This)

- Re-scaling per city
- Changing priors “slightly”
- Adjusting synthetic outcome
- Adding features mid-run
- Changing k values between cities
- Debugging by modifying the model

---

## 14. Execution Command


python -m rhb.phase3_transfer --city <CITY>
python -m rhb.pipelines.run_phase3_model --city <CITY>
python -m rhb.pipelines.compute_phase3_decision_metrics --city <CITY>


---

## 15. Minimal Valid Run

At least one complete transfer:

- RTM → HAM
- Fixed spec respected
- Decision metrics computed
- Results interpretable

---

## 16. Outcome Criterion

Phase 3 succeeds if:

- transfer executed reproducibly
- decision structure compared across cities
- result clearly supports or weakens structural hypothesis

