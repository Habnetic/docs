# Phase 3 — Decision Memo

## 1. Question
Can the fixed-spec model produce stable prioritisation decisions under cross-city transfer?

## 2. Setup
- Reference city: Rotterdam
- Target cities: Hamburg, Donostia
- Model: logistic baseline (fixed priors, fixed features)
- Scaling: Rotterdam-anchored
- Decision metric: top-k membership probability

## 3. Key Results

### 3.1 Top-k probability structure and transfer behaviour

<--- PUT YOUR TEXT HERE --->

## 4. Supporting Evidence

- ECDF plots of top-k probability
- Borderline share vs k
- Summary tables (see outputs)

## 5. Interpretation

- Stability persists under transfer (RTM → HAM)
- DON shows distribution shift, not instability explosion
- Uncertainty remains concentrated near decision threshold

## 6. Limitations

- Synthetic outcome
- Simple feature set (ERA5 + distance to water)
- Potential overconfidence due to model simplicity

The model exhibits very high confidence (probabilities near 0 or 1), which may reflect both structural signal and overconfidence due to model simplicity and synthetic outcome assumptions.

## 7. Operational takeaway

The model can be transferred across cities while preserving a highly polarised decision structure, with uncertainty remaining localised and measurable.

## 8. What to test next

- richer exposure features
- alternative hazard formulations
- real outcome calibration