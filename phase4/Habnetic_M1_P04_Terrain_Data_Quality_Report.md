# Habnetic | Phase 4 M1-P04: Rotterdam Terrain Data and Quality Report

**Version:** 0.1.0 (draft for methodological review)  
**Date:** 29 October 2026  
**Status:** Technical extraction and integrity checks completed; exploratory spatial QA partially completed; **not** empirical hydrological validation.  
**Proposed repository path:** `Habnetic/docs/phase4/Habnetic_M1_P04_Terrain_Data_Quality_Report.md`  
**Associated data repository:** `Habnetic/data`  
**Scientific baseline:** `Habnetic/resilient-housing-bayes`, frozen Phase 3 / M0 at commit `5e19254`, tag `phase3-code-freeze-20261022`.

## 1. Purpose and decision

Document the provenance, reproducibility, quantitative validation, measurement sensitivity and known limitations of Rotterdam's Phase 4 M1 topographic covariates. This report supports **controlled experiments on constrained selection under uncertainty**. Urban flooding is the experimental domain, not Habnetic's exclusive purpose.

**Provisional methodological decision:** retain the AHN5 5 m DTM as the primary terrain source, the local-minus-neighbourhood median (`T50`) as the primary candidate covariate and local-minus-neighbourhood lower quartile (`T25`) as a sensitivity covariate. Preserve both raw values and their quality metadata. Do **not** interpret either covariate as flood depth, flood probability or independently verified protective elevation.

**Release gate:** the dataset is ready for a *pre-registered synthetic decision experiment*, subject to the restrictions in the accompanying M1-A/M1-B protocol. It is not a certified flood-risk layer, and the spatial QA sample of 36 has not been exhaustively inspected.

## 2. Evidence and provenance

| Item | Recorded specification |
|---|---|
| Study area | Rotterdam (RTM), municipality; terrain search area buffered by 105 m |
| Asset universe | 221,324 Phase 3 OSM-derived building geometries; authoritative join key `bldg_id` |
| Buildings CRS | EPSG:28992 (Amersfoort / RD New) |
| Terrain source | AHN5 bare-earth Digital Terrain Model, 5 m, source-tile filenames from `bladwijzer.gpkg` |
| Raster CRS | EPSG:7415, compound RD New + NAP height |
| Source footprint | 28 tiles (24 filename-year 2023; 4 filename-year 2024) |
| Mosaic | 5 m, 10,000 × 5,000 pixels; bounds RD New `[55000, 425000, 105000, 450000]` |
| Mosaic NoData | `-9999.0` (`float32`), explicitly replacing source's problematic maximum-float NoData representation |
| Mosaic size | 72.07 MiB, as logged |
| Measurement date | Processing reported on 29 October 2026; source *acquisition dates* not independently verified beyond filename years |
| Input source and version details | Source URL and SHA-256 of each tile recorded in `ahn5_5m_download_inventory.csv`; complete SHA-256 values not reproduced here |

**Important:** different filename years may indicate different survey vintages. Survey-boundary effects and temporal alignment with OSM features have not been independently validated. A GeoPackage building label is not proof of current building use or geometry accuracy.

## 3. Feature definitions

Let \(B_i\) be building footprint \(i\), \(\operatorname{buffer}(B_i,r)\) its planar buffer at radius \(r\) metres, and \(z\) valid DTM elevations in metres relative to NAP.

- **Local ring:** \(L_i = \operatorname{buffer}(B_i,20)\setminus B_i\).
- **Neighbourhood ring:** \(N_i=\operatorname{buffer}(B_i,100)\setminus \operatorname{buffer}(B_i,20)\).
- **Local elevation:** \(z_{L50,i}=\operatorname{median}\{z(x):x\in L_i,\ x\text{ valid}\}\).
- **Neighbourhood reference:** \(z_{N50,i}=P_{50}(z\mid N_i)\), \(z_{N25,i}=P_{25}(z\mid N_i)\).

\[
T50_i=z_{L50,i}-z_{N50,i},\qquad
T25_i=z_{L50,i}-z_{N25,i}.
\]

