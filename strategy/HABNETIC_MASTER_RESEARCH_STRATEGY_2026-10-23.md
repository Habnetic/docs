# HABNETIC
## Master Research, Publication, Collaboration and Development Plan

**Version:** 1.0 — consolidated working strategy  
**Date:** 23 October 2026  
**Planning horizon:** Late 2026–2028, subject to evidence, time and opportunities  
**Status:** Proposed master plan, not an announcement of completed research, partnerships, products or funding  
**Project:** Habnetic, independent and open-source research  
**Prepared for:** Internal planning, research coordination and conversations with prospective collaborators

> **Central question:** When do uncertain probabilistic estimates provide a reliable basis for consequential decisions under constraints, and when do they produce decisions that are stable but wrong?
>
> **Strategic direction:** Maintain the published Phase 3 as a bounded first result. Complete the Phase 4 research and validation programme. Begin a controlled Phase 5 transfer benchmark (G0) when its protocol is ready. Pursue collaborators, real decision problems and economic opportunities **in parallel**, without depending on them for scientific progress or implying they already exist.

---

## 0. Executive decisions: what we have agreed

1. **Habnetic is not a flood-prediction product.** It studies uncertainty, quality of probabilistic evidence, constrained decisions, selection stability, sensitivity and, when outcomes are known, decision quality.
2. **Flooding remains the principal experimental domain for Phase 4 Part A**, not the definition of Habnetic. The original Phase 4 structure **M0–M4** is retained.
3. **M0 is completed and frozen.** M1 is the next domain-model research increment.
4. **Phase 4 Part B has two exploratory candidate domains:** decisions informed by building simulations and satellite/Earth-observation acquisition planning. Neither is a confirmed pilot.
5. **Phase 5 has three components:** G0, a controlled synthetic regional-transfer benchmark; G1, real-data regional-transfer validation if viable; and D, transfer of the *decision-evaluation method* to a second domain. Cross-disciplinary *statistical parameter sharing* is deferred.
6. **Hierarchical Bayes and partial pooling are established tools**, not Habnetic inventions. A possible contribution lies in experimentally assessing **beneficial versus harmful evidence borrowing**, misleading confidence, and downstream constrained decision loss, against serious existing baselines.
7. **The Phase 3 Zenodo preprint remains a standalone historical contribution.** Its corrected record must be verified, then left intact. Do not retroactively claim geographic posterior transfer or real-world loss validation.
8. **A preprint and a peer-reviewed journal article may be stages of the same manuscript.** We can seek a critical external review and conditionally submit Phase 3; design G0 from the outset for a strong preprint and journal submission. There is **no obligatory paper at the end of Phase 4**.
9. **Collaborations, funding and market discovery run concurrently**, with independent gates. A conversation is not a partnership, an expression of interest is not demand, and grant eligibility is not income.
10. **No automatic company, library or Phase 6.** Those are decisions taken after evidence of originality, reuse and/or demand.

### Research logic

```text
Evidence and model assumptions
          |
          v
Probabilistic estimates or transparently labelled scenarios
          |
          v
Explicit decision candidates, utilities/losses and constraints
          |
          v
Feasible action / selection / prioritisation
          |
          v
Decision stability + sensitivity + empirical validity (where testable)
          |
          v
Decision consequences, regret and reliability

Phase 5 extension:
Source-region evidence + scarce/imperfect local evidence
          |
          v
Statistical borrowing with explicit compatibility assumptions
          |
          v
Calibrated or misleading confidence in downstream decisions?
```

**Three non-interchangeable concepts:**

- **Internal coherence:** Does the probabilistic model behave consistently under its assumptions, computational diagnostics and observed training data?
- **Empirical validity:** Do predictions or decision outcomes agree with independent evidence?
- **Decision reliability:** Do the proposed actions perform acceptably across defensible uncertainty and consequence assumptions?

A highly stable selection can fail the second and third tests. A posterior cannot certify its own priors, and a model that uses synthetic labels cannot independently establish real-world flood performance.

---

## 1. Where we stand: completed Phase 3 and frozen M0

### 1.1 Phase 3 — completed research, published preprint

**Working manuscript title:** *Posterior-Based Decision Stability under Cross-City Stress Testing in Urban Flood Risk Prioritisation*.  
**Zenodo record:** https://doi.org/10.5281/zenodo.23110714  
**Editorial status:** Public preprint; not peer reviewed. The corrected version / record replacement must be verified before changing linked resources.

Phase 3 studied the propagation of uncertainty from a simplified Bayesian risk model into fixed-capacity urban-asset selections. It used Rotterdam, Hamburg and Donostia–San Sebastián with the **same model specification fitted independently for each city**. Its cross-city test examined structural stability, **not posterior transfer or hierarchical information borrowing**.

**Valid, bounded claims:** posterior-based inclusion probabilities and selection-boundary behaviour were analysed under the specified model; outputs and code form a first reproducible experiment.

**Not established:** independently validated flood probabilities, observed building-level losses, general cross-region knowledge transfer, and the correctness or optimality of real interventions.

**Publication handling:** preserve the historical version and scientific scope. Finalise the corrected manuscript, record what changed, follow Zenodo's version/correction policy, and verify DOI/links. Do not merge M1 or Phase 5 hypotheses into this paper.

### 1.2 Phase 4 M0 — completed, do not reopen casually

The authoritative Phase 4 roadmap version 1.2 (22 October 2026) records the reproduced Rotterdam baseline:

- **221,324 buildings**, **16 variables**, **13,953 synthetic damage outcomes**.
- Code freeze: commit `5e19254`; tag `phase3-code-freeze-20261022`.
- Four MCMC chains and 4,000 total posterior draws, with satisfactory *reported* convergence diagnostics and no reported divergences.
- At fixed capacity `k = 2,213` (about 1%), reproduced posterior-mean selection had **100% overlap** with the reference; uncertain-boundary counts were **30 versus 28**, reflecting minor draw variability.
- Posterior-predictive aggregate synthetic damage rate **0.063052**, close to the synthetic observed rate **0.063043**; reported aggregate 95% predictive interval approximately **[0.061670, 0.064385]**.
- Posterior-mean top-k versus exposure-only selection overlapped by **99.82%**.

