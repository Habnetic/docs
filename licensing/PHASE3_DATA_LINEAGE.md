# Habnetic | Phase 3 Data Lineage and Source Evidence

**Evidence snapshot:** 2026-11-20
**Scope:** Rotterdam (RTM), Hamburg (HAM), Donostia (DON), frozen Phase 3 research inputs
**Status:** `SOURCE_FAMILIES_IDENTIFIED__EXACT_FROZEN_LINEAGE_PENDING`
**Publication note:** This document records research provenance and known uncertainties. It is not a declaration that every input or derivative dataset has been cleared for unrestricted redistribution.

## 1. Purpose and evidence standard

This record distinguishes **source identity**, **intended processing pipeline**, **file identity**, and **verified input-to-output execution**. These are separate claims. In particular, a file SHA-256 confirms the identity of the file hashed on 2026-11-20; it does **not** independently prove which script execution produced it or that a frozen published Parquet was derived from it.

**Evidence labels**
- **Identified:** source product, provider, or input file supported by an acquisition record and/or inspected file metadata.
- **Pipeline documented:** processing scripts and configured input/output paths examined.
- **Fingerprint captured:** SHA-256 recorded for a specific local file.
- **Frozen lineage verified:** archived run evidence, manifests, or reproducible comparison binds a particular source and processing configuration to the published Phase 3 artifact. **Not yet established by this review.**

## 2. Cross-city source summary

| City | Hydrography source | Hydrography evidence | Pluvial hazard source | Hazard lineage evidence |
|---|---|---|---|---|
| RTM | Kadaster / PDOK, **BRT TOP50NL**, `top50nl_waterdeel_lijn` | Acquisition record, inspected GeoPackage layer inventory, normalization and municipal clipping scripts, three SHA-256 fingerprints | Copernicus CDS, **ERA5-Land hourly total precipitation (`tp`)** | City pipeline scripts identified; exact acquisition period and frozen-output binding pending |
| HAM | Hamburg, **Basis-Gewässernetz** | Local acquisition record (2026-04-16), normalization and Hamburg clipping script | Copernicus CDS, **ERA5-Land hourly `tp`**, 1991–2020 | Local acquisition record (2026-04-16), processing scripts identified; frozen-output binding pending |
| DON | Gipuzkoa Provincial Council / B5M, **Rivers and Streams** | Local source record (2026-04-16), provider identified; exact source file and terms pending | Copernicus CDS, **ERA5-Land hourly `tp`** | City pipeline scripts identified; exact acquisition period and frozen-output binding pending |

### 2.1 Rotterdam hydrography: evidence details

- **Original local file:** `data/raw/RTM/hazards/hydrography/downloads/hydrography.gpkg` (~3.94 GB).
- **Acquisition document:** `data/raw/RTM/hazards/hydrography/downloads/SOURCE.md`.
- **Recorded provider and product:** PDOK / Kadaster, **BRT TOP50NL**.
- **Recorded access:** PDOK TOPNL Atom feed, `https://service.pdok.nl/brt/topnl/atom/top50nl.xml`.
- **Recorded retrieval date:** 2026-01-15; **recorded dataset update:** 2025-12-04. The GeoPackage's layer timestamps are not independent proof of the official release date.
- **Recorded product licence:** **CC BY 4.0**; confirm the attribution wording required by the actual source release before redistribution. This is source-product licensing, not a licence automatically applied to every Habnetic output.
- **CRS:** EPSG:28992 (Amersfoort / RD New), confirmed in `gpkg_contents` for the inspected layers.
- **Selected input layer:** `top50nl_waterdeel_lijn`, confirmed both by the acquisition document and the hard-coded `raw_layer` in `data/scripts/rtm/normalize_hydrography.py`. `top50nl_waterdeel_vlak` also exists but is not selected in this pipeline.
- **Pipeline:** `normalize_hydrography.py` selects line geometries, normalizes fields and writes `processed/RTM/normalized/hydrography.gpkg`; `clip_hydrography_to_rtm.py` clips against `processed/RTM/normalized/boundary_rtm.gpkg` and writes `processed/RTM/derived/hydrography_rtm.gpkg`.
- **Interpretation:** the derived water-distance and buffer-length-density indicators reflect proximity to the **selected generalized waterway centreline network**. They are *not* flood probabilities, inundation depths, return periods, or a complete hydraulic model. Water polygons are excluded from this version. Generalization, missing smaller waterways and municipal-boundary effects limit cross-city comparability.

