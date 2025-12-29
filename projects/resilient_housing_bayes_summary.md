# Resilient Housing Bayes Summary

## One-line summary
A Bayesian simulation framework for how housing systems respond to stochastic hazards, estimating impacts, downtime, and reconstruction cost under uncertainty.

## Core idea
Resilience is not a single score. It is a distribution over outcomes:
- probability of damage states
- probability distribution of downtime
- probability distribution of cost and displacement proxies

## Modeling backbone
Hazard → Vulnerability → Impact → Recovery

### Outputs of interest
- P(damage state | hazard intensity, building features)
- E[downtime] and distribution of downtime
- E[cost] and distribution of cost
- tail risk metrics (e.g., 95th percentile downtime)

## Site strategy
- **DSS:** coastal + pluvial flooding benchmark
- **SFO:** multi-hazard urban benchmark (seismic/surge/wildfire)
- **SYN:** synthetic / lunar analog for extreme stress-testing
- **CON (future):** extreme Earth analog for out-of-distribution validation

## Current status
- Synthetic data generation: planned / implemented (link to code repo)
- Real data ingestion: in progress (see Habnetic/data)
- Bayesian inference: planned / partial (PyMC + ArviZ)

## What to test next
Pick one site (DSS) and run an end-to-end “thin slice”:
hazard layer → exposure join → fragility → downtime posterior predictive check.