**Scientific lesson:** the original ranking was dominated by exposure; the hazard coefficient was weakly identified; parameter uncertainty was propagated but building-level hazard/exposure input uncertainty was not. Reproducing a synthetic model does not validate its real-world hazard or consequences.

**Change rule:** leave M0 frozen unless a reproducible correctness defect is found; any necessary correction gets a separate documented change record.

---

## 2. Phase 4 — deepen the flood experiment and define a second domain

**Purpose:** improve the scientific credibility of the existing experimental machinery, while testing whether the underlying decision-analysis method can be formulated beyond flooding.

**Structure:** Part A = M0–M4; Part B = B1/B2 exploratory studies. These tracks can progress at different speeds.

### 2.1 Part A — M1: one topographic extension **[NEXT]**

**Question:** Does a physically interpretable terrain feature change model-derived priorities in an informative and defensible way?

**Plan**

1. Inventory suitable DEM/DTM sources for Rotterdam: provenance, licence, spatial extent, horizontal and vertical reference systems, spatial resolution, missingness and processing assumptions.
2. Choose **one** defensible feature, such as relative elevation, topographic depression or another clearly motivated local indicator. Avoid presenting absolute height as an adequate inundation model.
3. Declare whether it is a proxy, scenario input or fitted covariate. Keep every other M0 assumption fixed for the primary comparison.
4. Refit or recompute as appropriate; inspect prior predictive plausibility, posterior behaviour and sampling diagnostics whenever inference is involved.
5. Compare M0–M1 under the same candidate population and capacities: selected entries/exits, inclusion probabilities, borderline counts, exposure-driven baseline, sensitivity and spatial interpretation.

**Deliverable:** a reproducible experiment note with figures, source/processing manifest, exact run command, results and limitations.

**Pass gate:** the experiment isolates and explains the incremental effect, including a defensible null finding. **A changed ranking is not evidence of better prediction.**

### 2.2 Part A — M2: better-grounded hazard and uncertainty

**Question:** What happens to decisions when hazard inputs and uncertainty are specified more credibly?

**Plan**

- Define what hazard represents: rainfall proxy, flood probability over a stated time horizon, depth, scenario footprint or other quantity. Keep units and reference periods explicit.
- Evaluate improved inputs or latent hazard only when identifiable evidence exists. Do not build a hydraulic simulator by default.
- Distinguish parameter uncertainty, uncertainty in spatial inputs/measurement, scenario uncertainty and residual outcome variation.
- Propagate uncertainty through to the decision, rather than stopping at point hazard scores.
- Compare controlled M0/M1/M2 alternatives and their computational, predictive and decision diagnostics.
- Where appropriate, use prior predictive checks, posterior predictive checks, R-hat, ESS, trace plots, divergent transitions, alternate prior/likelihood and out-of-sample evaluation. Avoid naïve LOO claims when spatial/event dependence dominates.

**Pass gate:** better-motivated assumptions and explicit limits. Complexity without identifiable benefit fails.

### 2.3 Part A — M3: exposure, vulnerability, impact and recovery

**Question:** Do decisions change materially when consequences and constraints are represented rather than only probability rankings?

**Plan**

- Separate **hazard → vulnerability → impact → recovery**; state which links are modelled, observed or still missing.
- Evaluate usable building/use characteristics, consequence indicators, missingness and measurement error.
- Define actual *decision alternatives*: investigate, inspect, intervene, defer, and/or allocate resources, as supported by the selected use case.
- Preserve the simple **fixed-k** comparator but investigate unequal costs, feasibility, budgets and expected consequences where defensible.
- Specify the utility/loss function before estimating regret; do not treat higher modelled risk as automatically higher decision value.

**Pass gate:** a transparent comparison between risk-ranked and consequence-aware actions with quantitative assumptions, sensitivities and limits.

### 2.4 Part A — M4: independent evidence and validation

**Question:** Which conclusions survive comparison with external observations or recorded outcomes?

**Plan**

- Identify suitable observed flooding, inundation, impact or loss records. URA conversations may help, but no access is assumed.
- Audit definitions, event/date alignment, spatial coverage, reporting bias, licence, matching and measurement uncertainty.
- Define held-out spatial, temporal or event partitions **before** fitting, tuning or preprocessing. Avoid leakage from scaling, imputation or feature generation.
- Evaluate predictive calibration/proper scores where targets are truly observed; compare decisions and a realistic operational baseline under a matched problem definition.
- Separate validation of prediction from validation of allocation outcomes. Observational data might support one but not the other.

**Pass gate:** documented independent empirical evaluation, an informative negative result, or a well-supported report that the available evidence cannot answer the question. No invented validation.

### 2.5 Part A completion criteria

- M0 reference frozen and cited.
- M1–M3 experiments fully reproducible, with scientific limitations retained.
- M4's observational evidence and failure conditions recorded transparently.
- A complete input → posterior/scenario → constrained decision → report trail available.
- Explicit final statement identifying which conclusions have, and have not, received real-world validation.

**Publication rule:** Phase 4 completion **does not automatically generate a new preprint or journal submission**. If M4 reveals an independently meaningful empirical or methodological result, evaluate it as a possible manuscript; otherwise publish transparent technical reports and code.

### 2.6 Part B — B1: decisions informed by building simulations

**Research question:** Can uncertainty-aware decision evaluation help select building design, retrofit, operational or investment alternatives from uncertain simulation results, above what existing workflows already provide?

Potential inputs include energy demand, extreme-heat performance, comfort, occupancy, operating costs or financial scenarios, **only if a precise decision problem supports them**.

**First-stage tasks**