#### Rotterdam hydrography SHA-256 fingerprints

Fingerprints supplied from local `Get-FileHash -Algorithm SHA256` on 2026-11-20:

| File, relative to `Habnetic/data` | SHA-256 |
|---|---|
| `raw/RTM/hazards/hydrography/downloads/hydrography.gpkg` | `825C02A073774F9998A6A8C13600FC78869B5B8489DEAC08CC47A1DB110591F6` |
| `processed/RTM/normalized/hydrography.gpkg` | `9D89AA9737D3E76D44483D8CE38E3CF22CA04134E8FEB2F062AAC00B3E256C5E` |
| `processed/RTM/derived/hydrography_rtm.gpkg` | `92CCD739298B09C7F44E282C4B06018CA59582C097C37C2A71DE5AB9DE6D78C3` |

These files are **not** automatically approved for inclusion in public Git repositories or static-web bundles. Preserve the large original file locally and record its identity rather than committing it.

### 2.2 Hamburg hydrography

- **Acquisition record:** `data/raw/HAM/hazards/hydrography/SOURCES.md`.
- **Provider and product recorded:** Freie und Hansestadt Hamburg / Basis-Gewässernetz Hamburg (the source note names BUE).
- **Acquisition:** Hamburg Transparenzportal / WFS, 2026-04-16.
- **Licence recorded:** Datenlizenz Deutschland – Namensnennung 2.0 (**DL-DE BY 2.0**). Check the exact dataset service/attribution statement in the original record for publication.
- **CRS recorded:** source EPSG:3044, normalized EPSG:25832.
- **Pipeline:** `data/scripts/ham/clip_hydrography_to_ham.py` uses `processed/HAM/normalized/boundary_ham.gpkg`, clips the hydrography, and writes `processed/HAM/derived/hydrography_ham.gpkg`.
- **Documentation correction required:** the closing note in `SOURCES.md` referring to a **Donostia** boundary is inconsistent with the Hamburg script. Remove or correct the note only after confirming whether the related geometry-filtering statement applies to Hamburg; do not infer a computational error from this wording alone.
- **Known limitation recorded:** inland-network emphasis may leave coastal or sea-water context incompletely represented, affecting extreme building-to-water distances.

### 2.3 Donostia hydrography

- **Acquisition record:** `data/raw/DON/hazards/hydrography/SOURCE.md`.
- **Provider and product recorded:** Gipuzkoa Provincial Council / B5M, Rivers and Streams; retrieved 2026-04-16, GeoPackage.
- **CRS recorded:** normalized EPSG:25830; the raw file's CRS remains unconfirmed in the source note.
- **Rights status:** the local note says only “official open geographic data service”; it does not establish the exact licence or exceptions for this downloaded layer. Verify the dataset-specific terms, then record precise attribution.
- **Interpretation:** hydrographic centreline/network proximity, not flood occurrence probability.

## 3. Climate hazard: ERA5-Land

The three city repositories contain corresponding download, annual-aggregation, hazard-grid, and building-mapping scripts:

1. `data/scripts/<city>/hazards/pluvial/01_download_era5_land_tp.py`
2. `data/scripts/<city>/hazards/pluvial/03_year_check_and_concat.py`
3. `data/scripts/<city>/hazards/pluvial/04_compute_H_pluvial_v1_grid.py`
4. `data/scripts/<city>/hazards/pluvial/05_map_H_pluvial_v1_to_buildings.py`

Here `<city>` is `rtm`, `ham` or `don`. The Donostia download script specifies the CDS dataset identifier `reanalysis-era5-land`; the pipeline constructs the `H_pluvial_v1_mm` signal and maps it to building identifiers. Hamburg's acquisition note explicitly records **hourly `tp`, 1991–2020**, subset bounding box `[53.75, 9.55, 53.35, 10.45]`, and retrieval **2026-04-16**. Do not automatically generalize those exact dates, parameters or temporal ranges to RTM and DON until their own acquisition records are checked.

