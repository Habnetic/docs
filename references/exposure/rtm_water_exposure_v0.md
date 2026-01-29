---
title: RTM Water Exposure Prior (v0)
scope: Rotterdam (RTM)
status: stable
version: v0
last_updated: 2026-01-29
depends_on:
  - RTM hydrography (PDOK/Kadaster TOP50NL `waterdeel_lijn`)
  - RTM buildings (OpenStreetMap)
outputs:
  - processed/RTM/priors/building_water_proximity.parquet
---

# RTM — Water Exposure Prior (v0)

This document defines the **initial water exposure features** for the
Rotterdam (RTM) study area within the Habnetic ecosystem.

These features are **not probabilities** and **not hazard models**.
They are deterministic spatial metrics intended to act as **inputs or priors**
in downstream Bayesian models of hazard impact, damage, and resilience.

---

## 1. Purpose

The purpose of this exposure prior is to quantify, at building level,
the **degree to which a building is embedded in the urban water network**.

This exposure captures **spatial context**, not event likelihood.

It is designed to:
- differentiate buildings by long-term water proximity
- encode local and neighborhood-scale water presence
- support probabilistic modeling without embedding hydraulic assumptions

---

## 2. Study area and spatial reference

- **Study area**: Rotterdam municipality (RTM), Netherlands
- **CRS**: EPSG:28992 (Amersfoort / RD New)
- **Units**: meters

All distances and buffer radii are interpreted in meters under this CRS.
No spatial analysis is valid outside this reference system.

---

## 3. Data basis (fixed)

### Buildings
- Source: OpenStreetMap
- Geometry: building footprints (polygons)
- Processed layer:
processed/RTM/derived/buildings_rtm.gpkg


### Hydrography
- Provider: PDOK / Kadaster
- Dataset: BRT TOP50NL
- Feature class: `waterdeel_lijn`
- Geometry: LineString / MultiLineString (water network centerlines)
- Polygons (`waterdeel_vlak`) are **not included** in v0
- Processed layer:
processed/RTM/derived/hydrography_rtm.gpkg


Hydrography represents **where permanent water exists**, not flood extent
or flood probability.

---

## 4. Latent exposure concept

For each building *i*, define a latent variable:

\[
E_i \in \mathbb{R}^+
\]

where:

> \(E_i\) represents the **degree of long-term water exposure**
> arising from proximity to and concentration of the urban water network.

This latent exposure:
- is continuous
- is non-negative
- is uncertain
- is not directly observed

---

## 5. Observed exposure features

The latent exposure \(E_i\) is informed by the following observed,
deterministic spatial features.

All features are computed using a **representative point**
of the building footprint (guaranteed to lie within the polygon).

---

### 5.1 Distance to nearest water

**Symbol**: \(d_i\)

**Stored as**: `dist_to_water_m`

**Definition**:  
Minimum Euclidean distance (in meters) from the building representative point
to the nearest **hydrography centerline**.

**Interpretation**:
- Captures **local proximity** to water
- Smaller values imply closer embedding in the water network

**Notes**:
- Distance is purely geometric
- No distinction is made between river, canal, or other linear water features

---

### 5.2 Water length density — 250 m

**Symbol**: \(\rho_{250,i}\)

**Stored as**: `water_len_density_250m`

**Definition**:  
Total length (in meters) of hydrography centerlines within a 250 m radius
buffer around the building representative point, normalized by buffer area.

**Interpretation**:
- Captures **immediate neighborhood** water concentration
- Sensitive to small canals and dense water networks

---

### 5.3 Water length density — 500 m

**Symbol**: \(\rho_{500,i}\)

**Stored as**: `water_len_density_500m`

**Definition**:  
Total length of hydrography centerlines within a 500 m radius buffer,
normalized by buffer area.

**Interpretation**:
- Captures **district-scale** water context
- Smoother than the 250 m metric

---

