# Habnetic — Phase 4 Research Roadmap

**Version:** 1.2  
**Date:** 22 October 2026  
**Status:** Working research plan, subject to revision as evidence and conversations develop  
**Core principle:** Habnetic studies probabilistic inference, constrained selection and decision stability. **Urban flooding is the first experimental application, not the limit of the framework.**

## 1. Purpose and structure

Phase 4 has **two connected, but distinct, research parts**:

- **Part A — Depth and credibility:** develop the existing urban flood-risk application through milestones **M0–M4**, moving from a reproducible synthetic baseline toward more physically defensible inputs and independent evidence.
- **Part B — Transfer beyond flooding:** investigate whether the same uncertainty-to-decision methodology could be useful in a genuinely different problem. Two exploratory directions are **simulation-based building decisions** and **satellite / Earth-observation planning**. Neither is yet a defined pilot or promised collaboration.

The two parts answer different questions. Part A asks whether our results remain meaningful as the flood application becomes more realistic. Part B asks which parts of Habnetic can be reused when the candidates, uncertainties, objectives and constraints are different.

The intended general method is:

```text
Evidence and assumptions
        ↓
Probabilistic model or clearly labelled scenarios
        ↓
Uncertain outcomes / values for candidate decisions
        ↓
Selection under stated resource and feasibility constraints
        ↓
Decision stability, sensitivity, and interpretation
```

We must distinguish **reproducibility**, **predictive credibility**, **decision stability**, and **decision usefulness**. A stable choice is not automatically a correct choice.

---

## 2. Part A — Urban flood-risk research (M0–M4)

**Primary experimental city:** Rotterdam (RTM). Hamburg (HAM) and Donostia (DON) remain transfer sites where relevant. Keep the Phase 3 decision framework as the comparator, rather than changing several assumptions at once.

### M0 — Establish the reproducible baseline

**Status: completed as an operational Rotterdam baseline, 22 October 2026.**

M0 recovered and reproduced the Phase 3 processing, Bayesian inference and decision metrics without changing the scientific model.

**Verified results**

- Code reference: commit `5e19254`, tag `phase3-code-freeze-20261022`.
- Rotterdam dataset: **221,324 buildings**, 16 variables and **13,953 synthetic damage events**.
- Preprocessing reproduced with exactly matching values. The only observed schema difference was `Y_damage` stored as `int32` in the reference and `int64` in the reproduction.
- Four MCMC chains and 4,000 total posterior draws; reproduced posterior summaries were statistically consistent with the originals, with no reported divergences and satisfactory reported R-hat/ESS diagnostics.
- At **k = 2,213** (approximately 1%), the original and reproduced selections by posterior mean risk had **100% overlap**. Borderline counts were **30** and **28** respectively; two buildings changed stability category.
- The posterior-predictive **aggregate synthetic damage rate** was 0.063052, compared with an observed synthetic rate of 0.063043; the 95% predictive interval for the aggregate rate was approximately **[0.061670, 0.064385]**.
- The posterior-mean top-k ranking was overwhelmingly aligned with exposure alone: **99.82% overlap** at k = 2,213.

**Lessons:** the original hazard predictor varied little relative to exposure, its regression coefficient was weakly identified, and the ranking was largely exposure-driven. Phase 3 propagated posterior parameter uncertainty but did not explicitly propagate uncertainty in building-level hazard and exposure inputs. Reproducing this experiment does **not** validate real-world flood damage predictions.

**Record:** `Habnetic/docs/phase4/Habnetic_Phase4_M0_RTM_Baseline.md` (published). The reference results and detached reproducibility worktree are retained separately.

### M1 — Introduce topographic information

**Question:** How do priorities change when an interpretable terrain characteristic is introduced into the Rotterdam experiment?

- Identify available, trustworthy terrain data; document resolution, vertical reference, spatial coverage and provenance.
- Start with **one physically motivated variable**, possibly relative elevation or local depression characteristics, rather than treating absolute elevation as a complete flood model.
- Define explicitly whether the terrain variable is a scenario adjustment, a hazard proxy or part of a fitted probabilistic model. Do not claim empirical identification without appropriate observations.
- Compare the unchanged M0 baseline with M1: entries/exits from top-k, probabilities of selection, borderline cases, sensitivity to modelling assumptions and physical plausibility.

**Completion criterion:** a reproducible, interpretable comparison, including a negative result if terrain adds little. A changed ranking is **not by itself** evidence of better prediction.

### M2 — Improve flood-hazard representation