**Scientific caution:** this is an ERA5-Land-derived precipitation *proxy*. It is not a hydraulic inundation model, observed building damage, or a city-specific flood return-period map. The precise annual-maxima/aggregation definition, grid-to-building sampling choice, versions and execution parameters should be copied from the frozen pipeline manifests before making more specific claims.

**Relevant source-family identifier:** Copernicus Climate Data Store, ERA5-Land (`reanalysis-era5-land`), DOI `10.24381/cds.e2161bac`. Verify applicable CDS/ECMWF attribution and data-use notices at the time of release.

## 4. Published Phase 3 feature tables

Observed in public `resilient-housing-bayes` branch `origin/main` during the 2026-11-20 inventory:

| Published artifact, relative to `resilient-housing-bayes` | Rows | Columns |
|---|---:|---:|
| `outputs/phase3/RTM/phase3_features_scaled.parquet` | 221,324 | 16 |
| `outputs/phase3/HAM/phase3_features_scaled.parquet` | 341,530 | 16 |
| `outputs/phase3/DON/phase3_features_scaled.parquet` | 7,755 | 16 |
| `outputs/rtm/water_exposure_Ehat_v0.parquet` | 221,324 | 6 |
| `outputs/rtm/outcome_damage_v0.parquet` | 221,324 | 2 |
| `outputs/rtm/hazard_pluvial_v0.parquet` | 221,324 | 2 |

Schema inspection found **no explicit geometry, coordinate, address, or building-name columns** in these six Parquets. It did find `bldg_id` and derived water/hazard indicators; the three city feature tables also contain `p_damage_true` and `Y_damage`, used for **synthetic outcomes**, not actual observed flood damage or calibrated real-building probabilities. The absence of geometry does **not**, by itself, resolve database-right or attribution obligations for building-level derived data.

**Publication decision:** preserve scientific reproducibility inputs while the upstream licensing and database-right classification is reviewed; this record does not declare the six Parquets unrestricted or relicense them as MIT.

## 5. Open verification gates

- [ ] Bind the three Rotterdam hydrography fingerprints to archived run manifests and, if possible, verify the precise input-to-output transformation.
- [ ] Capture hashes of the **six published Parquets** to freeze the public research-input identity; compare against the archived Phase 3 generation manifests.
- [ ] Confirm exact RTM and DON ERA5-Land acquisition periods, processing parameters and frozen-output fingerprints; confirm HAM's archived run.
- [ ] Record exact Hamburg hydrography download/service URL, attribution and source file hash.
- [ ] Record exact Donostia B5M dataset URL, downloaded file hash, source CRS and dataset-specific usage terms.
- [ ] Check `bldg_id` provenance and derivative-database obligations for public tables across cities.
- [ ] Remove or correct the stale Donostia-boundary note in Hamburg's `SOURCES.md` after checking the geometry-filtering claim.
- [ ] Add cross-references from the appropriate source registers and experimental README without duplicating conflicting licensing statements.

## 6. Scope exclusions and safeguards

- `data/raw/RTM/hazards/river_margin/SOURCE.md` describes **Phase 0 regulatory waterway-context proxies** and illustrative filenames; it is **not** evidence for Phase 3 TOP50NL hydrography acquisition.
- This investigation does **not** modify the frozen Phase 3 models, their results, or the tagged local/private `rotterdam-reference-v1` Operations Center release.
- Public code licences, upstream data licences, data-product licences and permissions to redistribute packaged web assets are separate concerns.

## Subsequent publication audit closure

Update: 2026-11-28.

The original evidence snapshot above is retained as a
historical record.

The subsequent Phase 3 technical licensing investigation
was closed with documented limitations. Source attribution
and historical precipitation interpretation were updated
in manuscript v1.2 and the companion dataset release.

See the [Phase 3 publication closure](../phase3/PHASE3_PUBLICATION_CLOSURE_2026-11-28.md).

Residual exact historical lineage limitations remain
applicable.