A feature is **measured** only when each ring contains at least **20 valid pixels**. This is a provisional *availability* rule, not a calibrated reliability threshold. Coverage is the fraction of pixels selected by the rasterized ring that contain valid elevation. The extraction uses pixel-centre selection and preserves missing values; no interpolation or filling is applied.

**Sign convention:** positive `T50` indicates higher local median ground than the neighbourhood median, negative indicates lower. Neither implies a monotonic relationship to flood damage without a separately justified physical and probabilistic model.

## 4. Acquisition and technical validation

1. **Tile planning:** 27 tiles intersected the municipality; adding the 105 m margin required one more, `2023_M5_37CN1.TIF`, giving **28**.
2. **Acquisition:** 27 downloaded, 1 reused, 0 failures. Filenames, HTTPS URLs, sizes, hashes and raster metadata were inventoried.
3. **Spatial coverage:** 28/28 source grids aligned to the 5 m raster grid; combined source footprints cover the 310.472 km² buffered study area (uncovered area 0.0 m²). No positive-area overlaps among 378 pairs.
4. **Raster content:** mean tile-level valid-data fraction 0.6249, range 0.0414–0.9135. These are whole-tile fractions, not per-building usability measures.
5. **Critical processing error found and corrected:** an initial Rasterio merge with the source maximum-`float32` NoData produced an invalid 0.22 MiB mosaic with zero-filled content. The rebuild specified `nodata=-9999.0` and produced a 72.07 MiB mosaic.
6. **Exact source regression:** all **28/28** original tiles had identical valid-data masks and **0.0 m** maximum elevation difference against their corresponding mosaic windows.
7. **Pilot regression:** all 200 original pilot buildings reproduced `T50`, `T25`, local/neighbourhood pixel counts and coverage. Maximum absolute numerical differences: `T50` \(4.44\times10^{-16}\) m; `T25` \(8.88\times10^{-16}\) m.

**Inference from these checks:** the mosaic and extraction pipeline preserve measured source values reproducibly. These checks do *not* establish ground-truth terrain accuracy or hydrological validity.

## 5. Raster-resolution and definition sensitivity: 200-building pilot

A deterministic 200-building sample from a single Rotterdam AHN tile used identical footprints, ring geometries and extraction logic with 5 m and 0.5 m AHN5 DTMs. All 200 produced paired measurements. The 0.5 m source was **not** acquired citywide.

| Metric | `T50`: 5 m vs 0.5 m | `T25`: 5 m vs 0.5 m |
|---|---:|---:|
| Paired observations | 200 | 200 |
| Median absolute difference | 0.0099 m | 0.0116 m |
| 95th percentile absolute difference | 0.0840 m | 0.0514 m |
| Maximum absolute difference | 0.6698 m | 0.1387 m |
| Spearman rank correlation | 0.9804 | 0.9834 |

For `T50`, Pearson correlation was 0.9895, and 8/200 signs differed; every sign reversal was near zero (difference < 0.04 m). At building `920749`, `T50` was +0.6905 m at 5 m versus +0.0208 m at 0.5 m. Its local medians differed by ~0.013 m, but neighbourhood medians differed by ~0.657 m. Neighbourhood P25s remained close (5 m 1.2982 m; 0.5 m 1.3214 m), consistent with a median located near the transition between two elevation regimes.

**Interpretation:** resolution agreement is strong for this particular neighbourhood statistic and pilot population; rare disagreements are consequential for individual assets. `T25` reduced the large discrepancy in the test case but measures a different topographic contrast, so its greater tail stability is **not** evidence of superior flood relevance.

## 6. Citywide outputs and completeness

**Candidate output:** `runs/rtm_m1_citywide/terrain_features_m1_5m.parquet` in `Habnetic/data`. It consolidates **222** deterministic batches.

| Integrity / availability | Observed |
|---|---:|
| Rows | 221,324 |
| Unique `bldg_id` | 221,324 |
| Exact agreement with source building IDs | PASS |
| Agreement with 200-building pilot | PASS |
| Measured (`terrain_measured=True`) | 220,456 (99.608%) |
| Missing | 868 (0.392%) |
| Missing because local ring only failed 20-pixel rule | 854 |
| Missing because both rings failed | 14 |
| Missing because neighbourhood only failed | 0 |

