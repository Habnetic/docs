# Habnetic | Operations Center v0.1 Specification

**Version:** 0.1.0  
**Date:** 29 October 2026  
**Status:** Approved design target; **not implemented**  
**Repository:** `Habnetic/docs`  
**Intended path:** `phase4/Operations_Center_v0.1_Specification.md`  
**Related specification:** `phase4/Experiment_Artifact_Contract.md` (v0.1.0)  
**Initial scientific reference:** Phase 3, M0 code tag `phase3-code-freeze-20261022` (commit `5e19254`)

## 1. Purpose

The Habnetic Operations Center is a public, read-only interface for inspecting research into probabilistic inference, constrained selection, and decision stability under uncertainty. **Urban flooding is the first experimental domain, not the purpose or permanent scope of Habnetic.**

Version 0.1 makes existing **Phase 3** research inspectable without running the scientific pipeline. It establishes a reusable presentation and publication pattern for future Phase 4 experiments, including M1, without requiring those experiments to exist first.

**Governing principle:** The scientific pipeline generates evidence; a separate, reviewed publication step exposes selected artifacts; the interface reads them. A successful UI does not constitute empirical validation.

## 2. Audience and scientific boundaries

**Primary audience:** researchers, potential collaborators, technically interested visitors, and Habnetic maintainers.

**Phase 3 evidence:** the outcomes used to fit the baseline are **synthetic**. The interface must never present building-level scores, inclusion probabilities, or maps as observed flood losses, certified hazard assessments, or validated predictions of real-world damage. Cross-city transfer is a fixed-specification structural experiment, not independent empirical validation in Hamburg or Donostia.

Always label, near the relevant results:

- **Outcome evidence:** synthetic.
- **Experiment:** Phase 3, with the appropriate reference/target city.
- **Interpretation:** model-derived decision results under stated assumptions.
- **Validation:** computation and diagnostics, distinguished from observational/empirical validation.

Use terminology precisely: *inclusion probability* is not *damage probability*; *input-support diagnostics* are not *MCMC convergence diagnostics*; *stable ranking* is not *correct prediction*. Display exact provenance and limitations rather than relying on a disclaimer buried in a footer.

## 3. Scope of the first release

### Required in v0.1

1. **Experiment index:** discover and open published Phase 3 records for Rotterdam (`RTM`), Hamburg (`HAM`), and Donostia (`DON`).
2. **Experiment inspector:** show source and target city, model/setup metadata, synthetic outcome status, baseline reference, publication version, and limitations.
3. **Decision workspace:** inspect the six available selection-capacity records per city, including `k`, borderline share/count, stable inclusion/exclusion, and ranking variability. Preserve the original metric definitions.
4. **Input-support view:** inspect the five available covariate-support records per city, with clear units and reference/target meaning where documented.
5. **Model summary view:** display available posterior parameter summaries for `alpha`, `beta_E`, and `beta_H`. Distinguish this from complete inference diagnostics; do not claim convergence from `summary.json` alone.
6. **Command console:** accept a small allowlisted set of read-only commands, show useful responses, and surface explicit errors for unsupported commands.
7. **Traceability:** each displayed result identifies the experiment, publication manifest, code reference and relevant source artifact; offer a link to the scientific documentation.
8. **Loading and error states:** handle missing, unpublished, invalid, or unreachable artifacts without fabricating values.

### Deferred (not v0.1 requirements)

- Executing Python, PyMC, notebooks, GitHub Actions, or arbitrary commands from the browser.
- Uploading private data, editing scientific results, or authenticating users.
- Real-time inference, live job monitoring, or a database/backend.
- Recomputing posterior draws, generating new optimizations, or refitting across cities.
- A full citywide geospatial map or high-volume Parquet/NetCDF browser. A map may be added after a suitable curated spatial export is available and verified; the first release must not depend on it.
- M0-versus-M1 comparisons until M1 results are published with comparable definitions.
- Additional languages/domains, notifications, and user-specific research workspaces.

## 4. Information architecture and interaction

