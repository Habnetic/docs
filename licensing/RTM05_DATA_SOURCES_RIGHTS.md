# Habnetic | Rotterdam data sources, licences and release evidence

**Edition:** 2026-11-20 / RTM-05, evidence consolidation v1
**State:** `TECHNICAL_PASS_PENDING_RIGHTS_AND_UI_REVIEW`
**Release decision:** `HOLD_PUBLIC_GEODATA_PENDING_OUTPUT_REVIEW`
**Scope:** Operations Center Rotterdam reference implementation. Not legal advice; not an unconditional permission to distribute or publicly serve a database.

## Decision

The scientific and technical RTM-05 automated audit passed. It checked frozen-source fingerprints (8/8), browser GeoJSON 221,324 unique buildings and 224,099 polygon copies, 868 without T50, and original AHN5 DTM presence. The *rights* work remains separate. A hosted map can disclose and distribute static files even when the source repository is private. A private GitHub repository is **not** a deployment access-control mechanism.

An internally operated, closed-source Operations Center is a viable software approach. However, access by third parties, publication of derived vector layers, or public use of an adapted OSM database may trigger ODbL attribution and database availability/share-alike requirements. **No output is pre-cleared by this note.** Review the actual use/output, not simply the nominal provider licence.

## Evidence vocabulary

- **A:** local first-party source documentation, artifact, script or verified file evidence. Proves the stated fact, **not** chain-of-custody or licence clearance by itself.
- **B:** official product-level provider licence evidenced. Must still be bound to the particular historic source file and output.
- **C:** plausible source/product based on documentation or scripts, but execution lineage incomplete.
- **U:** unknown.
- **HOLD / PENDING:** do not treat as cleared to serve publicly.

## Source register, Rotterdam

### 1. Scientific building footprints (priority P0)

**Known:** `data/raw/RTM/buildings/osm/SOURCE.md` identifies OpenStreetMap via Geofabrik, Zuid-Holland, with ODbL 1.0. `data/scripts/rtm/normalize_buildings.py` reads `raw/RTM/buildings/osm/buildings_zuid_holland.geojson`, assigning `bldg_id` where needed. A normalized `data/processed/RTM/normalized/buildings.gpkg` exists. This is strong *candidate* upstream lineage for the Phase 3 footprint collection, **not yet proof** that the frozen GPKG and RTM-02 browser shard exports came from this specific input and run.

**Status:** `HOLD_PUBLIC_VECTOR`. **Action:** preserve the original GeoJSON/extract hash and acquisition date; inspect run logs, output hashes, ID assignment, topology and normalization; document the actual `bldg_id` → browser GeoJSON join. Per exported dataset, determine ODbL classification (Produced Work, Derivative/Collective Database, original independent calculation) and any availability obligations. The scientific coefficients and synthetic metrics are not automatically relicensed as OSM database contents by virtue of association.

**Credit if applicable:** © OpenStreetMap contributors, ODbL 1.0, with accessible licence link, and note Habnetic-derived extraction/joins. Persistent on-map attribution may be required as well as this rights page.

### 2. OSM context and building heights (priority P1)

**Known:** a separate, local `zuid-holland-260117.osm.pbf` file exists, along with `operations-center/scripts/export_rotterdam_osm_pbf.py` and `export_rtm_visual_heights.py`. It was used for visual OSM context and candidate building heights. This **must not be confused** with the historical scientific footprint extraction without proving the lineage. RTM-05 found a visual-height join for 4,791/221,324 scientific buildings (2.2%); 216,533 use the schematic 12 m fallback.

**Status:** `PENDING_OUTPUT_RIGHTS_REVIEW`. **Action:** record actual source URL, acquisition date (do not infer it solely from the PBF basename), hash, exported layers and output database classification.

### 3. AHN5 elevation and 3D terrain (priority P1)

**Known:** `data/docs/provenance/RTM_AHN5_Source.md` identifies AHN5 5 m DTM, official product links, tiles and SHA256; 28/28 named original tiles match the supplied scanned file inventory. Derived `data/processed/RTM/derived/ahn5_dtm_5m_rtm_mosaic.tif` exists. The AHN programme index states AHN5/6 use **CC BY 4.0** (AHN1–4 are CC0 1.0). Link source materials and output tiles before final signoff.

**Critical epistemic warning:** RTM-05 measured `imputed=0.699972` across the Terrarium 3D **tile pixels**. Those pixels were filled from nearest-valid samples *only to repair a visual display*. They are **not** measured AHN elevations, a hydrodynamic flood surface or bathymetry. This is **not** a claim that 70% of the municipality lacks source elevation data.

**Credit candidate:** Terrain: AHN5 / AHN programme, CC BY 4.0. Adapted by Habnetic (mosaic, Terrarium rendering, nearest-valid visual fill). **Status:** `PENDING_FINAL_ASSET_CREDIT`.

### 4. CBS Rotterdam municipality boundary (priority P1)

**Known:** `data/raw/RTM/boundaries/rotterdam_municipality/SOURCE.md` names CBS Gebiedsindelingen 2025, layer `gemeente_niet_gegeneraliseerd`, CC BY 4.0. `data/scripts/rtm/normalize_boundary.py` points at the matching source GPKG/layer and writes `processed/RTM/normalized/boundary_rtm.gpkg`. The national catalogue identifies CC BY 4.0 for this product family.

