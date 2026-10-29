# Habnetic | Experiment Artifact Contract

**Version:** 0.1.0
**Date:** 29 October 2026
**Status:** Draft specification, not implemented
**Repository:** `Habnetic/docs`
**Path:** `phase4/Experiment_Artifact_Contract.md`

## 1. Purpose

Define a minimal, portable record for a reproducible Habnetic experiment.

The record connects scientific assumptions, code versions, datasets, diagnostics and decision results without introducing dependencies on the Habnetic Operations Center.

**Fundamental principle:**

The scientific pipeline produces evidence. The Operations Center consumes published, reviewed scientific artifacts.

This contract must remain applicable beyond urban flooding.

## 2. Scope

The contract applies to new Habnetic experiments, beginning with Phase 4 M1.

It must:

* Preserve the existing Phase 3/M0 reference unchanged.
* Reuse existing scientific output formats.
* Record reproducibility and provenance.
* Distinguish scientific validation from technical validation.
* Support publication through GitHub.
* Remain compatible with a future server-backed platform.

It does not require modifying the Bayesian models, creating repositories, implementing a database or developing the Operations Center.

The completed M0 reference is:

* Commit: `5e19254`
* Tag: `phase3-code-freeze-20261022`

Historical M0 artifacts must remain unchanged.

## 3. Experiment artifacts

An experiment may produce the following existing outputs:

| Artifact                        | Purpose                                      |
| ------------------------------- | -------------------------------------------- |
| `run_metadata.json`             | Experimental configuration and preprocessing |
| `summary.json`                  | Posterior parameter summaries                |
| `decision_metrics.json`         | Selection and decision-stability metrics     |
| `support_diagnostics.json`      | Input-distribution and support diagnostics   |
| `asset_metrics.parquet`         | Building-level decision results              |
| `idata.nc`                      | Full Bayesian inference data                 |
| `prior_predictive_summary.json` | Prior predictive summaries                   |

Not every experiment must produce every artifact.

Missing outputs must be identified explicitly rather than replaced with invented values.

## 4. Experiment manifest

Each experiment intended for publication must have a portable `manifest.json`.

The manifest identifies the experiment and references its scientific outputs.

The initial implementation should support:

**Identification:** schema version, experiment ID, phase, milestone, scientific domain and study area.

**Scientific classification:** inference or scenario-analysis method, origin of outcome evidence and comparison baseline.

**Provenance:** repository, Git commit, dataset versions, source references, units, random seeds and checksums.

**Reproducibility:** configuration, dependency environment and documented execution command.

**Artifacts:** relative paths, file formats and checksums for included outputs.

**Checks:** artifact integrity, computational diagnostics, predictive checks and decision comparisons.

**Limitations:** unresolved issues, assumptions and restrictions on interpretation.

### Illustrative M1 manifest

```json
{
  "schema_version": "0.1.0",
  "experiment_id": "phase4-m1-rtm-001",
  "phase": "phase4",
  "milestone": "M1",
  "domain": "urban-flood-risk",
  "study_area": "RTM",
  "status": "draft",
  "analysis_kind": null,
  "outcome_evidence": "synthetic",
  "code": {
    "repository": "Habnetic/resilient-housing-bayes",
    "commit": null
  },
  "baseline": {
    "experiment_id": "phase3-rtm-m0",
    "git_tag": "phase3-code-freeze-20261022",
    "git_commit_short": "5e19254"
  },
  "inputs": [],
  "configuration": {
    "path": null,
    "seeds": {},
    "environment_path": null
  },
  "reproduction": {
    "command": null
  },
  "artifacts": {},
  "checks": {
    "artifact_integrity": "not_available",
    "inference_diagnostics": "not_available",
    "prior_predictive": "not_available",
    "posterior_predictive": "not_available",
    "decision_comparison": "not_available"
  },
  "limitations": [
    "Phase 3 baseline outcomes are synthetic; empirical predictive validity is not established."
  ]
}
```

This is a draft template, not an executed experiment.

The unresolved values must be supplied from actual experiment records before publication.

## 5. Artifact portability

Artifact references must be relative to the manifest's directory.

They must not contain:

* Local Windows drive paths.
* Machine-specific absolute directories.
* Credentials or private information.
* References that escape the artifact directory.

Included files must have recorded SHA-256 checksums.

Large scientific artifacts may remain outside Git, provided their provenance and availability are documented.

## 6. Validation

Before an experiment is published, verify:

**Structural validity:** required metadata exists and conforms to the contract.

**Artifact integrity:** included files exist and their checksums match.

**Reproducibility:** code, dataset, environment, configuration and execution instructions are documented.

**Scientific reporting:** applicable computational and predictive diagnostics are recorded, with unavailable checks explicitly identified.

**Decision comparability:** comparisons preserve or explicitly document changes in candidate sets, constraints, selection capacity and tie handling.

**Publication review:** technical checks do not imply scientific or empirical validity.

An experiment can be technically reproducible while remaining scientifically exploratory.

## 7. Repository responsibilities

**`resilient-housing-bayes`**

Scientific computation, experiment outputs, future manifest exporter and validation tests.

**`data`**

Dataset provenance, source documentation, spatial metadata and potentially curated public experiment records.

**`docs`**

Experiment contract, methodology, scientific protocols and research decisions.

**`habnetic.github.io`**

Future Operations Center interface consuming published scientific artifacts.

The scientific code must remain independent of the website.

## 8. Compatibility with the Operations Center

The future Operations Center will retrieve experiment records through a common data-access interface.

Initially, published artifacts will be delivered through GitHub.

A future server API may provide the same logical records without requiring a redesign of the scientific experiment format.

The contract does not define graphical components, terminal commands, authentication or backend infrastructure.

## 9. Implementation policy

During M1:

1. Reuse existing output formats.
2. Introduce manifest generation only where needed.
3. Avoid modifying historical M0 artifacts.
4. Test manifest integrity independently.
5. Keep scientific analysis separate from publication.

The first implementation should be small, reproducible and independently testable.

## 10. Completion criterion

Pre-M1 preparation is complete when:

* The artifact contract is documented.
* Existing M0 artifacts remain unchanged.
* The expected M1 output location and provenance requirements are understood.
* No additional infrastructure is required before scientific development begins.

**What to test next:** Verify that a manifest can reference existing Phase 3 results without altering them. Then begin M1's Rotterdam terrain-data inventory and controlled topographic comparison.
