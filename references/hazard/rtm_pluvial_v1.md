# RTM Pluvial Hazard v1 (ERA5-Land)

Status: ACTIVE (Phase 1 deterministic intensity)  
Scope: Deterministic pluvial hazard intensity only.

---

## Definition

H_pluvial_v1 is defined as:

**Mean annual maximum 1-hour precipitation (mm) over 1991–2020**,  
derived from ERA5-Land hourly total precipitation.

---

## Formal Definition

For grid cell g:

1) Hourly precipitation:
P_t(g) = hourly total precipitation (mm)

2) Annual maxima:
AMAX_y(g) = max_t P_t(g)  for year y

3) Climatological aggregation:
H_pluvial_v1(g) = mean_y AMAX_y(g),  y ∈ [1991..2020]

Units: millimeters (mm)

---

## Interpretation

- Represents typical annual worst-hour rainfall intensity.
- Deterministic climatological intensity index.
- Not a probability.
- Not a return level.
- Not a flood depth.
- Not calibrated to damage.

Valid only within the RTM spatial domain and ERA5-Land grid resolution.

---

## Data Source

Dataset: ERA5-Land hourly (Copernicus/ECMWF)  
Variable: `tp` (total precipitation)

Unit conversion:
tp_mm = tp * 1000

CDS bbox (N, W, S, E):
[52.05, 4.00, 51.85, 4.65]

---

## Spatial Aggregation to Buildings

Building hazard value is obtained via:

- centroid in EPSG:28992
- reprojection to EPSG:4326
- bilinear interpolation from ERA5-Land grid

H_pluvial_v1_bldg = interp_bilinear(H_pluvial_v1_grid, centroid)

---

## Non-Goals (Phase 1)

- No return period modeling
- No GEV fitting
- No runoff modeling
- No sewer/drainage modeling
- No probabilistic hazard representation

Probabilistic hazard modeling is deferred to Phase 2.