1. Identify an actual decision owner, alternatives, constraints and consequences.
2. Clarify whether inputs are probabilistic posteriors, predictive distributions or merely scenario ensembles; **do not call scenario weights posterior probabilities without inference**.
3. Document established simulation, optimisation and sensitivity techniques already in use.
4. Define one falsifiable comparison. Do **not** impose fixed-k selection if the decision is a single alternative or a sequence of investments.
5. Decide: small experiment, postpone or reject.

**External lead:** conversation planned with **Prof. Dr. Andrea González (ZHAW), 29 October 2026**. This is discovery, not a partnership. Aim to identify complementary work on the reliability of simulation-supported choices and a realistic validation route.

**Pass gate:** written decision-problem card plus baseline and accessible data/simulator. A useful negative conclusion is that existing tools already suffice.

### 2.7 Part B — B2: satellite / Earth-observation observation planning

**Research question:** Can uncertainty-aware decision analysis add value to resource-constrained decisions about **which observations to acquire, where and when**?

- Define candidates, information value, opportunity costs and actual constraints (coverage, timing, clouds, acquisition capacity or budget).
- Keep this distinct from *using satellite imagery as flood input*.
- Determine whether available public simulators or synthetic scenarios make a controlled experiment possible.
- Compare with domain-specific planning, optimisation and value-of-information methods.
- Do not assume an ESA/NASA partner, operational satellite access or a confirmed application.

**Pass gate:** domain-reviewed problem definition and feasible controlled experiment. Otherwise defer.

### 2.8 Phase 4 outputs

- M1–M4 experimental notes, reproducible runs and validation report where feasible.
- A bounded technical description of Habnetic's general method.
- At least **one** credible second-domain decision specification, if discovered; no requirement to force both B1 and B2 into implementation.
- A Phase 5 G0 preregistration/protocol, developed separately and possibly in parallel.

---

## 3. Phase 5 — probabilistic knowledge transfer and decision reliability

**Status:** proposed research programme, *not yet demonstrated*.  
**Main question:** Under what source–target conditions does borrowing external probabilistic evidence improve decisions with sparse local data, and when does it create stable but incorrect actions?

### 3.1 Four distinct meanings of transfer

| Meaning | Description | Commitment |
|---|---|---|
| **Predictive transport** | Apply relationships learned elsewhere to a target setting | Study and audit where appropriate |
| **Geographical statistical borrowing** | Estimate related regions jointly or hierarchically, possibly with target observations | **Core G0/G1 subject** |
| **Decision-method portability** | Reuse the evaluation protocol across unrelated problems without shared physical parameters | **D experiment** |
| **Cross-domain parameter sharing** | Fit shared latent statistical structure across different physical/economic disciplines | **Deferred speculative research** |

A published forecast is not automatically a Bayesian prior. Its role depends on its generative and measurement origin, potential dependence on local evidence, documented uncertainty and a statistically valid assimilation model. Double-use of training data must be avoided or explicitly modelled.

### 3.2 Hypotheses, phrased to permit failure

**H1 — useful borrowing:** Under adequate exchangeability and compatible data definitions, hierarchical partial pooling can improve predictive and/or decision performance in low-data targets relative to local-only and global alternatives.

**H2 — negative transfer:** Under source–target incompatibility, borrowing can worsen calibration and realised decisions, sometimes **while posterior selections become more stable or concentrated**.

**H3 — auditability:** Suitable diagnostics of distribution shift, source–target conflict, sensitivity, uncertainty and decision regret may help distinguish useful from harmful borrowing; their actual detection performance must be measured against existing methods.

**H4 — method portability:** A clearly separated inference-to-decision evaluation protocol can yield meaningful, domain-correct insight in a second application, without assuming the same predictor or loss applies everywhere.

*Novelty is unproven.* Hierarchical Bayes, transfer learning, negative transfer, posterior predictive checks, stochastic multicriteria decision analysis and robust decision-making already exist. We must establish a gap through targeted literature comparison, baselines and defensible new findings, not adjectives.

### 3.3 G0 — controlled generative transfer benchmark **[FIRST PHASE 5 EXPERIMENT]**

**Purpose:** Use known data-generating truth to assess when regional evidence borrowing helps or harms predictive and downstream decision performance.

#### Generative story

Create synthetic regions containing candidates (e.g., spatial assets) with measured covariates, region-varying outcome relations, measurement imperfections and explicit decision consequences. Vary controlled regimes:

1. **Compatible regions:** approximately exchangeable effects; plausible partial pooling.
2. **Partially heterogeneous regions:** some shared mechanisms and meaningful differences.
3. **Incompatible or shifted target:** source–target coefficient, baseline, covariate-support or outcome-mechanism changes that can generate negative transfer.

Include independent simulation replicates and held-out target regions; recognise the difference between many buildings and **few statistically independent regions**.

#### Likelihood and candidate models

A minimal illustrative binary-event story, to be replaced if another target is better justified:

```text
Observed target:        y[i,j] ~ Bernoulli(p[i,j])
Candidate mean model:   logit(p[i,j]) = alpha[j] + x[i,j]' beta[j]
Hierarchy (H):         (alpha[j], beta[j]) ~ group distribution(mu, Sigma)
```

For a first implementation prefer a varying-intercept hierarchy to a high-dimensional varying-slope/covariance structure. Standardise covariates, specify units and prior predictive plausibility. Include observation error or missingness only when necessary to test a declared hypothesis.

**Comparison arms**

- **L — Local / no pooling:** estimate a model using only target training labels, when such labels exist.
- **P — Global / complete pooling:** fit a pooled model that ignores relevant region effects.
- **H — Hierarchical / partial pooling:** estimate group variation and propagate uncertainty to the target.
- **R — Robust/adaptive borrowing:** a justified established method that discounts incompatible sources, added only when G0's basic comparison is reproducible.

**Important:** L is not a meaningful predictive baseline for a **zero-observation** target. Compare no-target-label methods in that regime; evaluate L in few-shot regimes.

#### Experimental dimensions

