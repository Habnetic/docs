# Roadmap 2026

## North star
Deliver a reproducible Bayesian workflow for resilience assessment that works on:
- one European benchmark (RTM, Phase 0)
- one US benchmark (SFO, Phase I)
- one extreme Earth analog (CON, Phase II)
- with a clear path to extraterrestrial synthetic habitats (LUN, MAR)

## Milestones

### Q1
- RTM thin-slice pipeline: hazard → exposure → fragility → downtime
- Data repo: scripts + provenance for at least 2 RTM datasets
- Prior predictive checks and first posterior predictive checks documented

### Q2
- Expand RTM: include socioeconomic exposure proxy
- Add SFO thin-slice (seismic focus)
- Model comparison baseline (LOO/WAIC) for at least 2 likelihood/prior variants

### Q3
- Introduce CON extreme Earth analog (isolation + thermal stress)
- Out-of-distribution stress testing across RTM → SFO → CON
- Documentation: “one-command” run instructions

### Q4
- Cross-site hierarchy (partial pooling across Earth-based sites)
- Sensitivity analysis and robustness report
- Public demo notebook + website narrative update
- Conceptual extension to LUN / MAR (synthetic, no calibration)

## Backlog (explicitly not now)
- High-fidelity hydraulic flood depth modeling
- Detailed building typologies and structural models
- Multi-agent occupant behavior
- Full lunar or Martian settlement simulations

## What to test next
Define one RTM success criterion that is falsifiable, e.g.:
Posterior predictive checks reproduce observed proportions of buildings within regulated flood-adjacent zones within tolerance.
