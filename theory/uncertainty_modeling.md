# Uncertainty Modeling (Habnetic)

## Purpose
List the main uncertainty sources in resilience modeling and how Habnetic represents them explicitly.

## Types of uncertainty
### Aleatoric (irreducible variability)
- hazard event variability
- damage variability at same intensity
- repair time randomness

### Epistemic (lack of knowledge)
- missing building attributes
- sparse observations of damage/downtime
- imperfect hazard layers

## Common representations
- measurement error models (observed ≠ true)
- hierarchical models (partial pooling across buildings / neighborhoods / sites)
- latent variables for unobserved features (e.g., construction quality)
- mixture models for heterogeneous populations

## Practical guidance
- scale predictors
- encode constraints (positive times, bounded probabilities)
- prefer simple likelihoods first; add complexity only if PPC fails

## Checks
- prior predictive: do we generate plausible cities?
- posterior predictive: do we reproduce observed damage and downtime patterns?
- sensitivity: alternate priors for key parameters

## Failure modes
- treating missingness as “just drop rows”
- deterministic preprocessing that hides uncertainty
- overfitting with too many covariates and not enough signal

## What to test next
Implement a missing-building-attribute model:
- observed: partial features
- latent: construction quality
and examine posterior predictive sensitivity.
