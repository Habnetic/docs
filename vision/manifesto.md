# Manifesto

**Habnetic is an open research effort to make uncertainty explicit in how we design, evaluate, and reason about housing resilience.**

Built environments fail under stress not because designers lack intelligence, but because uncertainty is routinely hidden, simplified, or deferred. Hazards are rare, data is incomplete, systems interact in non-linear ways, and decisions are made anyway.

Habnetic exists to model that reality rather than deny it.

---

## What Habnetic stands for

### 1. Uncertainty is a feature, not a flaw
Resilience cannot be reduced to deterministic checklists or single scores.  
Habnetic treats resilience as a **distribution of outcomes**, not a point estimate.

We model:
- variability in hazards  
- heterogeneity in buildings  
- incomplete and noisy observations  
- uncertainty in recovery, cost, and downtime  

If uncertainty is not visible in the results, it has been misplaced.

---

### 2. Models must explain themselves
Every model in Habnetic is expected to answer:
- What assumptions does this make?
- What data does it rely on?
- What would falsify it?

Methods, priors, and diagnostics are documented alongside results.  
Reproducibility is not an afterthought; it is a minimum requirement.

---

### 3. Simplicity is earned, not assumed
Habnetic prefers the **simplest model that can fail meaningfully**.

Complexity is introduced only when:
- diagnostics show systematic failure
- simpler models cannot represent observed behavior
- added structure improves interpretability or decision value

Elegance is not measured by parameter count.

---

### 4. Data has provenance, context, and limits
Open data is not neutral by default.  
Habnetic treats data sources, licenses, spatial resolution, and missingness as first-class modeling concerns.

Derived results are only as credible as the assumptions that produced them.

---

### 5. The purpose is decision support
Habnetic does not aim for perfect prediction.

It aims to support:
- comparison between scenarios
- understanding of trade-offs
- identification of tail risks
- transparent reasoning under uncertainty

Precision without insight is not progress.

---

## What Habnetic is not

- a universal “resilience index”
- a black-box scoring engine
- a collection of disconnected notebooks
- a promise of certainty where none exists

Habnetic is deliberately slower, more explicit, and less reassuring than typical tools.

---

## Scope and ambition

Habnetic begins with urban housing systems under climate and environmental stress.  
It extends, cautiously, to extreme and synthetic environments as **stress tests for assumptions**, not as speculative design claims.

The framework is intended to grow through:
- open data ingestion
- transparent modeling choices
- incremental validation across sites and hazard types

---

## A working commitment

Every Habnetic model should be able to:
1. Generate synthetic data from its assumptions  
2. Fit itself to observed or simulated data  
3. Pass prior and posterior predictive checks  
4. Expose where it fails  

Anything less is a sketch, not a result.
