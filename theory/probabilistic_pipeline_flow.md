# RTM Probabilistic Pipeline — Conceptual Flow

This diagram captures the structural logic of the RTM pipeline from generative assumptions to decision artifacts.

---

## 1. Generative Structure

```
Data → Exposure → Hazard → Outcome
                     ↓
             Posterior Inference
             (α, β_E, β_H)
```

- **Data → Exposure → Hazard → Outcome** defines the generative model.
- Posterior inference updates epistemic uncertainty in parameters.

---

## 2. Decision Extraction (City Scale)

```
Posterior draws
        ↓
Citywide scoring (N = 221,324)
        ↓
p_mean_city
topk_prob_city_k
decision stability metrics
```

- Posterior draws are applied to the full urban extent.
- Decision objects are derived from the posterior, not from point estimates.

---

## 3. Phase Separation

This architecture separates:

- **Phase 0** — Deterministic exposure backbone  
- **Phase 1** — Bayesian baseline and posterior → decision mapping  
- **Phase 2** — Structural uncertainty (hazard variability, misspecification, transfer)

---

Phase 1 validates inference and decision extraction under aligned generative structure.  
Phase 2 will test instability under structural perturbations.
