# Bayesian Reasoning (Habnetic)

## Purpose
Provide the minimal Bayesian concepts needed to build and critique Habnetic models without turning this into a religion.

## Key definitions
- **Prior:** beliefs before data, encoded as probability distributions.
- **Likelihood:** data-generating model, how observations arise.
- **Posterior:** updated beliefs after seeing data.
- **Posterior predictive:** simulated outcomes implied by the posterior.

## Why Bayesian here
Resilience questions are inherently uncertain:
- rare hazards
- incomplete building data
- measurement error
- heterogeneity across sites

Bayesian modeling makes uncertainty explicit and propagates it through decisions.

## Generic model template
### Generative story
1) hazard intensity is drawn from a forcing model  
2) buildings have latent vulnerability parameters  
3) damage state is sampled given hazard + vulnerability  
4) downtime and cost are sampled given damage state

### Likelihood
Define observed variables (damage, downtime, costs) and how they’re measured.

### Priors
Choose weakly-informative priors that respect units and constraints.

### Quantities of interest
Tail risk, exceedance probabilities, and comparative scenario deltas.

## Checks (non-negotiable)
- prior predictive sanity check
- posterior predictive check
- convergence diagnostics (R-hat, ESS)
- sensitivity to alternative priors / likelihoods

## Failure modes
- priors that imply absurd buildings or hazards
- “calibrated” models without posterior predictive checks
- using point estimates where distributions are the whole point

## What to test next
Write a toy model with 2 damage states and verify:
1) prior predictive outputs are plausible  
2) posterior predictive matches synthetic truth under known parameters.
