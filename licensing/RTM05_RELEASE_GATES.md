# RTM-05 | Rights, provenance and deployment release gates

**State as of 2026-11-20:** Technical checks passed; rights and UI checks pending. These tasks do not reopen the Bayesian model.

## P0 · Must resolve before third-party/public geodata exposure

- [ ] **RTM scientific footprints:** hash and evidence chain source Geofabrik file → `normalize_buildings.py` run → normalized GPKG → RTM-02 shards / 221,324 IDs; record version, exact licence and modifications.
- [ ] **ODbL classification per output:** for `buildings` GeoJSON/shards, joined P(top-k), OSM context and height lookup, decide Produced Work vs Derivative/Collective Database and applicable notice/share-alike/access-offer obligations. Seek legal review where classification is disputed.
- [ ] **RTM hydrography:** exact original GPKG package/layer/provider/version/terms, extraction date, normalize/export chain, whether it also generated historical Phase 3 water-distance covariates.
- [ ] **Deployment privacy:** authenticate all served app/static file URLs including `/local-research/` and `/rights/` if intended only for authorised users. Verify unauthenticated requests cannot retrieve GeoJSON or terrain tile assets.
- [ ] **Remove sensitive paths:** inspect both source `public/` and built `dist/` for `C:\\Users\\`, `file:///`, secrets, internal hosts. Prior audit flagged two context manifests. Back up before modifying them; re-build and re-test the app.

## P1 · Before RTM reference release tag

- [ ] **AHN5 output attribution:** link 28 archived source hashes/source metadata to exported terrain asset(s), credit AHN5 CC BY 4.0, disclose resampling/nearest-valid visual 3D reconstruction (~70% of tile pixels).
- [ ] **CBS output attribution:** verify exact `cbsgebiedsindelingen2025.gpkg`, layer, source terms and rendered municipality geometry; provide CBS CC BY 4.0 link; distinguish municipality vs scientific cohort.
- [ ] **Map-level credits:** confirm attribution is actually visible *while viewing Rotterdam*, not only on a separate legal page; include OSM/ODbL and terrain/boundary where applicable, with functional links.
- [ ] **Runtime dependencies/services:** audit MapLibre, fonts/icons and licence notices; browser network endpoints, map tile providers, weather endpoints, acceptable-use and privacy terms.
- [ ] **UI acceptance:** DON and RTM scenarios/legend, inspector, tools, boundary, weather, 2D/3D; source rights page accessible; scientific caveats accurate.
- [ ] **Publication manifest:** record each served asset path and SHA256, upstream source IDs, code commit/tag, output-specific clearance decision and intended access model.
- [ ] **Version/release:** commit OC and Habnetic/docs separately, ensure no private CSV/paths staged, then tag only after checks pass.

## P2 · Research documentation follow-up

- [ ] **Phase 3 ERA5:** identify exact CDS dataset ID, DOI, variable, time period, aggregation, raw file/download, transform, frozen proxy; separate Phase 3 and later Phase 4 inputs.
- [ ] **Paper maps:** add original building/base-layer source credits for the 2026 preprint's Phase 3 figures in a future editorial correction where appropriate.
- [ ] **Cross-city extension:** verify Donostia footprint/hydrography and Hamburg footprint/hydrography provenance, plus Hamburg four-source composite, independently of Rotterdam.

## Release states

- `TECHNICAL_PASS_PENDING_RIGHTS_AND_UI_REVIEW`: current.
- `PRIVATE_DEMO_WITH_HOLDS`: permissible project status only after verifying truly internal/restricted access and required internal notices. Not a licence clearance.
- `PUBLIC_GEODATA_CLEARED`: **not** assign automatically. Only after source-specific/output-specific human review and signoff.

**Assumptions:** Current work is on `feat/rotterdam`; the user's `Habnetic/docs` folder is a separate local Git repository. Installer does not commit or deploy.

**How to validate:** inspect both new rights pages and Git diffs; run `npm run build`, existing tests, and source/privacy checks; compare notice wording to original source terms.

**Next small step:** resolve source-to-GeoJSON lineage for building geometries and identify exact TOPNL hydrography input before exposing any new data bundles.
