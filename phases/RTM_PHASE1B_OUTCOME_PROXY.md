# RTM Phase 1b — Outcome proxy (synthetic) v0

status: DESIGN LOCKED (Phase 1b v0)
scope: Rotterdam (RTM)
purpose: enable inference + decision stability with real hazard proxy (H_pluvial_v1)

## Inputs (fixed)
- Exposure: E_hat (RTM water exposure v0), standardized to E_std
- Hazard: H_pluvial_v1_mm (ERA5-Land), standardized to H_std

## Outcome (synthetic, v0)
Binary damage flag:

Y_damage_v1b ∈ {0, 1}

Generative model:
logit(p_i) = α + β_E * E_std_i + β_H * H_std_i
Y_i ~ Bernoulli(p_i)

Parameter lock (ground truth):
- baseline damage rate: 5%
- α = log(0.05 / 0.95) ≈ -2.944
- β_E = 1.0
- β_H = 0.6
- seed = 42 (for reproducibility)

## Non-goals
- no empirical calibration
- no real flood probability
- no interaction term in v0 (reserved for Phase 1b sensitivity model)

## Outputs (expected)
Produced in resilient-housing-bayes:
- outputs/rtm/outcomes/Y_damage_v1b.parquet
  - bldg_id
  - Y_damage_v1b (0/1)
  - p_true (optional, debug)
  - outcome_version = "v1b"
  - outcome_src = "synthetic"