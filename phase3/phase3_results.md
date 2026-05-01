# Phase 3 — Cross-City Transfer Results

## Figures

- ECDF of top-k membership probability (k = 1000, 5000)
- Low-probability zoom (0–0.3)
- High-probability zoom (0.7–1.0)
- Borderline share vs k

---

## Interpretation

The top-k membership probability distributions are highly polarised across all cities. 
For Rotterdam and Hamburg, the vast majority of assets exhibit probabilities close to 0 or 1, 
with borderline probabilities (0.2–0.8) remaining negligible across all k.

This indicates that the model produces highly confident prioritisation decisions, 
with uncertainty concentrated in a narrow region around the decision threshold.

Under transfer to Hamburg, this structure remains largely unchanged, 
suggesting that ranking behaviour is stable under moderate domain shift.

Donostia exhibits a different pattern: borderline shares are consistently higher than in Rotterdam and Hamburg, 
particularly for larger k values, but remain below 1% overall. The dominant effect is a shift in the distribution 
of inclusion probabilities rather than a diffusion of uncertainty.

This confirms that uncertainty is concentrated in a narrow region around the decision boundary, 
rather than being distributed across the asset set.

Overall, the results indicate that posterior decision instability remains localised, 
and that cross-city transfer primarily affects ranking calibration rather than 
the structure of decision confidence.

---

## Key Takeaways

- **Near-binary decisions** dominate in RTM and HAM  
- **Instability is localised**, not systemic  
- **Transfer does not break the model**, it preserves structure  
- **DON differs**, but through distribution shift, not uncertainty explosion  

---

## Evidence (from tables)

- Borderline share remains below ~0.05% for RTM/HAM across k  
- DON shows higher variation, but remains below ~1%  
- ECDF curves confirm strong concentration at 0 and 1  

---

## Interpretation Boundary (what this does NOT claim)

- This does **not** validate real-world risk accuracy  
- This does **not** prove hazard correctness  
- This is strictly about **decision stability under the given generative model**

---

## Decision stability summary table

The decision summary table confirms the ECDF interpretation. Rotterdam and Hamburg show extremely low borderline shares across all tested k values, indicating near-binary top-k membership probabilities and highly stable prioritisation decisions.

Donostia exhibits higher borderline shares than Rotterdam and Hamburg, particularly for larger absolute k values, but the borderline mass remains below 1% across the tested thresholds. This suggests moderate transfer stress and a shift in the distribution of inclusion probabilities, rather than a breakdown of decision stability.

The table also shows that median top-k probabilities are generally 0 for Rotterdam and Hamburg, while Donostia reaches a median of 1 at k = 5000. This reflects the much smaller asset population in Donostia and confirms that absolute k values should be interpreted alongside relative portfolio size.

Absolute k values are not directly comparable across cities due to large differences in asset counts.

---

## What to test next

- Sensitivity to prior specification (already partially addressed)  
- Sensitivity to hazard formulation (proxy vs latent hazard)  
- Stability under stronger domain shift (future cities)  