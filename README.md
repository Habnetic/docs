# 🧠 Habnetic Documentation

Central documentation and research notes for the **Habnetic Open Research Lab**.

---

## 📚 Purpose

This repository serves as the written backbone of Habnetic — a structured archive of theory, methodology, and development notes linking architectural thinking with probabilistic modeling and open-source computation.

It replaces private note-taking tools with a transparent, version-controlled documentation system.

The guiding principles, scope, and intent of the project are defined in  
[`vision/manifesto.md`](vision/manifesto.md).

---

## 📌 Current Project State (RTM)

Habnetic’s Rotterdam (RTM) pipeline currently consists of:

### Phase 0 — Deterministic Exposure Backbone (Frozen)
- Spatial normalization (boundary, buildings, hydrography)
- Stable `bldg_id` propagation
- Water exposure priors
- Deterministic latent exposure index `E_hat`
- Deterministic hazard scaffold (`H_pluvial_v0`)

### Phase 1 — Bayesian Baseline + Decision Stability (Closed)
- Synthetic outcome proxy
- Logistic Bayesian model
- Sensitivity analysis (baseline + beta structure)
- Posterior → decision extraction
- Citywide posterior scoring (221,324 buildings)
- Quantified ranking stability and instability region

Phase 2 (structural uncertainty and probabilistic hazard propagation) is planned but not yet implemented.

---

## 🧭 How to read this repository

This documentation is intentionally **incremental**.

Many sections begin as conceptual scaffolding or placeholders and are expanded only when they are actively used in models or decisions. This avoids freezing assumptions too early and keeps documentation aligned with working code.

If a document appears minimal, it likely reflects an area still under exploration or not yet exercised by the modeling pipeline.

---

## 🗂 Structure

```
docs/
│   LICENSE
│   README.md
│
├───projects/
│       lunar_analogue_future_case.md
│       resilient_housing_bayes_summary.md
│       target_sites.md
│
├───references/
│       bibliography.bib
│       datasets_list.md
│
├───theory/
│       bayesian_reasoning.md
│       resilience_metrics.md
│       uncertainty_modeling.md
│
└───vision/
        collaboration.md
        manifesto.md
        roadmap_2026.md
```

---

## 🔁 Relationship to Code

This repository documents structure and theory.

Executable models, inference workflows, and decision artifacts live in:

- [Resilient Housing Bayes](https://github.com/Habnetic/resilient-housing-bayes)
- [Habnetic Data](https://github.com/Habnetic/data)

The documentation reflects the state of those repositories but does not duplicate their code.

---

## ✍️ Conventions

- All text is written in Markdown (`.md`) for maximum compatibility.
- Use relative links between sections (e.g. `../projects/resilient_housing_bayes_summary.md`).
- Reference datasets by linking to the Habnetic Data Repository.
- Clearly distinguish deterministic components from probabilistic components.
- Explicitly label synthetic assumptions.

---

## 🌍 Related Repositories

- [Resilient Housing Bayes](https://github.com/Habnetic/resilient-housing-bayes)
- [Habnetic Data](https://github.com/Habnetic/data)
- [Habnetic Website](https://habnetic.org)

---

## License

Unless otherwise stated, the contents of this repository are licensed under the MIT License.

The Habnetic name and logo are not licensed for reuse or endorsement.

---

© 2026 Habnetic — Open Research for Resilient Futures