| Measured distribution | `T50` (m) | `T25` (m) |
|---|---:|---:|
| Count | 220,456 | 220,456 |
| Mean | +0.0178 | +0.2470 |
| Standard deviation | 0.3134 | 0.4362 |
| Minimum | −11.2291 | −9.8809 |
| P05 | −0.3077 | −0.0442 |
| Median | +0.0166 | +0.1494 |
| P95 | +0.3145 | +0.8558 |
| P99 | +1.0898 | +2.2901 |
| Maximum | +16.0828 | +17.4080 |

Average local and neighbourhood coverages across all buildings were **0.7400** and **0.7475** (medians 0.7532 and 0.7595). Among measured buildings, **198** had either coverage below 25%; **11,239** below 50%; **137,625** below 75%. These are diagnostics, not exclusion rules.

**Extreme observations:** `T50 < −3 m`: **73**; `T50 > +3 m`: **119**, totaling **192** (0.0871% of measured assets). Only 11/192 had either coverage below 50%. A combined quality-case export flagged **1,256** unique assets based on missingness, extreme `|T50| > 3 m`, or either coverage <25% (some categories overlap).

## 7. Asset heterogeneity and targeted spatial assessment

A **36-asset QA GeoPackage** was produced: six extreme positives, six extreme negatives, five with zero local valid pixels, five with partial local coverage, six low-coverage measured cases and eight nominal controls. **Generation of the sample does not mean all 36 have been visually adjudicated.** Two extreme cases received closer QGIS scrutiny:

| Asset | Evidence | Current classification |
|---|---|---|
| `318017` | `local_z_50=+4.066 m`, `neighbourhood_z_50=−12.0168 m`, `T50=+16.0828 m`, local coverage 0.8991, neighbourhood coverage 0.4078. QGIS spot values in the vicinity included +4.77 and −12.25 m. | **Extreme heterogeneous terrain:** correct arithmetic and raster extraction; physical cause of deep rectangular depression unverified; flood significance unknown. |
| `262192` | `T50=−11.2291 m`; QGIS spot values around a deep transport corridor ranged approximately −18.62 to +3.25 m. OSM `building=industrial`, MultiPolygon, area 546.69 m²; centroid RD `(77870.617, 435412.286)`. Map context places it on the A24 corridor near the Maasdeltatunnel access. | **Special infrastructure / asset-definition uncertainty:** strong local elevation discontinuity; whether mapped object belongs to the intended intervention population unresolved. |

Nearby QGIS pixel samples are *not* substitutes for medians within the exact buffer rings. The specific engineering features and asset use should be verified against independently sourced mapping/history before a physical classification is finalized.

**OSM inventory composition:** `building=yes` 96,287; `house` 83,272; `apartments` 26,913; `shed` 7,480; `industrial` 2,060; etc. The top 20 types comprise 221,059 assets; **265 are in other types**. The `yes` label does not establish residential use. Among measured items, the incidence of extreme `|T50|>3 m` was 0.935% for `industrial` (19/2,032), 4.762% for `roof` (13/273) and 0.019% for `apartments` (5/26,909). Only 240/297 `houseboat` features had measured terrain. These descriptive associations do not establish causes or systematic source errors.

## 8. Known limitations and failure modes

- **Hydrological interpretation:** geometric rings ignore drainage pathways, water barriers, connectivity, flow direction and flood defence infrastructure. High `T50` is not necessarily protective.
- **Mixed terrain:** excavations, motorway approaches and bimodal terrain can make a neighbourhood median physically unrepresentative despite mathematically correct extraction.
- **Spatial NoData:** valid pixels may not be spatially representative; 20-pixel sufficiency is not scientifically calibrated. Missingness may be systematically related to asset type and geography.
- **Resolution sensitivity:** 0.5 m versus 5 m agreement was assessed on 200 assets in *one tile*. It does not establish citywide agreement or which product is more accurate.
- **Time and provenance:** a mix of 2023/2024 filename vintages and an OSM-derived asset inventory can create temporal mismatch, particularly at construction sites and new infrastructure.
- **Asset definition:** OSM building categories are not a validated residential eligibility list. Treat the full original Phase 3 inventory as the descriptive reference; any narrowed eligibility universe must be a separately labelled, paired sensitivity experiment.
- **Physical QA scope:** only selected cases were inspected; the 36-feature QA sample was generated but not exhaustively signed off. No observed flood-damage data were used to validate `T50`/`T25`.
- **Scientific baseline:** Phase 3 damage outcomes are synthetic. Adding measured terrain does not itself validate real flood-damage predictions.

