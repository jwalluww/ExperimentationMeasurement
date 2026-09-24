# ExperimentationMeasurement

A collection of projects focused on the design, execution, and analysis of experiments — and what to do when you can't run one. Built on realistic synthetic data modeled after the kinds of problems that show up at large-scale digital platforms: gaming, streaming, subscription products, and consumer marketplaces.

Every project starts with a real business question. The statistics exist to answer it, not the other way around.

---

## Why this repo exists

Running an A/B test is easy. Running one correctly — with the right randomization unit, the right metric, enough power, and a results readout that actually drives a decision — is the job.

This repo documents how I think about experimentation end-to-end, including the parts that usually get skipped: interference, variance reduction, metric design, concurrent experiment interactions, and communicating uncertainty to people who don't care about p-values.

The fictional companies used throughout are invented specifically for this repo. They're designed to mirror the types of platforms where experimentation is core infrastructure: a music streaming service, a gaming platform, a two-sided delivery marketplace, and a short-term rental platform.

---

## Fictional companies

| Company | Type | Real-world analog |
|---------|------|------------------|
| **Meridian** | Music streaming & podcast platform | Spotify |
| **Vaultline** | Gaming platform & digital storefront | PlayStation Store / Steam |
| **Carvo** | Two-sided delivery & rideshare marketplace | Uber / Instacart |
| **Nestaway** | Short-term rental marketplace | Airbnb |

---

## Projects

### 1. `ab-framework` — Reusable A/B Testing Framework
> *Did this change we made actually work, and how do we know we're not fooling ourselves?*

A reusable experimentation framework covering the full experiment lifecycle: power calculation, randomization, SRM detection, and a plain-English results readout. Includes a **"broken experiments museum"** — deliberately flawed tests (underpowered, peeked at early, wrong randomization unit, SRM ignored) with annotated diagnoses showing what went wrong and how to catch it. Also contains a lightweight experiment brief template for registering experiments before launch.

**Scenario:** Meridian tests a redesigned onboarding flow for new subscribers.

**Methods:** Power analysis · proportion z-test · t-test · Mann-Whitney · SRM detection · guardrail metric checking · experiment brief template

**Skills demonstrated:** Experiment framework design · hypothesis development · pre-launch registration · randomization validity · results communication · scalable best practices

---

### 2. `bayesian-ab` — Bayesian A/B Testing with Sequential Monitoring
> *Can we make a ship/no-ship decision before the experiment is scheduled to end — without inflating false positives?*

Frequentist fixed-horizon testing fails when stakeholders peek at results mid-experiment. Bayesian sequential testing solves this by updating continuously and stopping when there is enough evidence — without inflating the false positive rate. Simulates a conversion rate experiment with daily posterior updates, a ROPE-based stopping rule, and a direct comparison against a frequentist test that peeked at the same checkpoints.

**Scenario:** Vaultline tests a new game recommendations widget on the store homepage.

**Methods:** Beta-Binomial conjugate model · PyMC · posterior probability of superiority · ROPE · expected loss · mSPRT · always-valid confidence sequences

**Skills demonstrated:** Sequential testing · Bayesian inference · stopping rules · peeking problem · communicating posteriors to non-technical partners

---

### 3. `cuped` — Variance Reduction with CUPED
> *How do we run shorter experiments with fewer users without sacrificing statistical power?*

CUPED (Controlled-experiment Using Pre-Experiment Data) uses pre-experiment behavior as a covariate to reduce outcome variance — tightening confidence intervals and increasing effective power without adding a single user. Shows the variance reduction achieved, the equivalent sample size savings, and how the method extends to multiple covariates via OLS. Documents the assumptions and when CUPED doesn't help.

**Scenario:** Vaultline tests a new in-session achievement notification on weekly session time, a high-variance continuous metric.

**Methods:** CUPED · ANCOVA · OLS covariate adjustment · residualization · variance reduction % · post-stratification · MLRATE

**Skills demonstrated:** Variance reduction · experiment efficiency · covariate adjustment · experiment platform thinking

---