**Question:** How do decisions respond to more defensible flood-hazard evidence and a more explicit treatment of uncertainty?

Potential work includes replacing simplified proxies with better-grounded hazard inputs, clarifying the meaning of hazard (rainfall, inundation, event probability or scenario), and exploring latent or uncertain hazard representations where data support them.

Compare with M0/M1 under controlled assumptions. Check prior predictive plausibility, posterior predictive behaviour when observations are available, convergence diagnostics, sensitivity to priors and likelihoods, and how uncertainty reaches the decision boundary. Avoid building an entire hydraulic simulator unless a specific scientific need justifies it.

### M3 — Improve exposure, vulnerability and consequences

**Question:** Do more informative consequences or intervention objectives change the decisions that matter?

Distinguish **hazard → vulnerability → impact → recovery**. Investigate building use, relevant characteristics, potential damage or service disruption, and uncertainty caused by missing or imperfect measurements. Extend from a simple probability-of-damage ranking toward impact-aware choices only when the required quantities are meaningful and supported by evidence. Keep equal-capacity top-k comparisons distinct from decisions with unequal intervention costs.

### M4 — Evaluate against independent evidence

**Question:** Can the model and resulting decisions be evaluated beyond the synthetic outcomes used in Phase 3?

Explore available observations of flooding, inundation, damages or operational consequences; assess their definitions, quality, coverage and permissions. Compare predictive performance and decision outcomes with a realistic conventional baseline, guarding against spatial and temporal leakage. Calibration, predictive checks and sensitivity matter as much as top-k stability.

**URA discussions** may help identify an operational decision and relevant observational evidence. They do not imply that data access or a pilot has been agreed.

### Part A milestone overview

| Milestone | Main purpose | Current status |
|---|---|---|
| **M0** | Reproduce and audit Phase 3 | **Completed** |
| **M1** | Test a controlled topographic extension | **Next** |
| **M2** | Improve flood-hazard inputs and uncertainty | Planned |
| **M3** | Improve vulnerability, exposure and impact | Planned |
| **M4** | Test with independent observations and decisions | Planned |

Milestones are research gates, not promises about dates or positive findings.

---

## 3. Part B — Explore the transfer of the Habnetic method

**Aim:** determine whether Habnetic is useful outside the flood-risk experiment. This does not require committing now to a second full model, choosing a partner, or announcing a pilot.

The candidate domains below are **research leads**, not preselected implementations. The outcome of Part B may initially be a better-framed scientific problem rather than new software.

### B1 — Simulation-based building decisions

**Research direction:** explore whether the Habnetic decision framework can support the comparison of building design, development, investment or operational alternatives when their future performance is uncertain.

Building-performance simulations can inform choices across the property life cycle. Relevant uncertainties might include changing climate conditions (including extreme heat), occupancy, thermal comfort, energy demand, operating costs, technical performance and economic assumptions. These are **potential dimensions to investigate**, not a predetermined dataset, model or application.

The methodological question is whether outputs from simulations or explicitly defined future scenarios can be translated into **feasible choices under constraints**, and whether the preferred choices remain robust as assumptions change.

Possible decisions may involve building-level strategies, design variants, investment options or operating policies. **The precise candidates, objectives, available simulations and validation criteria remain open.** In particular, this track is not committed to an overheating-retrofit ranking or to the assumption that building-level decisions must take the form of top-k selection.

A first useful result could be a clear problem specification showing where existing simulation and optimisation tools already suffice, and where Habnetic's stability analysis would provide additional information. No external dataset, collaboration or pilot is assumed.

### B2 — Satellites and space / Earth-observation planning

**Research direction discussed previously:** applying Habnetic's methods to **satellite observation prioritisation and resource-constrained planning**. This is the more structurally different candidate domain, because it would test the decision method outside building and urban-risk applications.

The motivating question is deliberately broad:

> When observation opportunities are limited and their value or success is uncertain, how stable is the choice of what to observe?

Possible factors include observation targets, acquisition windows, cloud conditions, coverage, expected information value, operational availability and competing demands on satellite resources. The exact problem must be defined with someone familiar with the domain: real satellite tasking involves feasibility and scheduling constraints, not merely sorting an arbitrary list.

For Habnetic, the interesting transferable element is **uncertain evidence → constrained choices → robustness of the resulting priorities**. A small conceptual experiment might use Earth-observation opportunities as candidates, but no specific satellite, orbit, instrument, mission, dataset or optimisation formulation has been selected.

This direction is distinct from **using satellite imagery as an input to a flood-risk model**. The latter would support Part A; the former would test Habnetic as a decision framework in a different operational domain.

