# Habnetic | Phase 3 Publication Closure

Date: 2026-11-28

## Status

- Technical licensing investigation: CLOSED_WITH_LIMITATIONS
- Manuscript revision: v1.2
- Research dataset release: v1.0
- Historical computational implementation: UNCHANGED
- Historical numerical results: UNCHANGED
- Precipitation proxy interpretation: CORRECTED

## Publication register

| Resource | DOI | Licence |
|---|---|---|
| Preprint v1.2 | 10.5281/zenodo.24629543 | CC BY 4.0 |
| Preprint concept | 10.5281/zenodo.23110713 | Version-specific terms apply |
| Dataset v1.0 | 10.5281/zenodo.24629255 | ODbL 1.0 |
| Dataset concept | 10.5281/zenodo.24629254 | Version-specific terms apply |

## Research dataset scope

The release contains selected posterior decision-stability
summaries for 570,609 buildings.

| City | Buildings |
|---|---:|
| Rotterdam | 221324 |
| Hamburg | 341530 |
| Donostia-San Sebastian | 7755 |

The archive includes CSV and Parquet summaries, source
attribution, licensing information, a data dictionary,
manifest and checksums.

Complete model inputs, original geographic databases and
full MCMC traces are outside this release.

## Source attribution

- OpenStreetMap contributors / Geofabrik: ODbL 1.0.
- Rotterdam CBS administrative boundaries: CC BY 4.0.
- Rotterdam PDOK / Kadaster BRT TOP50NL: CC BY 4.0.
- Hamburg ALKIS administrative boundaries: DL-DE BY 2.0.
- Hamburg Basis-Gewaessernetz: DL-DE BY 2.0.
- Gipuzkoa Provincial Council / B5M: CC BY 4.0,
  subject to documented source-product qualifications.
- Copernicus ERA5-Land: CC BY 4.0.

Detailed attribution and modification acknowledgements are
included in the revised manuscript and dataset archive.

Historical source-edition and exact lineage limitations
remain documented. Closure does not constitute independent
legal certification or unrestricted redistribution approval
for all original geographic inputs.

## Scientific correction

The historical ERA5-Land implementation used accumulated
precipitation fields with accumulation periods of up to
24 hours, rather than independent one-hour totals.

The historical calculation and numerical results are
preserved. Manuscript v1.2 corrects the physical
interpretation and explicitly documents the limitation.

No new inference was performed for this revision.

Phase 4 M2 will evaluate deaccumulated precipitation
and alternative hazard representations.

## Repository traceability

- habnetic-papers: 0c85101
- data publication commit: 6b15458
- data main merge: f9757b4

## Publication boundary

This closure concerns the reviewed Phase 3 manuscript
and selected posterior-summary dataset.

Operations Center map services, additional geographic
layers, Phase 4 terrain inputs and other future releases
retain their separate attribution and release requirements.

The technical licensing investigation should not be
reopened without materially new evidence.

Public DOI resolution is a separate final publication
verification step.
