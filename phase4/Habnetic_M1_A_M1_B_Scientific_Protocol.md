# Habnetic | Phase 4 M1-A / M1-B: Pre-analysis Scientific Protocol

**Version:** 0.1.0 (proposed; no model run under this protocol yet)  
**Date:** 29 October 2026  
**Proposed repository path:** `Habnetic/docs/phase4/Habnetic_M1_A_M1_B_Scientific_Protocol.md`  
**Evidence class:** synthetic outcome experiments with observational AHN5 covariates.  
**Reference:** Phase 3 / M0 code frozen at `5e19254` (`phase3-code-freeze-20261022`).  
**Input report:** [M1-P04 Terrain Data and Quality Report](Habnetic_M1_P04_Terrain_Data_Quality_Report.md).

## 1. Scientific objective

Test how adding one documented observational covariate, local relative terrain elevation, affects **posterior inference, constrained asset selection, decision stability and synthetic expected decision loss**, separating predictive information from measurement and model uncertainty.

- **M1-A: negative-control information experiment.** The synthetic damage-generating mechanism has **no additional dependence on terrain conditional on the original Phase 3 predictors**. Does adding `T50` create spurious apparent information or destabilize decisions?
- **M1-B: known-signal experiment.** Generate fresh synthetic damage outcomes with a **specified terrain dependence**. Under what signal strengths and conditions does including `T50` improve held-out prediction or constrained selection?

A ranking change alone is **not** an improvement. Synthetic decision results do **not** demonstrate empirical flood-risk prediction. The experiment is about generalizable methodology for decisions under uncertainty; Rotterdam flooding is its first test bed.

## 2. Preregistered evidence boundaries and comparability

| Dimension | Phase 3 / M0 reference | M1-A and M1-B requirement |
|---|---|---|
| Asset identifiers | 221,324 OSM-derived `bldg_id` | Preserve identifiers and the full source inventory; never alter historical M0 output |
| Outcome type | Binary synthetic damage (`Y_damage`), synthetic probability `p_damage_true` in RTM Phase 3 features | Synthetic only; new labels in M1-B must be separately stored and seeded |
| Original covariates | Exposure and hazard; Phase 3 logistic parameterization includes `alpha`, `beta_E`, `beta_H` | Reuse **exact Phase 3 feature transformations and priors** after verifying the implementation; no guessed replacement formulas |
| Additional covariate | None | `T50` primary; `T25` alternative |
| Selection policy | Posterior-derived top-k selection | Reuse original code and tie rule after inspecting it; do not redefine inclusion probability |
| Budget / eligibility | Full RTM reference uses `k=2213` in previous Phase 3 checks | Fixed within every paired A/B evaluation. A restricted or held-out candidate universe is **not** directly comparable to historical full-city M0 without relabelling. |

**Population decision for the primary controlled comparison:** use only assets with measured `T50` (**220,456**), and refit **both** candidate models on the **same** observations/partition. This avoids inventing 868 missing terrain values and cleanly isolates the effect of adding terrain. The frozen full-city M0 results remain a *historical reference*, not a directly matched comparator. Label the result `shared-measured-universe`.

**Primary selection/evaluation design:** a fixed **spatial-block holdout** within the measured universe. Define grid blocks from building coordinates in EPSG:28992 and assign whole blocks to train/test with a recorded seed; target approximately 80% train and 20% test. Select from **held-out buildings only** in the primary evidence claim. Predefine capacity as `k_eval = floor(0.01 × N_test)` (require `k_eval ≥ 1`) and use exactly this `k_eval` for both models. Never compare that top-k directly to Phase 3's `k=2213` citywide list as if capacities and universes matched.

A separate **exploratory full-city ranking** may later score the measured 220,456 assets with a fixed `k=2213` for descriptive comparison; it cannot be framed as the primary held-out performance test. Full-universe selection of all 221,324 assets needs a *separately preregistered* missing-terrain strategy and remains deferred.

