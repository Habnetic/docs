# RTM Phase 1a — Pluvial Hazard Implementation

Status: COMPLETE  
Date: 2026-02-26

---

## Objective

Replace synthetic hazard proxy (H_pluvial_v0) with a real deterministic
ERA5-Land-based pluvial intensity signal.

---

## Change Introduced

Synthetic constant forcing:
H_pluvial_v0 = 1.0

Replaced by:

H_pluvial_v1 = mean annual maximum 1-hour precipitation (1991–2020)

See canonical definition:
Habnetic/docs/references/hazard/rtm_pluvial_v1.md

---

## Artifacts Produced

Grid:
processed/RTM/hazards/pluvial/H_pluvial_v1_grid.nc

Buildings:
processed/RTM/hazards/pluvial/H_pluvial_v1_buildings.parquet

Rows: 221,324 (complete building coverage)

---

## As-Built Range (RTM)

Grid:
min ≈ 25.38  
p50 ≈ 25.78  
p95 ≈ 26.44  
max ≈ 26.81  

Buildings:
min ≈ 25.41  
p50 ≈ 25.64  
p95 ≈ 26.01  
max ≈ 26.50  

---

## Edge Handling Decisions

- Domain clamp applied to centroids slightly outside ERA5 bbox.
- 3 grid NaN cells filled using nearest-valid-cell (KDTree) prior to interpolation.
- No probabilistic adjustments introduced.

These decisions preserve full building coverage without altering
Phase 1 deterministic semantics.

---

## Validation

QGIS sanity check:
runs/rtm_phase1_pluvial_v1/qgis_hazard_v1_sanity.png

Expected behavior:
- Smooth spatial gradient
- No checkerboard artifacts
- No tiling seams
- No missing coastal areas

---

## What Phase 1 Does NOT Claim

- No return periods
- No probabilistic hazard
- No flood depth
- No impact calibration

Phase 1 establishes a deterministic hazard scaffold only.

---

## Next Step

Introduce minimal outcome proxy and re-enable Bayesian inference
under Exposure → Hazard → Outcome structure.

## Phase linkage

- Phase 1a produces the deterministic hazard `H_pluvial_v1`.
- Phase 1b consumes `H_pluvial_v1` and introduces a minimal synthetic outcome to validate
  Bayesian inference and posterior decision stability.
  See: `phases/RTM_PHASE1B_SYNTH_OUTCOME_DECISION_STABILITY.md`.