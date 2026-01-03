# Resilience Metrics (Habnetic)

## Purpose
Define the outcome variables Habnetic models estimate, and how to compare scenarios without pretending a single index solves reality.

## Core outcomes
- **Damage state distribution:** P(ds | hazard, features)
- **Downtime distribution:** P(T_down | ds, context)
- **Reconstruction cost distribution:** P(C | ds, context)

## Useful summaries
- Expected values: E[T_down], E[C]
- Tail metrics: q95(T_down), q95(C)
- Exceedance: P(T_down > t), P(C > c)

## Resilience as a curve
Resilience is often better represented as a recovery curve R(t) ∈ [0,1]:
- 1 = full function
- 0 = no function

The **area under the loss-of-function curve** is a robust measure:
- “less lost service” = more resilient

## Comparisons
Prefer scenario deltas:
- ΔE[T_down] between retrofit vs baseline
- ΔP(T_down > 30 days)
- Δq95(C)

## Failure modes
- collapsing everything into one number too early
- mixing incomparable units (time vs money vs displacement) without explicit weighting
- ignoring tail risk (where the suffering lives)

## What to test next
Pick RTM and define:
- a baseline building typology
- a retrofit scenario
Then compare Δq95(downtime) under identical hazard forcing.
