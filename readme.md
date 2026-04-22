# ExperimentationMeasurement

A collection of projects focused on the design, execution, and analysis of experiments — and what to do when you can't run one. Built on realistic synthetic data modeled after the kinds of problems that show up at large-scale digital platforms: gaming, streaming, subscription products, and consumer apps.

Every project starts with a real business question. The statistics exist to answer it, not the other way around.

---

## Why this repo exists

Running an A/B test is easy. Running one correctly — with the right randomization unit, the right metric, enough power, and a results readout that actually drives a decision — is the job. This repo documents how I think about experimentation end-to-end, including the parts that usually get skipped: interference, variance reduction, metric design, and communicating uncertainty to people who don't care about p-values.

---

## Projects

### 1. `ab-framework` — Reusable A/B Testing Framework
> *How do we standardize experimentation so every test is designed and analyzed the same rigorous way?*

A Python framework covering the full experiment lifecycle: power calculation, randomization, sequential monitoring, and a plain-English results readout. Includes a "broken experiments" module — intentionally flawed tests with annotated diagnoses showing what went wrong and how to catch it.

**Methods:** Power analysis · t-test · Mann-Whitney · proportion z-test · multiple comparisons correction (Bonferroni, BH) · guardrail metric checking

**Skills demonstrated:** Experiment framework design · hypothesis development · metric selection · results communication · scalable best practices

---

### 2. `bayesian-ab` — Bayesian A/B Testing with Sequential Monitoring
> *Can we make a ship/no-ship decision before the experiment is scheduled to end — without inflating false positives?*

Frequentist fixed-horizon testing fails when stakeholders peek. Bayesian sequential testing solves this by updating continuously and stopping when there's enough evidence. Simulates a PlayStation Store conversion rate experiment with daily posterior updates, a ROPE-based stopping rule, and a comparison against a frequentist test that peeked at the same checkpoints.

**Methods:** Beta-Binomial conjugate model · PyMC · posterior probability of superiority · ROPE · expected loss · mSPRT · always-valid confidence sequences

**Skills demonstrated:** Sequential testing · Bayesian inference · stopping rules · peeking problem · communicating posteriors to non-technical partners

---

### 3. `cuped` — Variance Reduction with CUPED
> *How do we run shorter experiments with fewer users without sacrificing statistical power?*

CUPED (Controlled-experiment Using Pre-Experiment Data) uses pre-experiment behavior as a covariate to reduce outcome variance. Simulates a feature experiment on a gaming platform with high natural variance in session time. Shows the variance reduction achieved, the equivalent sample size savings, and how confidence intervals shrink — then documents the tradeoffs and assumptions.

**Methods:** CUPED · ANCOVA · OLS covariate adjustment · residualization · variance reduction % · post-stratification · MLRATE

**Skills demonstrated:** Variance reduction · experiment efficiency · covariate adjustment · experiment platform thinking

---

### 4. `quasi-experiments` — Causal Inference Without Randomization
> *A feature shipped without an experiment. Did it actually change behavior?*

Three quasi-experimental methods applied to the same synthetic scenario — a platform update that rolled out to all users with no holdout. Difference-in-differences uses a comparable unaffected cohort. Regression discontinuity exploits a version threshold as a natural experiment. Interrupted time series models the pre/post trend break. Each method gets its own assumptions, diagnostics, and honest limitations section.

**Methods:** Difference-in-differences · parallel trends test · regression discontinuity · ITS · ARIMAX · Bayesian structural time series

**Skills demonstrated:** Quasi-experimental design · causal identification · method selection judgment · assumption validation

---

### 5. `network-effects` — Experiments Under Interference
> *What happens when treatment and control users interact with each other?*

Standard A/B assumptions break down on social and multiplayer platforms. Simulates a new Party Chat feature on a synthetic social gaming network. Shows how individual randomization produces biased estimates due to spillover, then applies cluster randomization by friend group and a switchback design as alternatives. Compares all three estimates.

**Methods:** SUTVA · cluster randomization · graph clustering (networkx) · switchback design · exposure mapping · bias quantification

**Skills demonstrated:** Network experiment design · interference detection · cluster-level analysis · platform-specific experiment thinking

---

### 6. `metric-design` — Metric Selection & Sensitivity Analysis
> *We can measure a lot of things. Which ones should we actually put in the experiment?*