### 4. `metric-design` — Metric Selection & OEC Design
> *We can measure a lot of things. Which ones should we actually put in the experiment?*

A framework for evaluating and selecting experiment metrics — not just picking what's easy to measure. For each candidate metric, documents sensitivity (will it move if the feature works?), trustworthiness (can it be gamed or confounded?), and directionality (does up always mean good?). Builds a composite OEC from component metrics, demonstrates the delta method for ratio metric variance, and includes a guardrail metric audit.

**Scenario:** Meridian designs the metric suite for a major algorithm change to its Discovery feed.

**Methods:** Metric sensitivity simulation · variance estimation · MDE analysis · Goodhart's Law diagnostics · OEC design · ratio metric variance via delta method · guardrail framework

**Skills demonstrated:** Metric design · OEC construction · experiment strategy · hypothesis development · translating metrics to business outcomes

---

### 5. `network-effects` — Experiments Under Interference
> *What happens when treatment and control users interact with each other?*

Standard A/B assumptions break down on social and marketplace platforms. Simulates a new social feature on a gaming network, shows how individual randomization produces biased estimates due to spillover, then applies cluster randomization by friend group and a switchback design as alternatives. Quantifies the bias from naive randomization and compares all three estimates. Includes a section on when user-level testing is still defensible despite known interference, and how to reason about the direction of bias.

**Scenario:** Vaultline tests a new Party Up feature that lets players form squads — on a platform where users are connected in an active friend graph.

**Methods:** SUTVA · cluster randomization · graph clustering (networkx) · switchback design · exposure mapping · bias quantification · bias-direction reasoning

**Skills demonstrated:** Network experiment design · interference detection · cluster-level analysis · switchback design · platform-specific experiment thinking

---

### 6. `geo-testing` — Geo-Based Experiment Design
> *How do we design a test for something that can't be randomized at the user level?*

Some interventions — pricing changes, app store promotions, marketing campaigns — happen at the market level. Focuses on the design side: how to select and match markets, validate pre-treatment parallel trends, calculate power at the market level (where n is small), and structure the experiment to maximize sensitivity. Compares DMA-level randomization against a synthetic control approach, and runs placebo tests to validate the design.

**Scenario:** Carvo designs a geo experiment to test a new driver incentive structure across regional markets before committing to a national rollout.

**Methods:** DMA-level randomization · market matching · pre-treatment parallel trends validation · power analysis for small n · synthetic control · placebo tests · RMSPE

**Skills demonstrated:** Geo experiment design · market-level power analysis · pre-treatment diagnostics · synthetic control · incrementality framing

---

### 7. `quasi-experiments` — Causal Inference Without Randomization
> *A feature shipped to everyone with no holdout. Did it actually change behavior?*

When you can't run an experiment — a feature already shipped, a policy changed globally, no holdout was kept — quasi-experimental methods are the fallback. Applies regression discontinuity and interrupted time series to the same synthetic scenario, with full assumption diagnostics and honest limitations for each. Shows how the choice of method depends on the data-generating process, not personal preference.

**Scenario:** Nestaway rolled out a new host verification badge to all listings with no control group. Did it change booking rates?

**Methods:** Regression discontinuity · bandwidth selection · ITS · ARIMAX · Bayesian structural time series · assumption diagnostics

**Skills demonstrated:** Quasi-experimental design · causal identification without randomization · method selection judgment · assumption validation · honest limitations

---

### 8. `experiment-interactions` — Concurrent Experiment Interference
> *We're running 40 experiments at once. How do we know they're not affecting each other?*

At scale, experiments don't run in isolation — they overlap in time, share users, and can interact in ways that corrupt both results. Covers how to detect interaction effects between simultaneous experiments, when mutual exclusion (mutex groups) is necessary vs. unnecessarily conservative, and how to design an experiment namespace that scales. Simulates a case where two overlapping experiments produce misleading results and shows how interaction testing would have caught it.

**Scenario:** Meridian's experimentation platform is running concurrent tests on its recommendation algorithm and its notification system — on the same users at the same time.