**Final pre-run TODOs (must resolve from actual Phase 3 code, not inference):** exact model predictors/scaling, priors, outcome-generation code, spatial-block size and seed, deterministic top-k tie rule, score/utility formula, baseline posterior settings, and whether `p_damage_true` is accessible for all selected assets. Record decisions in an immutable run configuration **before** examining decision results.

## 3. Common data protocol

**Inputs** (paths relative to their repositories):

- `Habnetic/data/runs/rtm_m1_citywide/terrain_features_m1_5m.parquet` (221,324 rows; 220,456 terrain-measured), with `T50`, `T25`, counts and coverage.
- `Habnetic/data/processed/RTM/derived/buildings_rtm.gpkg` for IDs/geometries and OSM building types.
- `Habnetic/resilient-housing-bayes/outputs/phase3/RTM/phase3_features_scaled.parquet` and exact frozen Phase 3 M0 reference artifacts, including synthetic outcome and generating probability if present.
- `Habnetic/data/runs/rtm_m1_terrain_pilot/terrain_resolution_comparison.csv` for **limited one-tile** 5 m vs 0.5 m measurement sensitivity.

**Join key:** `bldg_id`. Enforce uniqueness, exact shared IDs, consistent ordering and no duplicate assets. Assert `T50`, `T25` only used if `terrain_measured=True`. Preserve original metres; use a **predefined deterministic transform** for model scale, estimated using **training inputs only**, and apply unchanged to held-out inputs. Because `T50` has heavy tails (`−11.23` to `+16.08 m`), a proposed minimal transform is:

\[
u_i=\operatorname{asinh}(T50_i / 1\,\mathrm{m}),\quad
x_{T,i}=(u_i-\mu_{\mathrm{train}})/s_{\mathrm{train}}.
\]

Here \(s_{\mathrm{train}}\) is the **training standard deviation** (require >0), and \(\mu_{\mathrm{train}}\) its mean. This produces a dimensionless covariate with a physically defined asinh scale before train-only standardization. It is an **analysis proposal**, not yet an implemented or validated transform. Do not winsorize or delete extreme cases for the primary run. Document the fitted scalers and transform version; apply the same scaling recipe in both model comparisons.

**Missingness:** the 868 unavailable terrain values are explicitly excluded **from both models in the primary paired analysis**, and their counts/types reported. This is a selection restriction, not missing-at-random inference; possible missing-not-at-random bias is part of limitations. Secondary sensitivity scenarios may use lower-coverage exclusions but must do so for both models with the same resulting candidate universe and predeclared budgets.

## 4. M1-A: negative-control experiment

### 4.1 Generative story

Use the **frozen Phase 3 synthetic mechanism** and its existing RTM synthetic outcomes. Let \(X_i\) denote exactly the exposure/hazard predictors in Phase 3. The null is:

\[
Y_i\mid X_i,T50_i \sim \operatorname{Bernoulli}(p_{0,i}),
\qquad p_{0,i}=g_{\mathrm{Phase3}}(X_i),
\]

with no additional `T50` term **conditional on** \(X_i\). `T50` may correlate with exposure/hazard marginally. Confirm the actual synthetic generating process from frozen source; otherwise this negative-control interpretation must be amended before execution. The observed outcome is synthetic, not real loss.

### 4.2 Competing likelihoods

**A0 (baseline):** the original Phase 3 binary logistic likelihood on original predictors and link function, refit on the shared measured-universe training set.

**A1 (augmented):** identical likelihood and original predictors plus one terrain term \(\gamma x_{T,i}\) in the linear predictor:

\[
Y_i\sim\operatorname{Bernoulli}(\sigma(\eta_{0,i}+\gamma x_{T,i})),
\]

where \(\eta_{0,i}\) is the fitted linear predictor from the verified Phase 3 specification and \(\sigma\) is the logistic function. Do not add extra interactions or nonlinear terrain effects in the primary comparison.

