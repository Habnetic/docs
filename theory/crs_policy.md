# Spatial Reference System (CRS) Policy

## Why this document exists

All spatial analysis in Habnetic **depends on distances, areas, and overlays being numerically correct**.

If Coordinate Reference Systems (CRS) are mixed, implicit, or chosen casually:

- distances are wrong  
- areas are wrong  
- overlays silently lie  
- Bayesian priors become expensive fiction  

This document defines the **one-CRS-per-study-area rule** and records the CRS choices for each case study.

---

## The core rule

> **One study area → one projected CRS → all datasets normalized to it**

- The CRS must be **projected**
- Units must be **meters (preferred)** or explicitly documented otherwise
- All raw datasets are reprojected during normalization
- No on-the-fly reprojection during analysis
- Scripts must assume the target CRS is already correct

If a dataset arrives in a different CRS, it is **normalized first or rejected**.

---

## CRS types (and why most are wrong)

### EPSG:4326 (WGS84)
- Latitude / longitude in degrees
- Good for GPS and global indexing
- **Terrible for distance, area, buffers**
- Typical for raw OSM and web APIs

**Never used beyond raw ingestion.**

---

### Projected CRS (what we actually use)
- Flat coordinate system
- Linear units (meters or feet)
- Locally optimized to minimize distortion

**All modeling happens here.**

---

### Unknown / mixed CRS
- QGIS may still draw it
- Overlays may appear to work
- Numbers are meaningless

**This is considered a data error.**

---

## Study area CRS registry

### Rotterdam, NL
- **CRS**: EPSG:28992  
- **Name**: Amersfoort / RD New  
- **Units**: meters  
- **Rationale**:
  - Dutch national standard
  - Minimal distortion at city scale
  - Compatible with CBS, PDOK, Kadaster

**Status**: locked and validated

---

### San Francisco, US

One option must be chosen **before** normalization begins.

#### Preferred (metric simplicity)
- **CRS**: EPSG:32610  
- **Name**: UTM Zone 10N  
- **Units**: meters  
- **Rationale**:
  - Clean metric units
  - Excellent for city-scale exposure modeling
  - Widely used in geospatial analysis

#### Alternative (agency alignment)
- **CRS**: EPSG:2227  
- **Name**: NAD83 / California zone 3  
- **Units**: US feet  
- **Rationale**:
  - Used by FEMA and local agencies
  - Requires explicit unit handling in analysis

**Decision must be recorded before data ingestion.**

---

### Concordia Station, Antarctica
- **CRS**: EPSG:3031  
- **Name**: Antarctic Polar Stereographic  
- **Units**: meters  
- **Rationale**:
  - Designed for polar regions
  - Standard in glaciology and climate science
  - Stable for regional-scale modeling

Lat/lon is not acceptable for analysis here.

---

## Extraterrestrial study areas

For non-Earth bodies, **EPSG codes are not authoritative**.

CRS definitions come from:
- IAU (International Astronomical Union)
- NASA / ESA planetary data systems

The same core rule applies:

> **One study area → one projected, metric CRS → explicitly documented**

---

### Moon (Lunar surface)

- **Reference body**: Moon  
- **Authority**: IAU / NASA  

#### Recommended projections
- Polar Stereographic (polar bases)
- Equirectangular (equatorial regions)

- **Units**: meters  
- **Rationale**:
  - Lat/lon exists but behaves like EPSG:4326
  - Polar stereographic is standard for south-pole studies
  - Used in LRO and Artemis planning

Lat/lon is allowed only for indexing and visualization.

---

### Mars (Surface habitats)

- **Reference body**: Mars  
- **Authority**: IAU / NASA  

#### Recommended projections
- Equirectangular (regional studies)
- Polar Stereographic (polar sites)

- **Units**: meters  
- **Rationale**:
  - Mars uses its own ellipsoid
  - Earth EPSG codes are meaningless here
  - Standard in HiRISE, MOLA, Mars Express data

Any dataset treated “as if Mars were Earth” is invalid.

---

## Planetary CRS summary

| Body        | EPSG used | Authority     | Units  | Notes |
|------------|----------|---------------|--------|------|
| Earth      | Yes      | EPSG          | meters | National / regional CRS |
| Antarctica | Yes      | EPSG          | meters | Polar stereographic |
| Moon       | No       | IAU / NASA    | meters | Planetary CRS |
| Mars       | No       | IAU / NASA    | meters | Planetary CRS |

---

## Implementation guidelines

For each study area, the CRS must be:

1. Declared once (README or metadata)
2. Applied during normalization
3. Verified in QGIS
4. Assumed correct by all downstream scripts

---

## Metadata declaration (YAML)

### What this is
This is **machine-readable metadata**, not configuration magic.

- Stored next to data or in a project README
- Used to document assumptions
- Can be parsed later for automation or validation

### Earth example

```yaml
study_area:
  name: Rotterdam
  body: Earth
  crs:
    epsg: 28992
    units: meters
    rationale: "Dutch national projected CRS"