- Local target sample size: 0, small, moderate.
- Source sample size and number of independent source regions.
- Relative measurement quality and missingness.
- Heterogeneity and explicit source–target shift.
- Decision capacity `k` or budget and feasibility assumptions.
- Prior breadth and at least one alternative likelihood or robust group distribution.

Start with a small factorial design, **changing one driver at a time**. Predeclare seeds, replication counts, sensitivity scenarios and primary metrics before looking at results.

#### Predictions, decisions and truth

Evaluate two separate layers:

**Predictive / inferential evaluation:** parameter or predictive coverage where truth is known, proper scores, calibration, posterior concentration, sensitivity, prior predictive plausibility and posterior predictive adequacy.

**Decision evaluation:** top-k or other feasible-set selection, inclusion stability, alternative-set overlap, realised loss, missed opportunities, regret relative to an oracle or precisely defined comparator, and frequency of stable-but-poor decisions.

Define the **loss/utility** function before interpreting regret. A ranking based on probability alone is not necessarily economically or socially optimal.

#### Holdout design and checks

- Use outer held-out **regions**, not random buildings alone, for geographic transfer claims.
- In few-shot target settings, divide target labels into explicitly limited *adaptation* and untouched *evaluation* sets.
- Put normalisation, imputation, prior calibration and tuning inside training partitions to prevent leakage.
- Check prior predictive, posterior predictive, sampling diagnostics (R-hat, ESS, divergences, traces), alternate priors and the role of the loss function.
- Use PSIS-LOO or WAIC only where its predictive unit and dependence assumptions suit the question. Pointwise LOO does **not** replace held-out-region testing.
- Examine source–target conflict and explain failures, not just average success.

**Pass gate:** a reproducible, pre-specified benchmark that distinguishes useful from harmful borrowing or reports an informative null/identifiability limitation; comparison with serious prior art. A new diagnostic is optional, not required.

**Stop/revise gate:** failures are artefacts of leakage, circular truth definitions, arbitrary thresholds, underidentified group variance or an already-solved benchmark; simplify, document or abandon before scaling up.

### 3.4 G1 — empirical geographic transfer **[CONDITIONAL]**

**Purpose:** Test whether G0 findings survive real regional differences, measurement imperfections and independent observational outcomes.

**Minimum entry conditions**

- Multiple meaningful and sufficiently independent regions, not merely a vast building count in three cities.
- Real observed target outcomes with compatible definitions and timescales, sufficient coverage and provenance.
- Relevant comparable covariates, observed region descriptors, units, permissions, missingness and uncertainty documentation.
- A feasible held-out-region/event/time evaluation protocol and an outcome or consequence basis for testing decisions.

**Method**

- Compare local, global, hierarchical and robust borrowing where each is applicable.
- Evaluate cold-start and few-shot regimes separately.
- Use held-out-region/time performance, proper scores, calibration, sensitivity and realised decision consequences.
- Show region-wise failures and selection biases, not only mean performance.
- Distinguish nested, crossed and overlapping geographic structures (city, basin, region, country); do not invent a convenient nested hierarchy unsupported by the data.

**Pass gate:** genuine predictive and/or decision findings that survive defensible independent evidence.

**No-go:** insufficient comparability, too few independent regions or no observed outcomes. A synthetic hazard proxy is not a substitute for external empirical validation.

**Dependency:** G1 may begin before the formal completion of M4 **if a separate adequate real dataset and independent validation design exist**. It is not automatically blocked by the Phase 4 flood workflow.

### 3.5 D — transfer the decision-audit method across disciplines **[CONDITIONAL]**

**Purpose:** Demonstrate useful portability of the evaluation protocol, not shared physical parameters.

**Candidate 1:** decisions informed by building simulations (from B1).  
**Candidate 2:** satellite-observation planning (from B2).

Required minimum:

1. Realistic alternatives and constraints.
2. Clearly identified probabilistic predictive samples or explicitly scenario-only outputs.
3. A credible domain-specific baseline.
4. Well-defined decisions and losses or appropriately labelled proxy metrics.
5. A replicable experiment showing what transfers, what fails and why.

If a domain cannot support a Bayesian posterior, do not invent one; evaluate appropriately weighted scenarios or decline probabilistic claims.

**Pass gate:** at least one meaningful independent second-domain test. A common function signature is not scientific generalisation.

### 3.6 S — cross-disciplinary shared statistical learning **[DEFERRED]**

Do **not** transfer regression coefficients between floods, energy models or satellites simply because all involve uncertainty. Shared latent parameters require a defensible generative mechanism and compatible observations. No implementation schedule until simpler transfer and portability hypotheses have been tested.

### 3.7 Scientific failure modes to monitor

- **Negative transfer:** incompatible source information introduces bias while reducing apparent uncertainty.
- **Pseudoreplication:** huge numbers of assets do not replace independent regions/events.
- **Double-use of evidence:** source-derived priors or calibrations reuse evaluation information.
- **Target leakage:** target effects or preprocessing learn from held-out labels.
- **Synthetic circularity:** benchmark outcomes are generated by the same assumptions used to claim real-world validity.
- **Unidentified utility:** probability of an event is substituted for consequence or optimal action.
- **Instability of labels:** data definitions and reporting vary between regions.
- **Overengineering:** elaborate latent structures conceal weak identification.
- **Redundant methods:** existing stochastic decision or transfer tools already solve the posed problem.

---

## 4. Publication strategy — **preprint versus peer-reviewed journal**

**Key distinction:** A *preprint* is a publicly shared manuscript, usually without journal peer review. A *peer-reviewed article* is a manuscript evaluated and accepted by a journal. They can be **the same research work** at different stages; a preprint DOI is not a journal acceptance.

### 4.1 Phase 3: what to do with the current preprint

**Decision:** preserve its scientific boundaries; close and verify the corrected Zenodo record. **Do not rewrite Phase 3 to tell the Phase 5 story.**

Then:

1. Seek at least one **independent methodological critique**, ideally from someone familiar with Bayesian inference and/or decision analysis. Ask them to inspect novelty, posterior inclusion interpretation, baseline comparisons, synthetic-target limitations and reproducibility.
2. Prepare a concise **journal suitability/novelty assessment** based on the review and actual journal aims. An application/methodological short article may fit better than a claim of general new theory.
3. **Submit to a suitable peer-reviewed journal if the contribution withstands critique.** Do not delay M1/G0 to rescue a weak article; if methodological gaps are fundamental, leave it as a useful preprint and documented baseline.
4. If submitted, disclose the preprint and follow the specific journal's preprint, versioning, anonymisation and publication policies.

**Current status:** Zenodo preprint public. External methodological review and journal submission are **proposed, not completed**.

### 4.2 Phase 4: what and when to publish

- Public milestone reports, data dictionaries, code, reproducibility notes and observed limitations are useful research outputs.
- There is **no mandatory Phase 4 paper** and no automatic preprint on completion of M4.
- If M4 yields a scientifically distinct observed validation or important failure finding with adequate baselines, consider a new standalone paper rather than forcing it into Phase 3.
- Share negative results where useful and interpretable.

### 4.3 G0: the next deliberately targeted manuscript

**Working title:** *When Bayesian Borrowing Misleads: Decision Reliability under Regional Data Scarcity*. This is a candidate title, not a novelty claim.

**Design toward journal standards from the beginning**, not after obtaining pleasing results:

- literature gap and explicit distinction from existing hierarchical-transfer, dataset-shift and decision-focused methods;
- predeclared testable hypotheses and intervention regimes;
- local, global, hierarchical and robust comparators as appropriate;
- held-out-region evaluation, controlled negative transfer and reproducible ground truth;
- both predictive performance and **downstream decision regret**;
- uncertainty over replicate/regional performance and sensitivity to assumptions;
- clear account of generalisability beyond the synthetic DGP.

**Preprint milestone:** only after experiments and claims are sufficiently complete, audited and internally reproducible. Then publish to Zenodo or another eligible preprint repository.  
**Journal milestone:** submit the **same scientific manuscript, appropriately adapted**, after independent critique, either soon after the preprint or once necessary empirical strengthening is complete.

**Important contingency:** if G0 is scientifically informative but a target journal requires stronger empirical evidence, combine or extend with a bounded G1 validation where feasible. Do not delay public disclosure forever waiting for perfect data.

### 4.4 Conditional future manuscripts

| Candidate | Required contribution | Dependency |
|---|---|---|
| **Phase 3 journal article** | Peer-review-ready version of already published preprint | Favourable external critique and journal fit |
| **G0 methodological manuscript** | Evidence on beneficial/harmful Bayesian borrowing **for decisions**, beyond obvious textbook behaviour | Reproducible G0 and novelty comparison |
| **G1 empirical manuscript** | Real cross-region external validation and decision consequences | Adequate independently observed outcomes |
| **Cross-domain D manuscript** | Substantive, domain-appropriate value of a shared evaluation protocol | Second-domain experiment and baseline |
| **M4-specific paper (optional)** | Distinct empirical flood-validation finding | M4 evidence supports novelty |

These are **possible manuscripts, not publication quotas**. Merging closely related work may be better than splitting it artificially.

### 4.5 Journal targeting: later, based on actual contribution

Possible *scope matches for investigation*, not confirmed recommendations or guarantees: **Decision Analysis**, **Risk Analysis**, **Environmental Modelling & Software**, and **Environmental Data Science**. Evaluate current editorial policies, scope, article types, preprint acceptance, open-access fees, expected contribution and literature fit **before submission**. A methods-focused G0 and an empirical G1 may belong in different journals.

**Editorial principle:** peer review is an external quality filter, not a certificate of truth. Editorial rejection is information to examine, not an automatic reason to add model complexity.

### 4.6 Publication decision gates

```text
Phase 3 corrected record
   -> independent critique
      -> sound and journal-suitable? -> submit existing work
      -> substantial gap?          -> retain as preprint; move on

G0 preregistered protocol
   -> reproducible results and benchmarks
      -> original, interpretable finding? -> preprint + journal submission
      -> more empirical evidence needed? -> limited G1 extension if possible
      -> no contribution?               -> transparent technical report / revise
```

---

## 5. Parallel track E — scientific collaborators, funding and economic opportunities

**Operating rule:** this track starts **now**, does not wait for G0 or M4, and must not distort the scientific claims. Keep partnership, grant and sales pipelines separately recorded.

### 5.1 E0: external problem discovery and scientific contacts

| Contact/area | Status on 23 Oct 2026 | Why relevant | Next evidence required |
|---|---|---|---|
| **Andrea González / ZHAW** | Conversation planned for **29 Oct** | B1 building simulations; possible scientific problem and validation | Defined decision, methods currently in use, plausible small comparison |
| **URA** | Discussion/contact being explored | Real flood/operational decision; M4 observed data | Concrete decision and data feasibility; no presumed access |
| **Peter Scherer / FHNW** | Emailed, no reply after ~1.5 weeks | Possible institutional research introduction | Response and genuine mutual interest |
| **ESA/NASA contacts** | Exploratory outreach, no confirmed partnership | B2 satellite-observation planning and future domain expertise | Real tasking problem and collaborator/data feasibility |
| **Engineering/risk consultancies** | Prospects to identify | Existing model owners and potential audit clients | A responsible decision maker with a real, costly problem |

**Before asking anyone for collaboration, create a one-page problem card:**

- Who owns the decision, and when must it be taken?
- What are the alternatives, resources and constraints?
- What probabilities, posterior draws, scenarios or simulations already exist?
- What mistakes are costly, and to whom?
- What baseline procedure is currently used?
- Can an independent evaluation be designed? Who controls the data and permissions?
- What would count as useful, and would anyone fund a bounded investigation?

**Near-term objective:** 3–5 high-quality discovery discussions, not a publicity campaign.

### 5.2 E1: first commercial hypothesis — a bounded decision-reliability audit