### 4.3 Priors and hypotheses

- Reuse existing Phase 3 priors for shared parameters **on the same variable scales**. If scaling changes, document the implied prior changes and reconcile them before fitting.
- For standardized terrain: proposed \(\gamma\sim\mathcal N(0,0.5^2)\), alternative prior \(\mathcal N(0,1^2)\). Both need prior predictive checks.
- **Null expectation:** no consistent **held-out** predictive or decision benefit from the extra variable; posterior \(\gamma\) should be compatible with zero under adequate model specification. Do not demand point-estimate equality to zero or identical top-k rankings in finite samples.

### 4.4 Posterior quantities and checks

Estimate posterior of \(\gamma\), held-out predictive risks, top-k inclusion probabilities under posterior draws, selection stability, and expected synthetic decision utility. Check prior predictive event-rate plausibility; posterior predictive event rates/calibration; split-respecting held-out log loss and Brier score; convergence (`R-hat`, bulk/tail ESS, divergences, trace behavior); and sensitivity to alternate terrain prior. Use PSIS-LOO/WAIC on an appropriately sized, supported comparison **only as secondary model checks**, not a substitute for spatial holdout. A0/A1 comparisons must use identical held-out assets and capacity.

### 4.5 Falsification criterion

M1-A fails its intended negative-control interpretation if A1 achieves systematic positive held-out decision utility across repeats under the verified null and the gain cannot be attributed to numerical error, finite-sample variance, altered information sets or model misspecification. A single changed ranking is **not** failure.

## 5. M1-B: controlled synthetic terrain-signal experiment

### 5.1 Generative story

Using **the same assets, terrain transform and spatial train/test partition**, build a new synthetic label set with explicit terrain dependence. If Phase 3 exposes true synthetic probabilities \(p_{0,i}\), define:

\[
\eta_{0,i}=\operatorname{logit}(\operatorname{clip}(p_{0,i},\epsilon,1-\epsilon)),
\quad
p_{\gamma,i}=\sigma(\eta_{0,i}+\gamma_\mathrm{true}x_{T,i}),
\quad
Y_{\gamma,i}\sim\operatorname{Bernoulli}(p_{\gamma,i}).
\]

Proposed \(\epsilon=10^{-6}\) is numerical protection only; record how many probabilities require clipping. This recipe **modifies only synthetic outcomes** and stores new labels separately; it does not modify AHN terrain or assert a causal relationship in the real world.

**Prespecified signal scenarios (proposed):** \(\gamma_\mathrm{true}=0\) (replicated null), \(−0.5\) (higher local elevation hypothetically protective) and \(+0.5\) (sign reversal as a stress test). Coefficients operate on the standardized, transformed terrain feature. They are chosen for methodological contrasts, **not calibrated flood effects**. Use explicitly enumerated random seeds. For every scenario, compare the same synthetic label draw between B0 and B1.

If Phase 3 synthetic probabilities are absent, **stop and retrieve/reproduce the frozen generating mechanism**. Do not invent a replacement baseline p without documenting a protocol amendment.

### 5.2 Competing likelihoods and priors

- **B0:** original verified Phase 3 predictor set, no terrain term.
- **B1:** same predictors, likelihood, training labels and priors plus \(\gamma x_T\).
- Prior predictive: inspect the induced distributions of synthetic damage probability and event counts for all proposed \(\gamma_\mathrm{true}\) scenarios before fitting.
- Use the same primary/alternative prior for \(\gamma\) as M1-A. Inference should be blind to the scenario's true \(\gamma\) except during evaluation.

### 5.3 Posterior estimands and diagnostic checks