**Methods:** Factorial interaction testing · mutex group design · experiment namespace architecture · interaction effect detection · 2x2 factorial analysis

**Skills demonstrated:** Experiment platform thinking · concurrent experiment management · interaction detection · mutex group tradeoffs · scaling experimentation infrastructure

---

### 9. `multi-metric-testing` — Multiple Comparisons & Experiment Integrity
> *We're testing 12 metrics. How do we avoid fooling ourselves into thinking something worked?*

Multiple comparisons inflate false positive rates — test enough metrics and something will look significant by chance. Simulates an experiment with a realistic metric suite (primary, secondary, guardrails) and applies Bonferroni, Benjamini-Hochberg, and Bayesian shrinkage to control error rates. Includes a worked example of a "winning" experiment that was a false positive, and shows how each correction method would have caught it.

**Scenario:** Carvo runs a delivery experience experiment with 12 metrics across driver, customer, and operations dimensions.

**Methods:** FWER · FDR · Bonferroni correction · Benjamini-Hochberg · Bayesian shrinkage · hierarchical modeling for multiple outcomes · simultaneous credible intervals

**Skills demonstrated:** Multiple comparisons · experiment integrity · metric hierarchy design · false positive control

---

### 10. `results-communication` — Translating Experiment Results into Decisions
> *The experiment is done. Now what do we actually tell leadership?*

A structured readout template and three worked examples for communicating experiment results to non-technical stakeholders. Takes three synthetic outcomes — a clear win, a clear null, and an ambiguous borderline case — and writes each up as a ship/no-ship recommendation memo. Documents assumptions, risks, financial impact, and what would have to be true for the recommendation to be wrong. Includes a post-experiment ROI documentation template.

**Scenario:** Nestaway tests a new instant booking feature across three outcome profiles.

**Methods:** Effect size framing · practical vs. statistical significance · confidence interval communication · decision framework · sensitivity analysis narrative · risk framing · ROI documentation

**Skills demonstrated:** Stakeholder communication · decision-driving analytics · technical-to-business translation · experiment readout design · financial impact quantification

---

### 11. `case-study` — End-to-End Experiment: All 14 Steps
> *Walk me through how you'd actually run an experiment from business question to ship decision.*

A single realistic experiment narrated through all 14 steps of the experiment lifecycle — from opportunity sizing and OEC design through power analysis, launch, validity checks, CUPED adjustment, results, and financial closeout. Includes four deliberate complications: a data logging bug, a period effect, a platform-level delivery gap, and a SUTVA spillover — each handled explicitly with documented reasoning.

**Scenario:** Carvo tests a real-time "Your Driver is 5 Minutes Away" push notification to reduce failed deliveries and increase 30-day repeat order rate.

**Methods:** Full lifecycle · opportunity sizing · OEC design · power analysis · SRM · CUPED · ITT vs. per-protocol · subgroup analysis · holdout design · ROI projection

**Skills demonstrated:** End-to-end experiment thinking · complication handling · business framing · cross-functional communication · closing the loop on projections

---

## The Experiment Lifecycle

Every project in this repo maps to one or more stages of running a real experiment end-to-end. The table below shows where each step lives across the portfolio.