**Credit candidate:** Municipality: CBS Gebiedsindelingen 2025, CC BY 4.0; normalized for display by Habnetic. The orange map boundary is a **municipal boundary**, **not** the Phase 3 scientific study-population footprint. **Status:** `PENDING_EXACT_FILE_CREDIT`. Verify source package and output hash.

### 5. Research hydrography (priority P0)

**Known:** `data/raw/RTM/hazards/hydrography/downloads/SOURCE.md` records CC BY 4.0 and `data/scripts/rtm/normalize_hydrography.py` loads `raw/RTM/hazards/hydrography/downloads/hydrography.gpkg`. That script mentions PDOK TOPNL as the *intended* input. **An instruction inside a script is not evidence that a particular TOPNL version was actually used.** Neither exact original GPKG metadata nor transformation→Phase 3 exposure chain has been fully verified.

**Status:** `HOLD_EXACT_SOURCE`. Read full original `SOURCE.md`, inspect the original GeoPackage's `gpkg_metadata`/layers, archive its acquisition URL/version/licence and verify the actual processing run and exported browser/Phase 3 outputs. Do **not** publish "TOPNL CC BY 4.0" as a final assertion on the strength of this script alone.

### 6. Phase 3 ERA5-derived precipitation proxy (priority P2)

**Known:** the October 2026 preprint describes an ERA5-derived pluvial proxy. Local ERA5-Land NetCDF candidates exist, but have not been linked to the archived Phase 3 variable. Do not infer Phase 3 used ERA5-Land, nor retroactively cite Phase 4 M1 AHN5 as a Phase 3 input.

**Status:** `HOLD_EXACT_DATASET`. Find the precise CDS dataset identifier/DOI, variables, dates, aggregation, download log, transformations and corresponding frozen feature hash. Cite the actual C3S/ECMWF product and applicable licence.

### 7. Basemap, hosted APIs, code assets (priority P1)

**Known:** MapLibre and other dependencies were inventoried from package declarations, but map style URLs, tile providers, fonts, icons, remote weather calls, service policies and actual bundled notices have not been audited at runtime.

**Status:** `PENDING_RUNTIME_AND_NOTICE_REVIEW`. Inspect `npm` licence notices and the browser network tab in the production build; verify required map-level credits and third-party service policies. A service's terms are distinct from its data's licence.

## Public asset and deployment controls

1. Treat **every** file in `operations-center/public/` and the production `dist/` as potentially downloadable. The scanner classified **733** entries as `browser-served` in its summary; the detailed inventory included **729** such rows (~416 MB), which should be reconciled before making a quantitative asset claim. The high-level numbers are a triage signal, not a clearance check.
2. Earlier audit found local Windows user paths exposed in `public/local-research/rtm/context/osm_context_manifest.json` and `research_hydrography_manifest.json`. Re-export or sanitize **without breaking the client**, then scan the production `dist/` for absolute paths/secrets.
3. Access control must cover static assets, not just the homepage. A "private" frontend with unprotected tile/GeoJSON URLs is not private.
4. Keep private file inventory, internal paths, hashes, audit CSVs and download logs under `.local-cache/`; do not publish them as app notices by accident.
5. The map should display provider-specific attribution visibly alongside the map when required, with a separate **Audit → Data sources & licences** for full notices and caveats.
6. Do not label source review `CLEARED` until original file, version, upstream notice, transformations, intended output and applicable access/use are signed off.

## Scientific scope that must appear in the UI

`P(top-k)` is *posterior inclusion in a capacity-constrained selection set*, not the probability that a building will flood. Synthetic outcomes and scenario comparisons are conditional model demonstrations, not observed flood damages. Phase 3 and Phase 4 M1-G2 must be credited separately. Visual building heights are predominantly schematic (12 m fallback). AHN5 Terrarium nearest-valid fill is illustrative, not science.

## Outstanding release tasks

See [RTM05_RELEASE_GATES.md](RTM05_RELEASE_GATES.md). **Do not tag `rotterdam-reference-v1` until rights/output controls, map credits, privacy scan and UI acceptance have been completed.** A private, internal reference build may still be useful with the recorded HOLD state.

## References

- Local evidence: `reference_evidence.csv`, `source_file_inventory.csv`, `REPORT.md` from Habnetic provenance scanner (private audit artefacts; available to project owner).
- Local records: `data/raw/RTM/buildings/osm/SOURCE.md`, `data/docs/provenance/RTM_AHN5_Source.md`, `data/raw/RTM/boundaries/rotterdam_municipality/SOURCE.md`, `data/raw/RTM/hazards/hydrography/downloads/SOURCE.md`.
- [ODbL 1.0 legal text](https://opendatacommons.org/licenses/odbl/1-0/) · [OSM attribution guidelines](https://osmfoundation.org/wiki/Licence/Attribution_Guidelines) · [OSM licence FAQ](https://osmfoundation.org/wiki/Licence_and_Legal_FAQ).
- [AHN edition-specific licensing](https://basisdata.nl/hwh-ahn/AUX/bladwijzer/index.html) · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- [CBS dataset catalogue](https://data.overheid.nl/dataset/83494-cbs-gebiedsindelingen) · [PDOK copyright index](https://www.pdok.nl/copyright).
- [ERA5/C3S catalogue](https://cds.climate.copernicus.eu/) · [MapLibre licensing project](https://maplibre.org/maplibre-gl-js/docs/).

---

**Method:** evidence-level rights/provenance summary from frozen November 2026 local scan, source register and RTM-05 report. **Not a licence opinion or independent file-by-file legal signoff.**
