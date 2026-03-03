# RTM Pipeline — Status + Finish Scheme

last_updated: 2026-02-05  
scope: Rotterdam (RTM)

This is the **map of the territory**.  
If you’re confused, read this before opening QGIS or writing more code.

---

## 1) RTM pipeline overview (end-to-end)

### A. Normalize sources (reproducible)
- boundary (CBS → EPSG:28992)
- buildings (OSM → EPSG:28992, schema filtered)
- hydrography (TOP50NL `waterdeel_lijn` → EPSG:28992)

### B. Derive study-area layers (reproducible)
- clip buildings to RTM boundary
- clip hydrography to RTM boundary

### C. Compute priors (deterministic)
- water exposure metrics per building:
  - distance to nearest water
  - water length density within 250 / 500 / 1000 m buffers

### D. Validate priors
- statistical sanity (quantiles, ranges, distributions)
- spatial sanity (QGIS join + graduated styling, manual plausibility check)


### E. Define latent exposure proxy (v0)
- deterministic index `E_hat` from transformed + standardized priors
- export as key-value table by stable building ID (`bldg_id`)

### F. Exposure observatory (v0)
- export run artifacts (state snapshot, logs, figures)
- generate static HTML report for human inspection
- visualize deterministic exposure (`E_hat`) with full provenance

### F2. Define hazard interface (v0)
- introduce first hazard variable (`H_pluvial_v0`)
- deterministic placeholder forcing
- no physical calibration or flood modeling
- establishes Exposure → Hazard interface

### G. Couple to hazard + impact (v1+)
- introduce hazard intensity (rainfall, flood depth, etc.)
- introduce outcome proxy (damage, downtime, cost, displacement)
- Bayesian inference becomes meaningful only here (likelihood exists)

---

## 2) Where we stand (today)

### Completed ✅

**Data repo**
- `processed/RTM/normalized/*` created
- `processed/RTM/derived/buildings_rtm.gpkg`  
  → **221,324 buildings**
- `processed/RTM/derived/hydrography_rtm.gpkg`
- `processed/RTM/priors/building_water_proximity.parquet`  
  → deterministic priors for all buildings  
  → stable `bldg_id` enforced end-to-end

**Resilient-Housing-Bayes**
- notebook 01: EDA completed  
  - distributions validated  
  - correlations understood and documented
- notebook 03 (Part A): **deterministic `E_hat` completed**
  - transforms applied
  - z-score standardization verified
  - `E_hat` exported
  - stats + metadata written
- notebook 03 (Part B): **explicitly disabled**
  - confirmed: no outcome ⇒ no learning
- notebook 03: output paths anchored to repo root (outputs/rtm/)
- notebook 04: hazard interface established
  - deterministic pluvial hazard placeholder (`H_pluvial_v0`)
  - constant forcing, no calibration
  - joined by `bldg_id`

**Phase 1 — Bayesian baseline**
- synthetic outcome proxy implemented
- logistic Bayesian model validated
- sensitivity experiments (baseline + beta)
- posterior decision metrics computed
- citywide posterior scoring completed (N=221,324)



**Model outputs**
- `outputs/rtm/water_exposure_Ehat_v0.parquet`
- `outputs/rtm/water_exposure_Ehat_v0_stats.json`
- `outputs/rtm/hazard_pluvial_v0.parquet`


**QGIS spatial sanity check completed**
  - E_hat joined to buildings_rtm.gpkg via bldg_id
  - graduated quantile styling
  - expected harbor / canal / inland gradients confirmed

**Environment**
- venv + ipykernel fixed
- notebooks fully runnable and reproducible

---

### In progress ⚠️
- Exposure observatory v0
  - artifact-driven
  - read-only
  - static HTML report per run

---

### Not started ❌
- Hazard intensity calibration (real data, ERA5, etc.)
- Outcome / impact definition
- Full Bayesian model with likelihood
- Multi-hazard composition


(All intentionally deferred until v0 is frozen.)