### 5.4 Water length density — 1000 m

**Symbol**: \(\rho_{1000,i}\)

**Stored as**: `water_len_density_1000m`

**Definition**:  
Total length of hydrography centerlines within a 1000 m radius buffer,
normalized by buffer area.

**Interpretation**:
- Captures **broad urban water embedding**
- Reflects large rivers, harbor systems, and extensive canal networks

---

## 6. Structural assumptions

The following assumptions are **modeling assumptions**, not empirical claims.

1. **Monotonicity**
 - Exposure decreases as distance to water increases
 - Exposure increases as water density increases

2. **Saturation**
 - Marginal effects diminish at very small distances or very high densities
 - Exposure does not increase without bound

3. **Nonlinearity**
 - Linear effects are unlikely to be sufficient
 - Logarithmic or spline-like transformations are expected

4. **Partial redundancy**
 - Distance and density metrics are correlated but not interchangeable
 - Multiple scales reduce brittleness

5. **Residual uncertainty**
 - Unmodeled spatial and contextual effects are expected
 - Exposure includes irreducible noise

---

## 7. Conceptual generative structure

A generic conceptual form is:

\[
E_i =
f\!\left(
\log(d_i + \epsilon),
\log(\rho_{250,i} + \epsilon),
\log(\rho_{500,i} + \epsilon),
\log(\rho_{1000,i} + \epsilon)
\right)
+ \eta_i
\]

where:
- \(f(\cdot)\) is an unknown but constrained function
- \(\epsilon\) avoids singularities at zero
- \(\eta_i\) represents unobserved influences

This structure is intentionally underspecified at v0.

---

## 8. Interpretation constraints

The following interpretations are **explicitly invalid**:

- \(E_i\) is **not** flood probability
- \(E_i\) is **not** flood depth or duration
- \(E_i\) is **not** a risk score
- \(E_i\) is **not** directly comparable across cities without re-scaling

All causal or predictive claims require explicit hazard and impact models.

---

## 9. Failure modes to avoid

- Treating exposure as hazard
- Collapsing distance and density into a single index prematurely
- Forcing linear effects for convenience
- Ignoring CRS units
- Using exposure as a decision threshold

These are modeling errors, not data errors.

---

## 10. Empirical distribution (RTM)

The following summary statistics were computed on the full
RTM building set (221,324 buildings).

### Distance to water (`dist_to_water_m`)
- Median: ~194 m
- 10th percentile: ~58 m
- 90th percentile: ~610 m
- Maximum: ~2.36 km

The distribution is strongly right-skewed, with a long tail
corresponding to inland and industrial areas.

### Water length density

All water density metrics are zero-inflated and right-skewed.

- At 250 m:
  - Median: ~0.00030
  - 75% of buildings have very low or zero local water presence
- At 500 m and 1000 m:
  - Distributions are smoother
  - Strong correlation across scales

### Correlation structure

- Distance is negatively correlated with all density measures
  (ρ ≈ −0.43 to −0.47)
- Density metrics across scales are strongly correlated
  (ρ ≈ 0.57–0.82)

This confirms:
- partial redundancy
- multi-scale stability
- suitability for joint modeling with regularization

---

## 11. Status and validation

- Version: v0
- Spatial preprocessing: complete
- Numerical validation: complete
- QGIS validation: complete
- Coverage: all 221,324 buildings in RTM

This exposure prior is **stable** and ready for:
- exploratory analysis
- prior predictive reasoning
- integration into Bayesian hazard–impact models

Derived values are stored in:
`processed/RTM/priors/building_water_proximity.parquet`

Generated by:
`scripts/rtm/compute_building_water_exposure.py`


---

## 12. What to test next

- Sensitivity to distance-only vs distance + density
- Effect of scale selection (250 / 500 / 1000 m)
- Transform choice (linear vs log)
- Interaction with hazard intensity variables

No additional spatial preprocessing is required for these steps.