| # | Step | What it covers | Where to find it |
|---|------|---------------|-----------------|
| 1 | **Business Question & Opportunity Sizing** | Is this worth testing? What's the potential value? | `case-study`, `results-communication` |
| 2 | **Experiment Registration & Brief** | Logging the experiment, locking hypothesis, owner, timeline, success criteria before launch | `ab-framework` — experiment brief template |
| 3 | **Metric Selection & OEC** | Primary KPI, guardrail metrics, composite OEC design, Goodhart's Law traps | `metric-design` |
| 4 | **Power Analysis** | MDE, alpha, target power, required sample size, experiment duration | `ab-framework`, `geo-testing` |
| 5 | **Unit of Randomization** | User vs. session vs. geo vs. cluster — and why it matters for bias | `network-effects`, `geo-testing` |
| 6 | **Experiment Design** | Treatment arms, traffic split, stratification, holdout groups, randomization mechanism | `ab-framework`, `geo-testing`, `experiment-interactions` |
| 7 | **Panel Balancing & Randomization Checks** | Pre-experiment covariate balance, SMD checks, verifying groups are comparable before launch | `ab-framework` — broken experiments module |
| 8 | **Launch & Instrumentation** | Confirming logging, event firing, and data pipeline integrity from day one | `ab-framework` — broken experiments module, `case-study` |
| 9 | **Validity Checks During Runtime** | SRM detection, peeking policy, sequential monitoring without inflating Type I error | `ab-framework`, `bayesian-ab` |
| 10 | **Variance Reduction (CUPED)** | Pre-experiment covariate adjustment, variance reduction %, confidence interval tightening | `cuped` |
| 11 | **Statistical Analysis & Results** | Effect size, confidence intervals, p-values, practical vs. statistical significance | `ab-framework`, `bayesian-ab`, `multi-metric-testing` |
| 12 | **Sensitivity & Subgroup Analysis** | Heterogeneous treatment effects, robustness checks, intent-to-treat vs. per-protocol | `results-communication`, `multi-metric-testing`, `case-study` |
| 13 | **Interpretation & Recommendation** | Ship/no-ship decision with business context, communicating uncertainty to non-technical partners | `results-communication` |
| 14 | **Documentation, Closeout & ROI** | Logging results, financial impact quantification, lessons learned, closing the loop on projections | `results-communication`, `case-study` |

---

## Reading order

If you want to read this repo as a single connected methodology rather than isolated projects:

1. **`metric-design`** — before you run anything, know what you're measuring and why
2. **`ab-framework`** — the full experiment skeleton: power, randomization, validity, results
3. **`cuped`** — make the experiment more efficient without adding users
4. **`bayesian-ab`** — handle the peeking problem and make decisions before the scheduled end date
5. **`network-effects`** — what breaks when users interact, and how to fix the design
6. **`geo-testing`** — when you can't randomize at the user level
7. **`experiment-interactions`** — when you're running many experiments at once
8. **`multi-metric-testing`** — keeping false positives under control across a full metric suite
9. **`quasi-experiments`** — when you couldn't run an experiment at all
10. **`results-communication`** — turning a result into a decision
11. **`case-study`** — all of it, end to end, with complications

---

## Where SUTVA fits

Several projects engage directly with the assumption that treatment and control units don't interfere with each other (SUTVA). When that assumption holds, user-level A/B testing is the cleanest and cheapest approach. When it doesn't, the design has to change — or you live with a known bias.

| Situation | SUTVA status | Recommended design |
|-----------|-------------|-------------------|
| Independent users, no shared resources | ✅ Holds | Standard user-level A/B (`ab-framework`) |
| Social platform, users interact | ❌ Violated | Cluster randomization (`network-effects`) |
| Marketplace, shared supply — bias direction known | ⚠️ Violated but bounded | User-level A/B with documented bias, or switchback (`network-effects`) |
| Platform-wide pricing or campaign | ❌ Not randomizable at user level | Geo-based experiment (`geo-testing`) |
| No holdout available | N/A | Quasi-experiment (`quasi-experiments`) |

Knowing the *direction* of the bias when SUTVA is violated — and deciding whether that's acceptable given the cost of a cleaner design — is one of the most practically important judgment calls in applied experimentation.

---

## Stack

```
Python · PyMC · ArviZ · scipy.stats · statsmodels · networkx
pandas · NumPy · matplotlib · seaborn
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
├── metric-design/
├── network-effects/
├── geo-testing/
├── quasi-experiments/
├── experiment-interactions/
├── multi-metric-testing/
├── results-communication/
└── case-study/
```

Each project folder contains its own README with the business question, modeling decisions, key findings, and honest limitations.

---

## About

Built by Justin — data scientist with 10+ years across retail, supply chain, marketing analytics, finance, and SaaS. Focused on experimentation, causal inference, and Bayesian methods applied to real product and business decisions.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/justindwall)