---

## 3) Definition of Done — RTM v0

RTM v0 is done when **all** are true:

1) `building_water_proximity.parquet` exists for all buildings ✅  
2) Deterministic `E_hat` exists and is exported ✅  
3) QGIS join + styling confirms expected spatial patterns (manual check)  
4) Docs specify:
   - features
   - transforms
   - `E_hat` formula
   - interpretation constraints
5) Resilient-Housing-Bayes contains:
   - updated notebook 03
   - outputs written under `outputs/rtm/`
6) One static exposure observatory report exists for an RTM run

At present: items 1–5 complete, item 6 pending.
RTM Phase 0 exposure proxy is frozen.


---

## 4) Final v0 file layout (locked)

### data/
- `processed/RTM/priors/building_water_proximity.parquet` ✅

### resilient-housing-bayes/
- `outputs/rtm/water_exposure_Ehat_v0.parquet` ✅
- `outputs/rtm/water_exposure_Ehat_v0_stats.json` ✅

### docs/
- `docs/references/exposure/rtm_water_exposure_v0.md` (update with E_hat export)

### runs/ (v0 observatory)
- `runs/<run_id>/state_snapshot.json`
- `runs/<run_id>/report.html`
- `runs/<run_id>/figures/`

---

## 5) Next milestone after v0 (v1)

Pick **one** hazard track only:

### Option 1 — Pluvial proxy
- rainfall extremes (ERA5 hourly precipitation)
- imperviousness / drainage proxy (OSM or Copernicus)

### Option 2 — Fluvial / coastal proxy
- flood maps or water levels (if available)
- elevation / distance-to-defense proxies

Then add **one** simple outcome:
- synthetic damage class
- or downtime proxy

Only then does Bayesian inference stop being decorative and start being useful.

---


## 6) Phase 1 — Bayesian Baseline + Decision Stability (v1b / v1c)

Phase 1 introduces a minimal generative structure and validates the full:

Exposure → Hazard → Outcome → Posterior → Decision pipeline.

### A. Outcome proxy (synthetic, explicit)

- Logistic damage proxy Y_damage_v1b
- Baseline probabilities explored: p02, p05, p10
- Beta sensitivity scenarios explored (v1c):

    - bE02_bH06
    - bE10_bH02
    - bE10_bH06

- No empirical calibration (explicitly synthetic)
- Purpose: structural validation of inference + decision extraction

### B. Bayesian inference (subsampled N=5000)

- Logistic model:
- logit(p_i) = α + β_E E_i + β_H H_i
- Priors:
  α, β_E, β_H ~ Normal(0, 2.5)
- MCMC (NUTS)
- Convergence diagnostics verified (r_hat ≈ 1, ESS adequate)

Inference intentionally performed on subsample (N=5000)
to validate model mechanics efficiently.

### C. Posterior decision objects

From posterior draws:

- posterior mean probability per building
- posterior standard deviation
- top-k membership probability
- ranking overlap metrics
- entropy and borderline share
- Decision stability quantified across:
- baseline probability sensitivity
- beta sensitivity

### D. Citywide posterior scoring (N=221,324)

Posterior draws from subsample inference applied to full city:
- p_mean_city
- p_sd_city
- topk_prob_city_k for k ∈ {1000, 2500, 5000}

Sanity check:

- mean(topk_prob_city_k) ≈ k / N_city

Result:

- High ranking stability under aligned generative structure
- Borderline region small (<0.3% for k ≤ 5000)

### E. Phase 1 interpretation

Under:

- coherent generative structure
- strong exposure–hazard signal
- low posterior variance

Prioritisation decisions are structurally robust.

Phase 1 establishes:

- correctness of inference
- correctness of posterior → decision mapping
- scalability to full urban extent
- baseline stability benchmark

### Phase 1 status: COMPLETE

Phase 1 closes the methodological baseline.

Future phases introduce:

- robabilistic hazard uncertainty
- model misspecification
- cross-city transfer stress tests