**Potential offering, not yet validated:** an independent review of a defined decision pipeline using an existing quantitative model or simulation.

**Scope might include:**

1. Document data provenance, model assumptions and uncertainty representation.
2. Trace inputs through predictions/scenarios into a concrete decision rule.
3. Stress test priors, modelling choices, scenarios and resource constraints.
4. Describe selection stability, alternative choices and consequence sensitivity.
5. Assess external calibration/performance **only when independent outcomes exist**.
6. Deliver a reproducible report including limitations and recommended additional evidence.

**Not offered:** certification of model truth; guarantees of risk reduction; claims that an unvalidated model becomes valid after an audit; an all-purpose prediction engine.

**Commercial gate:** identifiable pain + authorised data access + decision owner + budget + an agreed, bounded paid test. A helpful conversation and a click on the website are not paying customers.

**Why services before software:** a report can test demand with less engineering burden and can reveal whether existing libraries already solve the actual needs.

### 5.3 E2: possible Swiss research and innovation funding

Funding routes to **investigate**, not assumed entitlements:

- **Innosuisse Innovation Mentoring:** advice for an eligible organisation with a potential innovation project. Mentoring support is not unrestricted personal salary.
- **Innosuisse Innovation Cheque:** a small preliminary research study with an eligible Swiss research partner, subject to programme eligibility; up to CHF 15,000 under the previously reviewed scheme. Its financing is primarily for the research partner's work, not automatically Habnetic wages.
- **Innosuisse Innovation Projects with Implementation Partners:** applied joint research requiring a real implementation organisation, Swiss research partner and an appropriate financial contribution/co-financing.
- **Start-up support / incubators:** only if a suitable legal entity, scalable proposition, IP position and realistic market case exist.
- **Institutional research grants / European consortia:** contingent on qualified applicant, host institution, scientific scope and calls; no assumed access for an unaffiliated independent researcher.

**Suggested trigger for preparing a grant application:** named problem owner, falsifiable research question, candidate research partner, feasibility evidence, data access route, budget and legal eligibility. Verify **current** programme terms at time of application.

**Critical distinction:** grants to universities, cash paid by clients and personal income are different financial realities.

### 5.4 E3: business, legal structure and scale decision

Do not incorporate a new company merely because an application form mentions one. First assess the specific contracting vehicle, liability, taxes, insurance, conflicts and IP rights. Existing company arrangements, if any, must not be presumed suitable without review.

After at least one successful paid study and preferably repeated demand, consider:

- independent specialist consultancy;
- reusable audit service with standard reports;
- applied research partnership;
- open-source software with commercial support, integration or custom analysis.

**No fixed revenue promise or mandatory customer count.** A possible first paid pilot is a test of demand, not evidence of a scalable company.

### 5.5 Protection of employer and third-party intellectual property

- General concepts (Bayesian updating, hierarchical models, partial pooling, validation, decision theory) are scientific knowledge.
- **Do not transfer** employer code, private datasets, internal specifications, calibration choices, outputs, documents or derivations, even if renamed or superficially rewritten.
- Independently design Habnetic experiments from public methods, open datasets, original synthetic generators and clearly recorded public sources.
- Review employment agreements, confidentiality, invention ownership and non-compete/conflict terms before commercial work in an overlapping sector. Copyright is not the only issue; trade secrets and contractual duties can matter.
- Keep private client data and contracts separate from GitHub's public scientific repository. Secure permission before any publication.

---

## 6. Software and infrastructure strategy

### 6.1 A reusable Python library is **conditional**, not the goal

Before creating a general package, compare existing capabilities in **ArviZ**, **PySensMCDA**, **EMA Workbench**, scientific optimisation tools and probabilistic programming libraries. The overlap is real; do not claim a software vacuum.

A possible future reusable layer could encompass:

- standardised uncertainty samples with units, semantics and provenance;
- explicit alternatives/feasibility/capacity constraints;
- posterior selection probabilities and stability;
- sensitivity to prior/model/decision assumptions;
- empirical calibration and realised decision losses where possible;
- controlled transfer-audit reports and baseline comparisons.

**Packaging gate:** two scientifically meaningful implementations demonstrate a recurring unmet requirement that cannot be more simply satisfied by an existing package or upstream contribution. Maintainability and documented users matter.

### 6.2 KnowledgeLab and repository organisation

**GitHub = canonical versioned research source:** code, synthetic generators, notebooks, tests, protocol Markdown, reproducibility scripts and public experiment reports.

**Drive = raw/large research assets:** downloadable source datasets, documentation PDFs, intermediate rasters, public exports and large figures; maintain a manifest and hashes.

**Notion = index and decisions:** concept notes, experiment catalogue, bibliography, contact/decision log and pointers to canonical GitHub/Drive sources. Avoid duplicating the authoritative science there.

**Colab = scratch experiments:** acceptable for prototyping; export the tested implementation to GitHub before citing results as reproducible.

Suggested logical documentation paths (proposals, not assumptions that files already exist):

```text
docs/
  strategy/HABNETIC_MASTER_RESEARCH_STRATEGY.md
  phase3/PUBLICATION_AND_VERSION_LOG.md
  phase4/M0_RTM_BASELINE.md                  # existing authoritative record: retain reference
  phase4/M1_TOPOGRAPHY_PROTOCOL.md
  phase4/M2_HAZARD_PROTOCOL.md
  phase4/M3_CONSEQUENCE_PROTOCOL.md
  phase4/M4_VALIDATION_PROTOCOL.md
  phase4/B1_BUILDING_DECISION_CARD.md
  phase4/B2_SATELLITE_DECISION_CARD.md
  phase5/G0_PREREGISTRATION.md
  phase5/G1_DATA_FEASIBILITY.md
  phase5/D_CROSS_DOMAIN_PROTOCOL.md
  publishing/JOURNAL_SUBMISSION_LOG.md
  external/CONTACT_AND_PROBLEM_LOG.md       # private when personal/client data involved
```

Avoid modifying original frozen M0 records to reflect later interpretations; create linked decision records.