**Primary route:** `https://habnetic.org/research-lab/` (subject to the website's existing routing conventions). The experience is integrated into the existing public site, not deployed as a separate application or repository.

**Core regions:**

- **Header/status bar:** Habnetic identity, active experiment, data-publication version, and static/read-only status.
- **Experiment navigator:** published entries, study areas, and selection controls.
- **Scientific workspace (approximately 60% width on desktop):** visualizations/tables of model and decision results, with units, definitions and limitations.
- **Inspector (right column):** context, evidence status, provenance, diagnostic category and links.
- **Console (lower right):** command entry, help, results, and local interface/activity messages.

On narrow screens, stack these regions; the scientific workspace must remain legible and keyboard-accessible.

### v0.1 console command contract

| Command | Behavior |
|---|---|
| `help` | Show supported commands and syntax. |
| `status` | Show current publication version, dataset availability and read-only state. |
| `experiments` | List available published experiments. |
| `inspect RTM` | Open the published Rotterdam experiment; likewise `HAM`, `DON`. |
| `decisions RTM` | Display selection-capacity metrics for Rotterdam. |
| `diagnostics HAM` | Show available diagnostics with explicit categories and caveats. |
| `compare RTM HAM` | Compare only like-for-like, actually published Phase 3 metrics and label fixed-model cross-city transfer; if not supported by the publication data, return a clear unavailable response. |
| `logs` | Show local UI events and recorded publication events, **not** simulated scientific jobs. |

Aliases and UX refinements are implementation choices. **Commands are parsed into predefined application actions**, never passed to a shell, server command executor, or arbitrary file reader. The graphical controls and console call the *same* application actions and operate on the same validated data.

## 5. Visual direction

Use a **restrained, instrument-oriented scientific interface** emphasizing analytical clarity, data provenance, and operational legibility. The visual identity must be original and consistent with Habnetic’s public research positioning.

- Slate and grey-blue surfaces, restrained instrument-style accents, clear separators and readable typography.
- Approximately 60/40 desktop workspace/inspector composition.
- Charts and information hierarchy first; decoration second.
- Subtle state indicators rather than flashing pseudo-telemetry.
- Sufficient contrast, visible focus states, keyboard operation and reduced-motion support.
- Genuine uncertainty displays with legends, units, reference groups and provenance.

A command console is a useful navigation and inspection surface, **not** evidence of a live operational control system.

## 6. Architecture: four independent layers

| Layer | v0.1 implementation | Responsibility |
|---|---|---|
| **Scientific** | Existing `Habnetic/resilient-housing-bayes` outputs | Inference, scoring, decision analysis, reproducibility. |
| **Infrastructure/publication** | Curated, versioned static artifacts hosted through existing GitHub infrastructure | Validate, publish, and serve reviewed results. |
| **Application** | Shared browser-side state/actions, manifest reader and safe command parser | Navigate experiments, transform curated records for display, handle errors. |
| **Presentation** | `Habnetic/habnetic.github.io`, published under `/research-lab/` | Responsive research workspace, inspector, console. |

The frontend must not import scientific Python code. The science code must not depend on the frontend. A future API may replace static hosting behind the same application-level data interface, without changing the artifact contract.

**No new repository, CMS, database, Cloudflare Worker, or dedicated server is required for v0.1.** GitHub Pages is sufficient for a curated static site; infrastructure changes require a demonstrated constraint.

## 7. Publication data contract and flow

The authoritative source for an experiment's portable metadata is the **Experiment Artifact Contract v0.1.0**. This application specification does not redefine its manifest schema.

```text
Scientific outputs (local; M0 frozen)
    |
    v
Read-only selection + validation + provenance review
    |
    v
Versioned public bundle
    |- experiment index (published experiment identifiers + manifest references)
    |- manifest.json per experiment (Artifact Contract v0.1.0)
    |- curated JSON summaries / derived display data as needed
    `- documented source links and explicit exclusions
    |
    v
Static ResearchProvider -> shared actions -> UI and console
```

**Public bundle location:** decide during implementation, using one existing repository/hosting arrangement. Do not assume that the local `outputs/phase3` directory or `Habnetic/data/runs` is already a published web registry. The website needs a stable public URL for an index and each manifest; exact paths are implementation details to record once verified.

**Publication rules:**

1. **Never alter historical M0 files.** Generate manifests and curated views separately.
2. Use relative, portable artifact references as specified by the contract; no Windows absolute paths, private identifiers, or credentials in published artifacts.
3. Retain a fixed, verified scientific source reference, including the M0 Git tag/commit and applicable data provenance.
4. Compute and verify SHA-256 checksums for bundled artifacts, with a publication review before the index lists them.
5. Export **small browser-consumable JSON** for tables/charts when necessary. Parquet and NetCDF are scientific sources, not obligatory browser download formats; do not upload huge files merely to populate the interface.
6. Preserve units, threshold meanings, city mappings, metric definitions, and selection `k` exactly. Never silently rename or reinterpret a statistic.
7. Treat missing or non-applicable diagnostics as `not_available`, not passing results. Publish scientific caveats adjacent to visualizations.
8. Each publication is immutable or versioned so that the same URL/version can be audited later; an index update can point to a newer reviewed publication.

### Available Phase 3 source artifacts (verified locally)

- `run_metadata.json`: configuration, feature variables, synthetic-data parameters and local paths; **requires sanitization for public display**.
- `summary.json`: parameter summaries for `alpha`, `beta_E`, `beta_H`.
- `decision_metrics.json`: **six records per city**, including `k`, borderline/stability counts and shares, and rank-standard-deviation summaries.
- `support_diagnostics.json`: **five records per city**, describing reference-versus-target covariate support.
- `asset_metrics.parquet`: building-level decision results; use only reviewed subsets/aggregates in initial browser exports.
- `prior_predictive_summary.json`: prior predictive information, where suitable for a curated summary.
- `idata.nc`: inference draws for scientific reproduction/analysis; not required for initial browser delivery.

These artifacts have been located and their structures inspected; this is **not** a claim that public bundles, manifest generation, or the site already exist. Existing top-k probability-sum checks are structural checks, not empirical model validation.

## 8. Evidence presentation and comparison constraints

For every plot, table or comparison, include the appropriate:

- Experiment ID, study area, publication version and source artifact.
- Metric name and definition, selection capacity `k`, and units if applicable.
- Reference/target status, synthetic evidence label, and model/scenario assumptions.
- Data completeness and validation status.

The first release may compare Phase 3 sites only where the data and metric definitions are meaningfully compatible. Differences in city size, selection capacities or feature-support distributions must remain visible. Do not describe a comparison as an M1 effect or evidence of cross-city predictive accuracy.

For later M0–M1 comparisons, require an explicit comparator linking the two experiment manifests and documenting the candidate universe, capacity, constraints, tie rules, changed terrain variable, and any changes to the statistical model. **Do not implement this comparator before genuine M1 outputs exist.**

## 9. Security, reliability, and accessibility

- Entire site is **read-only**; no science execution endpoints or shell interpretation.
- Permit only predefined command grammar and known experiment IDs; escape user-entered text and treat loaded strings as data, not HTML.
- Load from an approved static artifact origin; do not let a manifest redirect the app to arbitrary local paths or untrusted URLs.
- Validate publication integrity **before deployment**. Browser-side validation and graceful error handling supplement, rather than replace, the publication gate.
- Present explicit loading, empty, missing-data, schema-mismatch and network-error states.
- Ensure keyboard navigation, visible focus, chart alternatives (tables/text), responsive layout, contrast and reduced-motion behavior.
- Do not display fake real-time model activity, invented confidence intervals or synthetic log entries as authentic scientific operations.

## 10. Definition of done: Operations Center v0.1

The release is complete only when all of the following are demonstrated against **published artifacts**, not mock data:

- [ ] `/research-lab/` loads publicly from the existing Habnetic website and works on desktop and mobile.
- [ ] A curated index loads verified Phase 3 experiments for **RTM, HAM and DON**.
- [ ] Each experiment has a validated manifest complying with the Experiment Artifact Contract v0.1.0 and immutable/traceable scientific provenance.
- [ ] Existing Phase 3/M0 scientific outputs remain unchanged.
- [ ] Users can inspect actual posterior summaries, all six decision-metric records and five input-support records per city.
- [ ] Evidence is consistently marked **synthetic**, and model, support and empirical validation are clearly distinguished.
- [ ] Workspace controls and allowlisted console commands navigate the same data and return correct, testable results.
- [ ] Bad commands, unknown experiments, unavailable metrics and failed artifact fetches produce understandable errors.
- [ ] Public bundle contains no private absolute paths or credentials; checksums, references and publication review pass.
- [ ] Basic keyboard, mobile, accessibility and static-site smoke tests pass.
- [ ] A documented repeatable command/process explains how to validate and republish a bundle.

**This document defines release readiness; it is not a statement that the release is already built.**

## 11. Implementation sequence and non-blocking relationship to M1

1. **Document (now):** commit this specification in `Habnetic/docs`; no scientific code changes.
2. **When building v0.1:** select and document a public bundle location, export one curated Phase 3 experiment, validate it, then repeat for the other cities.
3. Implement the smallest possible static `ResearchProvider`, shared application actions, interface and command console.
4. Test with the authentic published data and release against the checklist above.
5. **Independently:** execute Phase 4 M1's controlled Rotterdam topographic experiment. M1 must not wait for Operations Center UI implementation.
6. Add M1 later as a new, reviewed experiment publication, not by overwriting M0.

### Assumptions and open decisions

- The website already uses GitHub-backed static hosting; exact routing and frontend framework must be checked before implementation.
- A public bundle location and manifest/index hosting path are **not yet selected**.
- Source data outputs are available locally; durable archival locations for all large scientific results should be verified before linking them publicly.
- Full spatial maps are postponed until a reviewable, appropriately licensed, browser-sized export exists.

**Next small step:** Commit this specification, close the architectural preparation gate, and begin the M1 scientific protocol/Rotterdam terrain-data inventory. Build the first public Phase 3 bundle when actual Operations Center implementation starts.