Report \(\gamma\) recovery (interval coverage/estimation error over repeats), calibration and held-out log/Brier scores, posterior predictive checks, `R-hat`, ESS, divergences and trace behaviour. Check posterior sensitivity to prior width, T25 substitution and missing/low-coverage restrictions. Record software versions, seed and source hashes with every run. If MCMC at citywide scale is prohibitive, pilot on a predeclared smaller spatial subset and report it as such; do not silently switch inference algorithms.

## 6. Decision experiment: held-out top-k selection

**Decision story:** under a fixed capacity \(k\) and uniform hypothetical intervention benefit/cost, select \(k\) held-out assets to maximize the total synthetic damage probability covered. This is a *stylized* expected prevented-event metric, not a real-world cost-benefit model. All models share the same eligible assets, posterior draws convention, tie rule, selection constraints and capacity.

For posterior sample \(s\), compute a risk score \(p_i^{(s)}\) using the **verified existing Phase 3 decision pipeline**. Form the exact top-\(k\) set \(S^{(s)}\). Then:

\[
\pi_i=\frac{1}{S}\sum_{s=1}^{S}\mathbb{1}\{i\in S^{(s)}\},
\qquad \sum_i\pi_i=k.
\]

Here \(\pi_i\) is a **posterior-derived inclusion probability**, not the probability that asset \(i\) experiences damage. A single final policy \(\hat S\) can be constructed from the same preregistered posterior point-score aggregation (for example posterior mean probability), but its aggregation and tie-breaking must match between models and be defined before runs.

When the synthetic truth \(p_i^{\mathrm{true}}\) is known, evaluate:

\[
U(\hat S)=\sum_{i\in \hat S}p_i^{\mathrm{true}},\qquad
R(\hat S)=U(S^*)-U(\hat S),
\]

where \(S^*\) selects the \(k\) largest true synthetic probabilities **within the identical held-out eligible set**. `U` is expected synthetic events covered, and `R` is nonnegative oracle regret. Higher `U`/lower `R` indicates better **synthetic expected prioritization**, not validated real avoided flood losses.

**Primary comparison:** paired \(\Delta U=U(\hat S_{\mathrm{augmented}})-U(\hat S_{\mathrm{baseline}})\) on each held-out draw/scenario; aggregate distribution across fixed synthetic seeds/spatial splits. Report uncertainty intervals across replicate scenarios and posterior draws where appropriate. Predeclare tie handling, all-zero/near-tie behaviour and any cost model before execution.

**Secondary decision diagnostics:** selected-set Jaccard/overlap, swaps and turnover, probability-of-inclusion shifts, borderline inclusion share, dispersion of inclusion probabilities, proportion with \(\pi\) near 0/1, and sensitivity to `T25`/coverage and OSM type. Use exact Phase 3 metrics where available; label newly introduced metrics with distinct definitions.

## 7. Gates, controls and sensitivity matrix

| Gate | Required evidence before proceeding |
|---|---|
| G0: Input contract | Exact ID joins, `p_damage_true`/outcome columns, verified Phase 3 model and decision functions, source hashes and train-only feature scaling |
| G1: Negative control | M1-A prior predictive and held-out PPC/calibration; inference diagnostics; primary decision metrics |
| G2: Synthetic signal | M1-B truth probability and seeds saved separately, observed synthetic event rates, scenario-specific prior predictive checks |
| G3: Fit quality | Converged chains (`R-hat`, ESS, divergences, trace), posterior predictive adequacy; document failures instead of publishing them as validated |
| G4: Decision quality | Same exact eligible set and `k` for the paired models, exact \(\sum_i\pi_i=k\), synthetic oracle regret/utility reported |
| G5: Sensitivities | At least `T25` instead of `T50`; alternate terrain prior; preset low-coverage / asset-type subgroup checks, each rerun with matched populations |
| G6: Reporting | Separate synthetic prediction, decision effects, technical data integrity and unsupported real-world claims; include provenance and limitations |

