# Phase 3 Transfer Protocol
## Resilient Housing Bayes
**Purpose:** enforce a strict, reproducible, fixed-specification cross-city transfer experiment

---

## 1. Principle

Phase 3 is not a modelling phase. It is a **structural test**.

We do not adapt the model to each city.  
We do not “improve” features mid-transfer.  
We do not tune until things look reasonable.

We apply the same system to different cities and observe how it behaves.

If the behaviour changes, that is the result.

---

## 2. Core rule

**Everything that defines the model and decision process must be fixed before running transfer.**

Only raw data changes.

---

## 3. Fixed elements

The following must remain identical across all cities:

### 3.1 Model specification
- Logistic model:
  ```text
  logit(p_i) = α + β_E * E_i + β_H * H_i
  Y_i ~ Bernoulli(p_i)
  ```
- No interaction terms
- No additional predictors
- No latent variables

### 3.2 Priors
- α ~ Normal(0, 2.5)
- β_E ~ Normal(0, 2.5)
- β_H ~ Normal(0, 2.5)

No city-specific prior tuning allowed.

---

### 3.3 Feature definitions

Exposure:
- distance to water
- water density (same buffer sizes as Rotterdam)

Hazard:
- ERA5-derived precipitation proxy
- same temporal aggregation logic

No additional features per city.

---

### 3.4 Outcome definition
- same synthetic outcome generation rule as Phase 1

No recalibration per city.

---

### 3.5 Scaling rule

**Rotterdam-anchored standardisation (mandatory)**

- Compute mean and standard deviation on Rotterdam
- Save as reference scaler
- Apply unchanged to all target cities

No per-city scaling.

---

### 3.6 Decision metrics

For each k:
- top-k membership probability
- borderline share (0.2 < p < 0.8)
- stable inclusion (p ≥ 0.8)
- stable exclusion (p ≤ 0.2)
- rank standard deviation

Same definitions, same thresholds.

---

### 3.7 k values

Absolute:
- 1000
- 2500
- 5000

Relative:
- 0.5%
- 1%
- 2.5%

---

### 3.8 Inference settings

```python
CHAINS = 2
DRAWS = 500
TUNE = 500
TARGET_ACCEPT = 0.9
```

Do not change per city unless explicitly versioned.

---

## 4. Allowed variation

The following can differ per city:

- number of buildings
- spatial structure
- hydrography distribution
- hazard distribution
- raw feature ranges

---

## 5. Disallowed variation

The following are explicitly forbidden during Phase 3:

- adding new features for a specific city
- removing features for a specific city
- re-scaling per city
- changing priors per city
- changing k thresholds per city
- changing model structure
- adjusting outcome generation per city
- manual filtering of “problematic” assets

If something breaks, document it. Do not fix it silently.

---

## 6. Transfer pipeline

For each city:

### Step 1 — Load raw data
- buildings
- hydrography
- hazard data

### Step 2 — Build features
- exposure features using same logic as Rotterdam
- hazard proxy using same aggregation

### Step 3 — Apply scaling
- load Rotterdam scaler
- apply to city features

### Step 4 — Generate outcome
- apply same synthetic outcome function

### Step 5 — Fit model
- same model
- same priors
- same inference settings

### Step 6 — Compute decision metrics
- top-k probabilities
- borderline share
- rank spread

### Step 7 — Save outputs
- idata.nc
- asset_metrics.parquet
- summary.json

---

## 7. Support diagnostics (mandatory)

For each target city:

Compute:
- min/max comparison vs Rotterdam
- percentile comparison (5%, 50%, 95%)
- share of values outside Rotterdam range

Flag:
- high extrapolation regions

Interpretation:
- results outside support are structural stress, not model failure

---

## 8. Comparison protocol

Each city is compared against Rotterdam:

### Required comparisons
- borderline share vs k
- ECDF of top-k probability
- rank spread distribution
- stable inclusion/exclusion shares

### Interpretation axis
- concentrated vs diffuse instability
- polarised vs flat top-k probabilities
- local vs global rank uncertainty

---

## 9. Naming convention

```text
outputs/phase3/
├── RTM/
├── HAM/
├── DON/
```

Optional experiment labels:
```text
exp_001_transfer_rtm_reference
exp_002_transfer_ham_fixedspec
exp_003_transfer_don_fixedspec
```

---

## 10. Minimal reproducibility requirement

Each city must be runnable with one command:

```bash
python -m rhb.pipelines.run_phase3_transfer --city HAM
```

Outputs must include:
- model trace
- decision metrics
- metadata

---

## 11. Validation checklist

Before accepting results:

- [ ] schema matches Rotterdam
- [ ] scaler applied correctly
- [ ] priors unchanged
- [ ] inference converged (R-hat, ESS)
- [ ] posterior predictive check run
- [ ] decision metrics computed identically
- [ ] outputs saved correctly
- [ ] support diagnostics computed

---

## 12. Interpretation rules

### Structural robustness
- low borderline share
- strong polarisation of top-k probability
- instability near threshold only

### Structural stress
- moderate increase in borderline region
- still localised

### Structural breakdown
- large borderline share
- diffuse instability across population
- weak polarisation

---

## 13. First run protocol (MVP)

Only run:

- Rotterdam (reference)
- Hamburg (first transfer)

Do NOT run Donostia yet.

Purpose:
- fast falsification
- minimal complexity
- clear interpretation

---

## 14. Output expectation

For first transfer:

Produce:
- one comparison table
- one figure set
- one short memo

Answer:

> Does the narrow decision boundary persist under transfer?

---

## 15. Failure policy

If something does not work:

- do not silently fix it
- do not modify model
- log the issue
- continue if possible
- interpret the failure

---

## 16. Final note

This protocol exists to prevent:

- accidental re-fitting
- hidden changes in assumptions
- “soft” transfer that adapts to each city

If the model breaks, that is information.

If the model survives, that is also information.

Anything in between is usually caused by humans trying to be helpful.
