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

Donostia exhibits a different pattern: while borderline shares increase slightly, 
they remain small, and the main effect is a shift in the distribution of probabilities 
rather than a diffusion of uncertainty.

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

- Borderline share remains below ~2–3% for RTM/HAM across k  
- DON shows higher variation, but still limited borderline mass  
- ECDF curves confirm concentration at 0 and 1  

---

## Interpretation Boundary (what this does NOT claim)

- This does **not** validate real-world risk accuracy  
- This does **not** prove hazard correctness  
- This is strictly about **decision stability under the given generative model**

---

## What to test next

- Sensitivity to prior specification (already partially addressed)  
- Sensitivity to hazard formulation (proxy vs latent hazard)  
- Stability under stronger domain shift (future cities)  