**Suggested staged execution, not implemented:** (1) verify frozen Phase 3 pipeline and make a tiny one-command smoke test using a preregistered subset; (2) negative-control M1-A; (3) M1-B at small sample for convergence/performance, then scale when justified; (4) repeated scenario evaluation and sensitivity; (5) checked scientific bundle under the [Experiment Artifact Contract](Experiment_Artifact_Contract.md). Do not publish selection maps as measured flood outcomes.

## 8. Interpretation rules and possible outcomes

1. **A1 and A0 similar; B1 better under signal:** expected evidence that the method detects/usefully exploits *injected synthetic information*, not real flood predictive power.
2. **A1 apparently better under null:** test leakage, changes in feature scale/priors, model misspecification, spatial split effects or repeated-measure instability before treating it as substantive.
3. **B1 predicts better but decisions barely change:** plausible when ranking under tight capacity is insensitive to the added information or gains occur far from the top-k boundary.
4. **B1 rankings change but regret does not improve:** instability without demonstrated decision benefit.
5. **Measurement-sensitive assets dominate swaps:** report sensitivity to DTM representation, missingness and asset type. Never select the favoured resolution *after* observing which one produces attractive outcomes.
6. **Convergence or PPC failure:** no affirmative inference/decision claim until diagnosed, with results marked exploratory.

## 9. Outputs, reproducibility and version control

**Repository responsibility:**

- `Habnetic/docs`: this pre-analysis protocol, M1-P04 report and any versioned amendments/decision log.
- `Habnetic/data`: raw terrain, derived mosaic, per-building features, manifests, inventories and checksums; large assets stay out of Git when unsuitable.
- `Habnetic/resilient-housing-bayes`: actual probabilistic code, environment lock, tests, model fitting and decision outputs. The Phase 3 / M0 tag stays frozen.
- Future Operations Center: read-only curated artifact publication only; not a scientific runtime or replacement for checks.

**Run artifact expectation** (not files that already exist): one versioned run directory per scenario/seed/model with config, input hashes, code commit, training/test IDs, scaler parameters, synthetic truth where appropriate, inference summaries/diagnostics, predictive scores, decision metrics, posterior-derived inclusion probabilities and a compact `manifest.json`. Record `analysis_kind`, `outcome_evidence=synthetic`, `status`, checks and limitations per the existing artifact contract. Keep source-of-truth p_true and new synthetic Y separate from any fitted output.

**One-command principle:** exact commands and dependency versions are **TODOs** until the existing Phase 3 experiment entrypoints and the new M1 code have been inspected. Do not fabricate runnable commands in a protocol. Once implemented, document a single testable command and expected input/output schema in the scientific repo README, with seed and environment lock.

## 10. Preregistration checklist and decision log

Before the first M1-A fit, freeze a versioned configuration resolving:

- [ ] Exact Phase 3 generator, model predictors, link, priors, scaling and sampling configuration verified from frozen code.
- [ ] Source dataset hashes and code commits; stable 220,456-ID shared-universe list.
- [ ] Geographical block definition, assignment seed, train/test counts and leakage checks.
- [ ] Terrain transform (`asinh` proposal), standardization and alternative prior agreed after prior predictive checks.
- [ ] Synthetic truth construction and clipping rule, scenario effect strengths and repetition seeds.
- [ ] Exact policy score aggregation, tie rule, capacity formula and evaluation truth/utility.
- [ ] Model checks, thresholds for debugging, sensitivity grid and stopping rules specified.
- [ ] No M0 edits, no real-world flood-damage claims, no unreviewed QA reclassification.

**Decision `M1-D02` (proposed):** run M1-A first on the measured shared universe with a held-out spatial evaluation, then M1-B with preregistered synthetic terrain effects. Treat Phase 3 M0 full-population outputs as immutable context, not an automatically identical comparator.

**What to test next:** verify the actual Phase 3 generative/model/decision code and turn the remaining explicit TODOs into one frozen M1 pre-run configuration. Only then begin Bayesian implementation.