A framework for evaluating and selecting experiment metrics — not just picking what's easy to measure. Applied to a synthetic subscription gaming platform. For each candidate metric, documents sensitivity (will it move if the feature works?), trustworthiness (can it be gamed or confounded?), and directionality (does up always mean good?). Includes a guardrail metric audit.

**Methods:** Metric sensitivity simulation · variance estimation · minimum detectable effect analysis · Goodhart's Law diagnostics · OEC (overall evaluation criterion) design · ratio metric variance via delta method

**Skills demonstrated:** Metric design · experiment strategy · hypothesis development · guardrail framework · translating metrics to business outcomes

---

### 7. `geo-testing` — Geo-Based Experiments for Platform-Wide Changes
> *How do we test something that can't be randomized at the user level?*

Some interventions — pricing changes, app store promotions, TV campaigns — happen at the market level. Simulates a subscription price test across synthetic regional markets. Applies synthetic control for counterfactual estimation and validates the pre-treatment parallel trends assumption. Compares against a naive pre/post estimate to quantify the bias from not having a proper control.

**Methods:** Synthetic control · difference-in-differences · DMA-level randomization · pre-treatment fit diagnostics · placebo tests · RMSPE

**Skills demonstrated:** Geo experiment design · market-level causal inference · synthetic control · incrementality estimation

---

### 8. `player-retention` — Retention Curve Modeling & Experiment Sensitivity
> *Does this feature change long-term retention, and would our experiment have been able to detect it?*

Survival analysis applied to synthetic player cohort data — daily logins, session lengths, and churn events across 90 days post-signup. Fits a Kaplan-Meier curve and a Weibull survival model. Then simulates what a 5% improvement in D7 retention would look like downstream on D30 and LTV, and back-calculates what experiment duration and sample size would be needed to reliably detect it.

**Methods:** Kaplan-Meier · Weibull survival model · D1/D7/D30/D90 retention curves · power analysis for survival outcomes · LTV projection · lifelines

**Skills demonstrated:** Retention modeling · survival analysis · experiment sensitivity · connecting experiment outcomes to business metrics

---

### 9. `results-communication` — Translating Experiment Results into Decisions
> *The experiment is done. Now what do we actually tell leadership?*

A structured readout template and worked examples for communicating experiment results to non-technical stakeholders. Takes three synthetic experiment outcomes — a clear win, a clear null, and an ambiguous borderline case — and writes each up as a ship/no-ship recommendation memo. Documents assumptions, risks, and what would have to be true for the recommendation to be wrong.

**Methods:** Effect size framing · practical vs. statistical significance · confidence interval communication · decision framework · sensitivity analysis narrative · risk framing

**Skills demonstrated:** Stakeholder communication · decision-driving analytics · technical-to-business translation · experiment readout design

---

### 10. `multi-metric-testing` — Multiple Metrics, Multiple Comparisons, and Experiment Integrity
> *We're testing 12 metrics. How do we avoid fooling ourselves into thinking something worked?*

Multiple comparisons inflate false positive rates — if you test enough metrics, something will look significant by chance. Simulates an experiment with a realistic metric suite (primary, secondary, guardrails) and applies Bonferroni, Benjamini-Hochberg, and Bayesian shrinkage approaches to control error rates. Includes a worked example of a "winning" experiment that was actually a false positive, and how each correction method would have caught it.

**Methods:** FWER · FDR · Bonferroni correction · Benjamini-Hochberg · Bayesian shrinkage · hierarchical modeling for multiple outcomes · simultaneous credible intervals

**Skills demonstrated:** Multiple comparisons · experiment integrity · metric hierarchy design · false positive control

---

## Stack

```
Python · PyMC · ArviZ · scipy.stats · statsmodels · EconML
lifelines · networkx · pandas · NumPy · matplotlib · seaborn
SQL · Jupyter
```

---

## Structure

```
ExperimentationMeasurement/
├── ab-framework/
│   ├── data/                  # synthetic data generation
│   ├── notebooks/             # step-by-step walkthrough
│   ├── src/                   # reusable experiment class
│   └── README.md
├── bayesian-ab/
├── cuped/
├── quasi-experiments/
├── network-effects/
├── metric-design/
├── geo-testing/
├── player-retention/
├── results-communication/
└── multi-metric-testing/
```

Each project folder contains its own README with the business question, modeling decisions, key findings, and honest limitations.

---

## About

Built by Justin — data scientist with 10+ years across retail, supply chain, marketing analytics, finance, and SaaS. Focused on experimentation, causal inference, and Bayesian methods applied to real product and business decisions.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/justindwall)