This is an exploratory research direction only. No institutional support, mission access or planned collaboration is assumed.

### B3 — How Part B proceeds

Keep both directions open while gathering information. As the two possible domains become better specified, assess whether either offers:

- A concrete decision under uncertainty, with identifiable alternatives and constraints.
- Credible simulation, forecast or observational inputs and a meaningful comparator.
- A plausible way to examine decision stability and, ideally, usefulness.
- A domain expert or external evidence able to challenge the results.

Only then decide whether to run a small transfer experiment, and which direction is most defensible. **Do not force both into Phase 4**, and do not insist on selecting one on an arbitrary deadline.

If the uncertainty is generated from scenarios rather than updated from observations, call the output a **scenario-based stability analysis**, not a Bayesian posterior. Generalisability means that the common decision logic remains useful without inventing a new method for each application.

---

## 4. Scientific standards shared by both parts

For any proposed probabilistic model, document the **generative story**, **observational likelihood**, **priors**, **posterior quantities of interest**, and **checks** (prior predictive, posterior predictive, MCMC diagnostics and sensitivity). When there is no likelihood informed by observations, explicitly label the exercise as scenario-based.

For any decision experiment, define the candidates, objectives, units, admissible constraints, deterministic comparator and policy for ties. Compare decisions at the same capacity and under the same feasibility rules. Track membership probabilities, entries/exits, stability near the selection boundary and sensitivity to assumptions; evaluate actual decision value when a defensible reference exists.

Keep the decision module independent of the application. More realistic data should be added because they address a specific failure mode or unresolved question, not because a more complicated model would look impressive.

### Reproducibility and quality

- Record input provenance, licences, units, seeds, hashes, dependency versions and Git revisions.
- Provide a short, runnable command and expected outputs for every experiment.
- Preserve M0 as an unchanged comparison; do not overwrite historical outputs.
- Check R-hat, ESS, divergences and trace behaviour for MCMC; compare alternatives with out-of-sample predictive methods (including LOO when appropriate).
- Test at least one alternative prior, likelihood, input assumption or scenario.
- Fix or document known implementation limitations such as tie handling and large-matrix memory consumption when they become relevant to correctness or scaling.

---

## 5. Next research steps and decision gates

Part B remains open until its candidate applications can be described as testable decision problems. External discussions may inform the framing, but prospective conversations and unconfirmed access arrangements are **not part of this public roadmap**.

**Immediate research sequence:**

1. Preserve the M0 report and reproducible baseline as fixed references.
2. Begin M1 with a terrain-data inventory and one falsifiable comparison against M0.
3. Clarify the building-simulation direction: candidate alternatives, uncertain outputs, constraints and an appropriate comparator.
4. Clarify the satellite observation-planning direction: feasible acquisition options, uncertainty in expected value and operational constraints.
5. Evaluate the two directions using evidence availability, testability, scientific contribution and independent challenge; undertake a small transfer experiment only if justified.

There is **no fixed Part B implementation schedule, resource allocation, satellite count or guaranteed collaboration** in this plan.

---

## 6. Files and decision records

- **Code and tests:** `resilient-housing-bayes` (current Phase 4 development branch: `phase4/m1-topography`).
- **Stable research documentation:** `Habnetic/docs/phase4/`.
- **Data, large model outputs and supporting materials:** local data storage / Drive, with provenance and access restrictions recorded.
- **KnowledgeLab / Notion:** index, learning notes, meetings and decision log.
- **Exploratory notebooks:** Colab when useful, with substantive work committed back to the code repository.

After each milestone or research decision, record **what we learned, which assumptions changed, what remains uncertain, and what to test next**. Keep exploratory correspondence and private meeting notes in access-controlled records, not in the public repository.

## What to test next

1. **Part A / M1:** whether one well-defined topographic characteristic makes a meaningful and defensible difference to Rotterdam's exposure-dominated decisions.
2. **Part B / buildings:** whether simulation outputs, competing building strategies and uncertainty create a decision problem where Habnetic could contribute something distinct.
3. **Part B / satellites:** whether uncertain observation value and real acquisition constraints offer a credible test of transfer beyond urban risk.

**Working distinction:** *Part A tests depth and credibility. Part B explores transfer and generality. Neither substitutes for the other.*

---

## Notes on evidence

The two Part B directions are **exploratory hypotheses**, not findings from a completed domain study. Detailed literature reviews, source provenance and external validation criteria should be added when a specific problem has been defined. Public citations should support technical claims without disclosing tentative contacts or private correspondence.
