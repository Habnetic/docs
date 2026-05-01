# Phase 3 — Cross-city transfer results

## Goal

Evaluate stability of decision-relevant quantities under fixed-specification transfer across cities (RTM → HAM → DON).

Model structure, priors, and feature transformations are held constant. Only raw data changes.

---

## Exposure proxy behaviour

The exposure proxy (E_hat_v0) shows:

- coherent spatial gradients aligned with hydrography proximity
- comparable scale and variation across cities after reference scaling
- no visible artefacts or discontinuities introduced by transfer

This supports its use as a stable cross-city exposure representation.

---

## DON hydrography sensitivity

A Donostia-only refined water proximity variant was tested after visual inspection suggested local inconsistencies in the distance-to-water surface.

The refined variant added filtered OSM river/canal geometries and filtered OSM water polygon boundaries to the v3 baseline. Although this reduced mean distance-to-water, the resulting map introduced noisy inland artefacts and reduced spatial coherence.

For this reason, the v3 water proximity version is retained as the Phase 3 baseline. The refined DON variant is archived as a sensitivity experiment, not used as the main cross-city transfer input.

This supports a methodological point: adding more detailed local GIS features does not automatically improve the exposure proxy. For the transfer experiment, cross-city consistency and spatial coherence are prioritised over local feature density.

---

## Hazard proxy behaviour

The pluvial hazard proxy (H_pluvial_v1_mm):

- shows smooth spatial gradients consistent with precipitation patterns
- remains stable under transfer due to deterministic construction
- does not introduce local artefacts or discontinuities

---

## Decision metrics

Posterior-derived decision quantities show:

- strong spatial clustering of high-risk assets
- a narrow band of instability concentrated near the decision boundary
- low overall borderline share (assets with 0.2 < P(top-k) < 0.8)

This confirms that:

- ranking uncertainty is not uniformly distributed
- most assets are decisively inside or outside the top-k set
- instability is localized rather than systemic

---

## Interpretation

The key result is structural:

- uncertainty propagates through the model,
- but decision instability concentrates in a small subset of assets.

This supports the use of posterior-derived decision metrics (e.g. top-k probability) as meaningful objects for prioritisation.

---

## Limitations

- hazard is a deterministic proxy (no latent hazard yet)
- outcome is synthetic (no empirical calibration)
- exposure proxy is simplified (distance-based)

Results should be interpreted as methodological validation, not real-world risk estimation.

---

## Model validation (PPC)

Posterior predictive checks confirm that the model reproduces the observed synthetic outcome distribution across all cities.

This ensures that the decision metrics are derived from a coherent generative model, not from a mis-specified likelihood.