## 9. Reproducibility and artifact index

**Repository:** `Habnetic/data`. The following commands refer to files already created during this work; do not present them as a new package API. Run from the data repository root using the project environment and pin its package versions before formal release.

```powershell
python scripts/rtm/m1_download_ahn5_5m.py
python scripts/rtm/m1_build_terrain_mosaic.py      # only if mosaic does not exist
python scripts/rtm/m1_validate_mosaic.py
python scripts/rtm/m1_validate_pilot_mosaic.py
python scripts/rtm/m1_extract_terrain_citywide.py --all
python scripts/rtm/m1_consolidate_terrain.py
python scripts/rtm/m1_export_spatial_qa.py          # only if QA export does not exist
```

The mosaic builder and QA exporter intentionally refuse to overwrite existing outputs; do not rerun indiscriminately. The extractor reuses per-batch outputs based on IDs and row counts, **not** on checksums of code, configuration and inputs. Preserve the exact run environment and scripts used for the existing batch set; formal release needs code commit, dependency lock and dataset hash.

| Artifact (relative to `Habnetic/data`) | Role |
|---|---|
| `raw/RTM/terrain/ahn/bladwijzer.gpkg` | AHN tile index |
| `runs/rtm_m1_citywide/ahn5_5m_tile_manifest_buffered.csv` | 28-tile manifest |
| `runs/rtm_m1_citywide/ahn5_5m_download_inventory.csv` | Per-file URL, SHA-256, metadata |
| `runs/rtm_m1_citywide/ahn5_5m_coverage_validation.csv` | Source pixel-coverage fractions |
| `runs/rtm_m1_citywide/ahn5_5m_mosaic_integrity.csv` | Exact 28-tile source-to-mosaic check |
| `processed/RTM/derived/ahn5_dtm_5m_rtm_mosaic.tif` | Validated 5 m mosaic |
| `runs/rtm_m1_terrain_pilot/terrain_resolution_comparison.csv` | 200-asset resolution comparison |
| `runs/rtm_m1_terrain_pilot/terrain_quantile_sensitivity.csv` | P25/P50 sensitivity |
| `runs/rtm_m1_terrain_pilot/mosaic_pilot_regression.csv` | 200-asset regression |
| `runs/rtm_m1_citywide/terrain_batches/` | 222 batch Parquet files |
| `runs/rtm_m1_citywide/terrain_features_m1_5m.parquet` | Consolidated per-asset terrain candidate |
| `runs/rtm_m1_citywide/terrain_quality_cases.csv` | 1,256 flagged QA records |
| `runs/rtm_m1_citywide/terrain_spatial_qa_sample.gpkg` | 36 targeted spatial review cases |

**Before marking a scientific artifact as `checked`:** record exact code commits, Python/geospatial dependency versions, SHA-256 of the consolidated Parquet and mosaic, the Phase 3 asset snapshot, and the methodological decision/QA outcomes. A `checked` artifact is not synonymous with empirically validated data. Use the existing [Experiment Artifact Contract](Experiment_Artifact_Contract.md) when publishing a curated bundle, without copying large raw terrain assets into Git.

## 10. Decision and next tests

**Decision `M1-D01` (provisional):** proceed to the registered M1-A and M1-B synthetic decision experiments with `T50` primary and `T25` as sensitivity. Preserve extreme and missing observations with explicit flags. Do not automatically substitute 0.5 m when rankings change, reinterpret a negative/positive `T50` as hydrological risk, or filter OSM classes post hoc.

**What to test next**

1. Lock the exact shared evaluation universe and model/decision implementation against Phase 3 / M0.
2. Run the protocol's negative-control experiment M1-A before the synthetic terrain-effect experiment M1-B.
3. Report decision impacts and predictive diagnostics alongside terrain missingness, classification and measurement sensitivity; do not claim empirical flood prediction.
