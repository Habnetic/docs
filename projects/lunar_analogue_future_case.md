# Lunar Analogue Future Case

## Purpose
Define a synthetic / extraplanetary benchmark case that stress-tests the Habnetic resilience framework beyond Earth urban assumptions.

## Why a lunar analog
Urban resilience models quietly assume:
- abundant resupply
- mild ambient temperatures
- breathable atmosphere
- repair crews and external services

A lunar habitat removes those assumptions and forces explicit modeling of:
- closed-loop life support dependency
- energy as survival constraint
- repair latency and redundancy topology
- extreme thermal cycling and radiation exposure

## Intended role in Habnetic
- **Not** a realism claim about lunar architecture.
- A **validation target** for the framework:
  - can the model represent failure propagation?
  - can it quantify downtime under isolation?
  - do priors behave under extreme forcing?

## Minimal model mapping
- **Hazard / forcing:** thermal cycle, radiation spikes, dust intrusion
- **Vulnerability:** subsystem fragility (power, thermal control, air, water)
- **Impact:** loss-of-function (partial/full), habitable volume reduction
- **Recovery:** repair times with limited spares + constrained labor
- **Cost:** mass, energy, operational penalty (proxy metrics)

## Data stance
Primary: synthetic / procedural fields (SYN site).  
Optional: seed terrain from public lunar DEM tiles.

## Failure modes
- “Everything is a hazard” and nothing is measurable
- Too many subsystem parameters too early
- Confusing narrative plausibility with statistical identifiability

## What to test next
Implement a 3-subsystem toy habitat (power–thermal–air) with cascading failures and compare:
1) independent failu
