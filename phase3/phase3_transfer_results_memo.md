# Phase 3 — Cross-City Transfer Results Memo
## Resilient Housing Bayes

**Objective:**  
Test whether posterior decision instability remains concentrated near a narrow prioritisation boundary under fixed-spec cross-city transfer.

---

## 1. Context

Phases 1–2 established that:

- Posterior decision instability in Rotterdam is **not diffuse**
- Instability is concentrated near a **narrow prioritisation boundary**
- Hazard perturbation (Gaussian noise) does **not destroy this structure**

This eliminates a key alternative explanation:

> The narrow boundary is not an artefact of a deterministic hazard input.

Phase 3 tests a stronger hypothesis:

> Does the same decision structure persist when the **city itself changes**, under a fully fixed model specification?

---

## 2. Experimental Design

### 2.1 Cities

- **Reference:** RTM (Rotterdam)
- **Targets:** HAM (Hamburg), DON (Donostia)

### 2.2 Fixed elements (strict)

- Model form  
- Priors  
- Feature definitions  
- Hazard construction logic  
- Scaling rule (Rotterdam-anchored)  
- Posterior workflow  
- Decision metric definitions  

---

## 2.3 Model

### Generative structure

logit(p_i) = α + β_E E_hat_v0_i + β_H H_pluvial_v1_logrel_i  
Y_i ~ Bernoulli(p_i)

### Priors

α ~ Normal(0, 2.5)  
β_E ~ Normal(0, 2.5)  
β_H ~ Normal(0, 2.5)

### Predictors

- `E_hat_v0`: deterministic exposure proxy  
- `H_pluvial_v1_logrel`: hazard relative to RTM median (log-scale)  

### Outcome

- `Y_damage`: synthetic Bernoulli outcome  

---

## 3. Data Pipeline Status

For all cities:

- Phase 3 assets constructed  
- Rotterdam-anchored scaling applied  
- Exposure proxy derived  
- Hazard transformed to relative/log scale  
- Synthetic outcome generated  
- Bayesian inference executed (4 chains, NUTS)  

Outputs:

```
outputs/phase3/<CITY>/
├── phase3_features_scaled.parquet
├── idata.nc
├── summary.json
├── asset_metrics.parquet
├── decision_metrics.json
```

---

## 4. Posterior Parameter Behaviour

### 4.1 Exposure coefficient (β_E)

| City | Mean |
|------|------|
| RTM  | ~0.99 |
| HAM  | ~0.99 |
| DON  | ~1.04 |

**Interpretation:**

- Highly stable across cities  
- Exposure signal is structurally consistent  
- Feature construction transfers cleanly  

---

### 4.2 Hazard coefficient (β_H)

| City | Mean | Uncertainty |
|------|------|------------|
| RTM  | ~0.38 | High |
| HAM  | ~3.72 | Moderate |
| DON  | ~-0.74 | High |

**Interpretation:**

- Weakly identified across cities  
- Sensitive to low hazard variance and collinearity  
- Exposure dominates decision structure  

---

## 5. Decision Metrics

### Definition

For each posterior draw:

- rank assets by posterior risk  
- compute top-k membership  

For each asset:

- `topk_prob`: probability of being in top-k  
- `rank_std`: rank variability  

**Borderline definition:**

0.2 < p < 0.8

---

## 6. Results — Cross-City Comparison

### 6.1 Borderline share

RTM: 0.00% → 0.05%  
HAM: 0.00% → 0.03%  
DON: 0.08% → 0.89%

---

### 6.2 Stable structure

- Stable exclusion dominates (~97–99%)  
- Stable inclusion scales with k  
- Borderline region remains extremely small  

---

### 6.3 Rank variability

- RTM / HAM: high absolute variability (large N)  
- DON: lower absolute variability  

Interpretation:

- Variability exists globally  
- Decision instability is localised  

---

## 7. Interpretation

### 7.1 Core finding

The narrow-boundary instability structure persists under cross-city transfer.

---

### 7.2 Structural behaviour

- Posterior probabilities highly polarised  
- Most assets clearly included or excluded  
- Uncertainty concentrated near threshold  

---

### 7.3 Transfer effect

Transfer introduces:

- mild widening (DON)  
- small shifts in inclusion  

But does not produce:

- diffuse instability  
- system-wide uncertainty  

---

## 8. Hypothesis Evaluation

**Baseline hypothesis:** Supported  
**Competing hypothesis:** Not supported  

---

## 9. Limitations

- Synthetic outcome  
- Weak hazard variation  
- Minimal feature space  
- Scaling dependency  

---

## 10. What this does NOT claim

- Not a real flood model  
- Not calibrated to reality  
- Not operational  

This is a **methodological result**.

---

## 11. Implications

### Methodological

- Decision stability appears structural  
- Ranking robust under domain shift  

### Practical

- Prioritisation more stable than expected  
- Uncertainty manageable  

---

## 12. Next Steps

- Introduce latent (probabilistic) hazard layer
- Compare proxy vs latent hazard formulations
- Evaluate impact on decision stability structure

---

## 13. Conclusion

The narrow-boundary instability:

- is not Rotterdam-specific  
- survives cross-city transfer  
- remains localised  

Posterior decision instability is structurally concentrated.