### 6.3 Non-negotiable experiment quality checklist

Every substantial modelling experiment should state:

1. **Scientific question** and falsifiable hypothesis.
2. **Generative story**: units, process, hierarchy, dependence and data-collection mechanism.
3. **Observed likelihood**: target, measurement assumptions and missingness. Explicitly label scenario-only work.
4. **Priors**: parameterisation, units, scaling, external evidence and possible double-use.
5. **Posterior targets**: uncertainty, predictions, decision probabilities and sensitivity.
6. **Checks**: prior predictive, posterior predictive, R-hat/ESS/traces/divergences, alternate prior or likelihood, and appropriate held-out evaluation.
7. **Decision definition**: alternatives, feasibility, costs, budget/capacity, loss/utility and tie rules.
8. **Comparators**: simplest plausible baseline and established competing methods.
9. **Reproducibility**: versioned inputs, dependency lockfile, deterministic seeds where possible, machine/environment note, one-command run and expected outputs.
10. **Failure interpretation**: nulls, leaks, evidence limitations and conditions to stop or simplify.

Default stack: **Python, PyMC, ArviZ, pandas/numpy/xarray as needed, matplotlib, pytest, ruff and black**. Prefer small tested functions over irreproducible monolithic notebooks.

Suggested one-command interface once G0 is implemented (illustrative, not yet available):

```bash
python -m habnetic.experiments.g0 --config configs/g0_baseline.yaml
```

The corresponding README must state dataset/source expectations, run prerequisites, outputs, timing/memory assumptions, seeds and how to reproduce published plots.

---

## 7. Dependencies, priority and decision gates

### 7.1 Parallelism without uncontrolled scope

```text
Phase 3 corrected manuscript --------------------------------> independent critique
                                                                -> possible journal submission

Phase 4 Part A:   M0 [DONE] -> M1 -> M2 -> M3 -> M4 [real-data gate]
                                  |           |
                                  +-- findings and data for future G1 (if useful)

Phase 4 Part B:   B1 [Andrea / building decisions] ---> D [conditional]
                  B2 [satellite planning] -----------> D [alternative]

Phase 5:          G0 protocol -> G0 synthetic benchmark -> G0 preprint -> journal attempt
                                                          |
                                                          v
                  G1 [real observed evidence gate] -> empirical study [conditional]

External track:   collaborations | data access | market discovery | funding [all parallel]

Software:         only after reusable gap and ≥2 substantive implementations
```

**Priority rule:** at most **one primary coding experiment** plus one lighter design/outreach stream at any moment, unless available capacity materially changes. In the immediate period, M1 is primary engineering work; G0 is initially a protocol, not another simultaneous full implementation.

### 7.2 Research gates

| Gate | Required evidence | Decision |
|---|---|---|
| **P3 editorial** | Corrected preprint and independent methodological critique | Journal submission or retain preprint |
| **M1** | Isolated reproducible terrain comparison | Advance, revise or document null |
| **M2/M3** | Justified physical and decision extensions | Advance only if evidence supports added complexity |
| **M4** | Independently measured targets/outcomes and valid splits | Validate, report failure or record evidence gap |
| **B1/B2** | Real decision description, existing baselines and feasible data | Run a small study or defer |
| **G0** | Predeclared simulation, region holdouts, calibrated/predictive and regret baselines | Manuscript candidate, revise or stop |
| **G1** | Multiple independent regions and real compatible outcomes | Empirical test or no-go |
| **D** | Credible non-flood decision and comparisons | Demonstrate method portability or stop |
| **Commercial** | Budget owner, bounded useful problem, data rights, paid scope | Pilot or do not productise |
| **Software** | Demonstrated reusable unmet gap across studies | Package, contribute upstream or remain project-specific |

---

## 8. Publication and operating calendar (indicative)

| Window | Research | Publication | External and economic track |
|---|---|---|---|
| **23–31 Oct 2026** | Preserve M0; prepare M1 dataset inventory and G0 question | Verify corrected Phase 3 Zenodo version/DOI; update site links | Andrea discussion **29 Oct**; prepare URA decision/data questions |
| **Nov–Dec 2026** | Execute M1; sketch B1/B2; predeclare minimal G0 benchmark | Seek methodological critique of Phase 3 and assess journal fit | 3–5 qualified outreach conversations; identify actual decision owners |
| **Q1–Q2 2027** | M2 as feasible; implement G0 once protocol is fixed | Phase 3 journal submission **if review supports it**; G0 manuscript evidence accumulation | Explore a research partner and funding only with a defined project; qualify paid-audit opportunity |
| **Q3–Q4 2027** | Advance M3/M4 where data permit; evaluate G0 stress tests; screen G1 datasets | **G0 preprint and journal submission if the science warrants it** | Scoped paid pilot or funded study if an eligible party commits |
| **2028+** | Real-world G1 and second-domain D if feasible; reflect on model/generalisation failures | Conditional empirical or cross-domain manuscripts | Decide on consultancy, repeatable service, small library or narrowed research programme |

**No artificial scientific deadlines.** Data access, methods, critical feedback and available research time determine actual pace. G0 need not wait for M4. G1 need not depend on M4's specific dataset, but **must** satisfy its own empirical-evidence conditions.

---

## 9. Risk register and corrective actions

