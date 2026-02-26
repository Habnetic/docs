# RTM Phase 1b — Synthetic Outcome + Decision Stability (Logistic)

Status: COMPLETE (Phase 1b)
Date: 2026-02-26
Scope: Synthetic damage outcome + Bayesian inference + posterior decision stability metrics.
Non-claim: This is not empirical flood risk. It is a scaffold to validate decision-stability mechanics.

## Goal

After Phase 1a produced a deterministic ERA5-Land hazard intensity signal (H_pluvial_v1),
Phase 1b closes the loop by introducing a **minimal synthetic outcome** and running Bayesian inference
to obtain **decision distributions** (not just expected loss proxies).

This implements the “hourglass bottom”:
posterior → decision quantity → stability diagnostics.

## Inputs (frozen for Phase 1b)

- Exposure proxy (Phase 0):
  - `E_hat` (deterministic exposure index)

- Hazard intensity (Phase 1a):
  - `H_pluvial_v1_mm` (deterministic mean annual max 1-hour precipitation, 1991–2020)

## Synthetic outcome (v1b)

Outcome definition:
- Binary damage flag `Y_damage_v1b ∈ {0,1}`
- Baseline damage rate: **5%**
- Generated via a logistic model of standardized exposure and hazard:
  - `logit(p_i) = α + β_E * E_std_i + β_H * H_std_i`
  - `Y_i ~ Bernoulli(p_i)`

Purpose:
- Provide a minimal, controlled “observed” variable to validate inference + decision stability
  without claiming real-world impacts.

## Inference model (v1b)

Model:
- Bernoulli-logistic regression with priors on α, β_E, β_H.
- Posterior predictive checks performed.
- Inference run on a subsample (e.g. N=5000) for tractability.

Posterior sanity:
- Convergence diagnostics acceptable (r_hat ~ 1.00–1.01).
- PPC indicates the model reproduces the synthetic outcome structure.

## Decision quantities

Primary decision object:
- **top-k membership probability**
  - `topk_prob_i(k) = P(i ∈ Top-k | posterior draws)`

Derived stability diagnostics across k:
- Mean top-k membership:
  - `mean(topk_prob) = k / N` (sanity identity; must hold)
- Borderline set share:
  - share of buildings with `0.2 < topk_prob < 0.8`
- Mean membership entropy (Bernoulli entropy of topk_prob)

Observed structure (expected, correct):
- Instability concentrates around the ranking threshold band.
- Borderline/entropy peak at intermediate k and collapse as k → N (trivial decision).

## Outputs (resilient-housing-bayes)

Artifacts produced in:
`resilient-housing-bayes/outputs/rtm/`

- Outcome:
  - `outputs/rtm/outcomes/Y_damage_v1b.parquet`

- Inference:
  - `outputs/rtm/inference/rtm_damage_v1b_logistic_idata.nc`

- Decision stability tables (per k, on the inference subset):
  - `outputs/rtm/decision_stability/v1b/rtm_decision_stability_v1b_top{k}.parquet`

- Figures:
  - `outputs/rtm/decision_stability/v1b/figures/*.png`

Notes:
- k must satisfy `k <= N` where N is the inference sample size.
- For city-wide mapping, a later step must compute topk_prob over the full 221,324 buildings
  (or use an approximation strategy).

## Interpretation constraints (non-claims)

- No flood probability.
- No return periods.
- No empirical damage calibration.
- This phase validates the decision-stability mechanism only.

## Next step (Phase 1c / Phase 2 boundary)

Choose one:

1) Deterministic vs posterior ranking comparison:
   - overlap of Top-k sets under `p_mean` vs posterior membership distribution

2) Sensitivity stress test:
   - rerun with baseline damage rate {2%, 10%} and compare stability curves

3) Phase 2 entry:
   - introduce probabilistic hazard (GEV / return levels) and propagate hazard uncertainty