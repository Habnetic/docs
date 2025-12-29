# Roadmap 2026

## North star
Deliver a reproducible Bayesian workflow for resilience assessment that works on:
- one European benchmark (DSS)
- one US benchmark (SFO)
- one extreme synthetic analog (SYN)

## Milestones
### Q1
- DSS thin-slice pipeline: hazard → exposure → fragility → downtime
- Data repo: scripts + provenance for at least 2 DSS datasets
- First posterior predictive checks documented

### Q2
- Expand DSS: include socioeconomic exposure proxy
- Add SFO thin-slice (seismic)
- Model comparison baseline (LOO/WAIC) for at least 2 likelihood/prior variants

### Q3
- Introduce SYN stress tests (out-of-distribution)
- Cascading failure toy model (3 subsystems)
- Documentation: “one-command” run instructions

### Q4
- Cross-site hierarchy (partial pooling within site class)
- Sensitivity analysis and robustness report
- Public demo notebook + website narrative update

## Backlog (explicitly not now)
- Concordia Station (CON) extreme Earth analog (Phase 3)
- high-fidelity building typologies
- multi-agent occupant behavior

## What to test next
Define one DSS “success criterion” that is falsifiable, e.g.:
PPC reproduces observed flood-zone exposure proportions within tolerance.