| Risk | Warning signal | Response |
|---|---|---|
| Apparent novelty is already established | Literature or software comparator explains all findings | Narrow or change research question; cite prior work |
| Stable but wrong decisions | Stable selections with poor held-out regret | Make failure central; do not advertise stability as validity |
| Synthetic outcomes appear as real validation | Results cannot be matched to independent observed outcomes | State limits explicitly; reserve empirical claims for G1/M4 |
| Leakage/double counting | Prior tuning or preprocessing uses held-out target data | Rebuild splits and pipeline; invalidate affected results |
| Too few independent regions | Very precise estimates driven by many correlated buildings | Report region-level uncertainty; simplify hierarchy or gather more regions |
| Incorrect decision utility | Ranking uses hazard alone while decisions concern costs/consequences | Declare surrogate nature; obtain domain-appropriate loss |
| Domain mismatch | B1/B2 has no genuine probabilistic output or decision objective | Use scenario-language and appropriate baselines, or decline study |
| Overengineering | Larger model without stronger testable claim | Return to the smallest falsifiable model |
| Academic dependence | A prospective collaborator does not reply | Continue independent protocols and other outreach |
| Commercial overstatement | Interested contact but no decision/budget | Classify as discovery, not customer |
| IP/confidentiality conflict | Proposed work depends on employer/client materials | Stop and independently redesign; obtain legal/contractual clarity |
| Funding misunderstood | Grant would pay a partner, not personal time | Build realistic cash flow and eligibility budget |

---

## 10. What the project looks like after Phase 5

There is **no mandatory Phase 6**, startup, publication quota or software launch.

Review the accumulated record on four dimensions:

1. **Scientific contribution:** Is there a falsifiable, replicated and contextualised finding about uncertainty, transfer and decisions that withstands external scrutiny?
2. **Empirical transportability:** Have we validated meaningful claims in observed settings, or must they remain restricted to controlled simulation?
3. **Practical relevance:** Have actual decision owners identified a consequential problem and funded or adopted a bounded study?
4. **Technical reuse:** Has independent use in different settings established a need for a reusable software layer beyond existing tools?

**Possible outcomes:** continue methodological research; collaborate with a research institution; offer a small consultancy/audit service; package a limited open-source component with support; or conclude the research programme and archive all reproducible findings. Multiple outcomes may coexist, but each requires separate evidence.

A negative or narrow outcome is not an organisational failure if the experiment answered its question responsibly.

---

## 11. Immediate action checklist: next two weeks

### Publication and public record

- [ ] Freeze and verify the corrected Phase 3 manuscript and Zenodo DOI/version history.
- [ ] Record publication changes and check every website one-pager/demo link that refers to the paper.
- [ ] Update the Research and Applications English pages to reflect completed research versus proposed Phase 4/5 work; then synchronise translations EN/DE/ES/EU.
- [ ] Prepare a short independent critique request for Phase 3; **do not** start an arbitrary journal submission until fit and contribution are assessed.

### Research

- [ ] Preserve M0 frozen references.
- [ ] Create M1 topography input/data inventory and choose one feature.
- [ ] Write a two-page G0 experiment preregistration with three shift regimes, 0/few-shot targets, baseline arms, region-held-out splits and two primary decision metrics.
- [ ] Start targeted literature/novelty matrix against Bayesian transfer, negative transfer, robust decisions, SMAA, EMA Workbench and PySensMCDA.

### People and feasibility

- [ ] Prepare Andrea meeting for **29 October**: one-sentence proposal, specific building-simulation decision questions, baseline/validation questions, and no assumed collaboration.
- [ ] Record URA data and decision questions separately.
- [ ] Mark FHNW Peter Scherer as pending, not an active workstream.
- [ ] Create a concise private contact/problem-discovery log and select 3–5 additional qualified conversations.
- [ ] Explore Innosuisse only once a defined implementation problem and eligible research/business partner are identified.

---

## 12. Source hierarchy, working references and change control

**This document is the cross-programme coordination strategy. It does not supersede frozen or published scientific records.**

Authoritative internal evidence and related planning inputs:

1. **Published Phase 3 Zenodo preprint:** https://doi.org/10.5281/zenodo.23110714. Actual record/version to be rechecked after corrections.
2. **Phase 4 roadmap v1.2, dated 22 October 2026:** canonical detailed M0–M4 and Part B plan until an explicit revision is adopted.
3. **Phase 4 M0 Rotterdam reproduction audit:** frozen code reference and exact audit findings cited above.
4. **Phase 4 v1.3 proposed amendments:** useful additions, but not silently treated as approved or as a replacement for the v1.2 source.
5. **Phase 5 concept note:** detailed proposed hypotheses and study design; not implemented evidence.
6. **This master plan:** coordinates research, publication stages, partners, money and future decision gates.

**Change policy:** future material changes require a short dated decision note identifying the reason, the affected scientific assumption, what is newly proposed, and what completed work remains unchanged. Revisions may reorder experiments without rewriting historic results.

**Public background reading and comparison starting points:** Bayesian transfer learning, partial pooling and negative transfer; distribution shift and predictive calibration; spatial/hierarchical cross-validation; proper scoring rules and decision regret; stochastic multicriteria acceptability analysis (SMAA), EMA Workbench and PySensMCDA. Verify current bibliographic records and exact software capabilities in each manuscript's formal literature review.

---

## Assumptions made

- Independent research capacity remains constrained by other employment and personal obligations.
- No institutional partner, funding award, access to independent real-world loss data or paying client is currently confirmed by this plan.
- The scientific core remains inference/evidence quality **plus** constrained decisions and reliability, not development of a universal predictive model.
- Phase 3 is already public as a preprint; the correction/versioning operation still needs confirmation.
- G0 can be independently simulated; a credible G1 requires genuinely observed outcomes and enough independent regions.
- No use of employer-proprietary assets or confidential knowledge is authorised or needed.

## How to validate this plan

- Compare M0/M1–M4 commitments against the actual Phase 4 v1.2 source and frozen code.
- Compare every external/public statement with published evidence and marked future hypotheses.
- Confirm journal policies and funding eligibility before any formal application.
- For each experiment, apply its **go/no-go gate** and update the dated decision log.
- Verify that one experiment can be reproduced by someone other than its author using documented inputs and a single command.

## Next small step

**Close and verify the corrected Phase 3 Zenodo record. Then update the two website pages, conduct the Andrea conversation on 29 October, and execute M1 while pre-registering the minimal G0 experiment.**

**What to test next:** Can a deliberately misspecified hierarchical model make target-region selections *more stable* while also increasing *held-out decision regret*, and do standard diagnostics detect the problem before the decision is made?
