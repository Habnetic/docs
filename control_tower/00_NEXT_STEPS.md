# Habnetic — Next Steps (Control Tower)

last_updated: 2026-03-03  
owner: C. Price  
scope: RTM (Rotterdam) pipeline — Phase 0 + Phase 1 completed

This file lives in the parent `Habnetic/` folder on purpose.  
It is the **one place to read before touching any repo**.

---

# Where we are right now (RTM)

## Phase 0 — Deterministic Exposure Backbone (Frozen) ✅

- RTM boundary, buildings, hydrography normalized, clipped, validated  
- CRS harmonized (EPSG:28992), geometry hygiene enforced  
- 221,324 buildings processed  
- Stable `bldg_id` propagated end-to-end  

Water exposure priors computed:

- Distance to nearest water  
- Water length density at 250 m / 500 m / 1000 m  

Deterministic latent exposure proxy:

- `E_hat` computed
- Exported with metadata
- Spatial sanity verified in QGIS

Hazard scaffolding:

- Deterministic pluvial proxy `H_pluvial_v0`
- Structural placeholder only
- No physical calibration implied

Environment:

- venv stable
- PyTensor compiler working
- Notebooks reproducible

Phase 0 status: COMPLETE

---

## Phase 1 — Bayesian Baseline + Decision Stability (Closed) ✅

Phase 1 introduces a minimal generative structure and validates the full:

Exposure → Hazard → Outcome → Posterior → Decision

### 1. Outcome proxy (synthetic)

Implemented:

- Logistic synthetic damage proxy (`Y_damage_v1b`)
- Baseline probability scenarios: p02, p05, p10
- Beta sensitivity scenarios (v1c):
  - bE02_bH06
  - bE10_bH02
  - bE10_bH06

Explicitly synthetic.
No empirical calibration claims.

---

### 2. Bayesian inference (validated)

Model:

logit(p_i) = α + β_E E_i + β_H H_i

Priors:

α, β_E, β_H ~ Normal(0, 2.5)

- MCMC (NUTS)
- Convergence diagnostics verified (r_hat ≈ 1)
- ESS adequate
- Subsample inference (N=5000) for structural validation

---

### 3. Posterior decision extraction

Computed from posterior draws:

- posterior mean probability per building
- posterior standard deviation
- top-k membership probability
- ranking overlap metrics
- entropy
- borderline share (0.2 < p < 0.8)

Sensitivity validated across:

- baseline probability
- beta structure

Result:

- High ranking stability under aligned generative structure
- Borderline region small for k ≤ 5000

---

### 4. Citywide posterior scoring (N=221,324)

Posterior draws applied to full city.

Produced per scenario:

- `p_mean_city`
- `p_sd_city`
- `topk_prob_city_1000`
- `topk_prob_city_2500`
- `topk_prob_city_5000`

Sanity condition verified:

mean(topk_prob_city_k) ≈ k / N_city

Citywide instability share:

< 0.3% for k ≤ 5000

Phase 1 status: COMPLETE

---

# What RTM currently is

RTM now provides:

- Deterministic exposure backbone (Phase 0)
- Bayesian structural baseline (Phase 1)
- Posterior → decision mapping
- City-scale ranking stability metrics
- Reproducible full pipeline

It does NOT yet include:

- Probabilistic hazard uncertainty
- Real calibrated damage data
- Multi-hazard composition
- Cross-city transfer validation

---

# Next milestone — Phase 2 (Not started)

Introduce structural uncertainty.

Planned directions:

- Probabilistic hazard layer
- Hazard uncertainty propagation
- Specification sensitivity
- Cross-city stress testing
- Posterior rank variance inflation analysis

Phase 2 must preserve:

- Generative coherence
- Explicit assumptions
- Clear separation of deterministic vs probabilistic components

---

# What NOT to do

- Do not claim real flood probability.
- Do not imply empirical validation.
- Do not add new hazards prematurely.
- Do not blur Phase 1 baseline with Phase 2 ambitions.

---

# Canonical docs to keep aligned

- `docs/references/exposure/rtm_water_exposure_v0.md`
- `00_RTM_PIPELINE_STATUS.md`
- inference notebooks (07, 12, 13, 14)
- city scoring outputs under `outputs/rtm/city_scoring/`

This file defines the operational state of Habnetic RTM.

Phase 0: exposure backbone frozen.  
Phase 1: Bayesian baseline closed.  
Phase 2: structural uncertainty (future).
