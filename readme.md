# MarketingScience

A collection of end-to-end marketing data science projects built on realistic synthetic data. Each project tackles a core problem in modern marketing analytics — measurement, attribution, personalization, pricing, and customer value — using the methods and tools used in production at leading retail and e-commerce organizations.

All datasets are synthetically generated to mirror real-world complexity: seasonality, noise, confounding, and uneven sample sizes included.

---

## Why this repo exists

Marketing science sits at the intersection of causal inference, Bayesian modeling, and business strategy. These projects are my way of going deep on the methods that actually move the needle — not toy examples, but realistic problem framings with defensible modeling choices and interpretable outputs.

---

## Projects

### 1. `mmm-bayesian` — Bayesian Marketing Mix Modeling
> *Which channels are actually driving revenue, and how should we reallocate budget?*

Bayesian MMM built in PyMC with adstock decay and saturation curves across five synthetic marketing channels (paid search, social, email, display, TV). Outputs channel-level ROI with full posterior uncertainty and a budget reallocation optimizer.

**Methods:** Adstock transformation · Hill saturation · Bayesian regression · PyMC-Marketing · posterior predictive checks · budget optimization via SciPy

**Skills demonstrated:** MMM architecture · diminishing returns modeling · uncertainty quantification · media planning recommendations

---

### 2. `geo-incrementality` — Geo-Based Incrementality Testing
> *Did this promotion actually cause a sales lift, or would those customers have bought anyway?*

Simulates a regional promotional campaign across synthetic DMAs. Applies synthetic control and difference-in-differences to estimate true incremental lift, then compares against a naive pre/post estimate to show the bias without a proper counterfactual.

**Methods:** Synthetic control · difference-in-differences · parallel trends validation · Bayesian structural time series · CausalImpact-style analysis

**Skills demonstrated:** Geo experiment design · causal identification · counterfactual estimation · communicating lift to non-technical stakeholders

---

### 3. `clv-hierarchical` — Customer Lifetime Value with Hierarchical Bayes
> *Which customers are worth investing in long-term, and how much should we spend to acquire or retain them?*

BG/NBD model for purchase frequency and Gamma-Gamma model for spend, fit on synthetic retail transaction data with realistic patterns: seasonal buyers, one-time purchasers, high-frequency loyalists. Outputs 12-month CLV distributions by customer segment with uncertainty intervals.

**Methods:** BG/NBD · Gamma-Gamma · PyMC · hierarchical priors · partial pooling across segments · posterior CLV distributions

**Skills demonstrated:** Probabilistic customer modeling · retention budget framing · segment-level uncertainty · CLV-to-acquisition-cost comparison

---

### 4. `uplift-promotions` — Dual Uplift Modeling for Promotional Targeting
> *Who should we send this offer to, and who are we just training to wait for discounts?*

Synthetic dataset of customers offered a promotional discount vs. control. Builds a two-model uplift approach alongside a causal forest via EconML. Segments customers into the four classic quadrants: persuadables, sure things, lost causes, sleeping dogs. Outputs a targeting policy with estimated net revenue lift vs. blanket promotion.

**Methods:** S-learner · T-learner · causal forest (EconML) · CATE estimation · Qini curve · uplift decile analysis

**Skills demonstrated:** Heterogeneous treatment effects · promotion ROI framing · targeting policy design · dual uplift methodology

---

### 5. `price-elasticity` — Hierarchical Bayesian Price Elasticity Modeling
> *If we raise the price of this product, what actually happens to demand and total revenue?*

Synthetic SKU-level weekly price and sales data across product categories with realistic cross-price effects and seasonal demand patterns. Fits a hierarchical Bayesian model where elasticity varies by category, season, and price tier. Includes a revenue simulator for "what if we raise price X% on category Y" scenarios.

**Methods:** Hierarchical Bayesian regression · log-log demand model · PyMC · partial pooling across SKUs · posterior predictive revenue simulation

**Skills demonstrated:** Demand modeling · price optimization · category-level elasticity · revenue impact simulation

---

### 6. `segmentation-propensity` — Customer Segmentation & Purchase Propensity
> *Who is likely to buy soon, and how do we prioritize outreach across a large customer base?*

Synthetic CRM dataset with purchase history, loyalty tier, channel engagement, and recency signals. RFM-based segmentation combined with a hierarchical propensity model for likelihood to purchase in the next 30 days. Thin-data customers borrow strength from similar segments via partial pooling.

**Methods:** RFM segmentation · hierarchical logistic regression · PyMC · partial pooling · calibration curves · ranking by expected incremental value

**Skills demonstrated:** Customer segmentation · propensity modeling · personalization targeting · handling sparse customer histories

---

### 7. `mta-attribution` — Multi-Touch Attribution
> *Which touchpoints in the customer journey deserve credit for the conversion?*

Synthetic customer journey data with sequences of marketing touchpoints leading to purchase or no purchase. Builds Markov chain attribution and Shapley value attribution, compares both against last-touch as the naive baseline. Shows how budget decisions change under each model.

**Methods:** Markov chain attribution · Shapley values · transition matrix estimation · removal effect · comparison vs. last-touch and first-touch baselines

**Skills demonstrated:** Attribution modeling · customer journey analysis · channel credit allocation · translating attribution to budget decisions

---

### 8. `promo-cannibalization` — Promotion Cannibalization & Halo Effects
> *When we discount one product, does it lift the whole category or just steal sales from adjacent SKUs?*

Synthetic transaction data across a product category during a promotional event. Models cross-SKU and cross-category effects to estimate net category lift, cannibalization rate, and halo lift on adjacent products. Often the story is more nuanced than the headline promo lift number.

**Methods:** Difference-in-differences · synthetic control · cross-elasticity estimation · hierarchical Bayesian regression · category-level net lift

**Skills demonstrated:** Promotion measurement · cannibalization modeling · category management analytics · retail-specific causal framing

---

### 9. `demand-forecasting` — Hierarchical Demand Forecasting
> *How many units will we sell next week, and how uncertain should we be about that?*

Synthetic SKU-store-week sales data with realistic seasonality, trend, promotional spikes, and stockout patterns. Fits a hierarchical time series model so low-volume SKUs borrow signal from category-level trends. Outputs probabilistic forecasts with credible intervals rather than point estimates.

**Methods:** Hierarchical time series · Prophet · statsforecast · Bayesian structural time series · partial pooling across SKU hierarchy · quantile forecasting

**Skills demonstrated:** Demand forecasting · retail hierarchy modeling · probabilistic forecasting · inventory decision framing

---

### 10. `ab-testing-framework` — Experimentation Framework & Best Practices
> *Did this change we made actually work, and how do we know we're not fooling ourselves?*

A reusable experimentation framework covering the full lifecycle: power calculation, randomization, sequential monitoring, and results readout. Includes a "bad experiments museum" — deliberately broken tests (underpowered, peeked at early, wrong randomization unit) with annotated diagnoses.

**Methods:** Power analysis · t-test · Mann-Whitney · Bayesian A/B · sequential testing · CUPED · multiple comparisons correction

**Skills demonstrated:** Experiment design · statistical rigor · framework thinking · communicating results to non-technical partners

---

## Stack

```
Python · PyMC · ArviZ · EconML · statsmodels · scikit-learn
pandas · NumPy · SciPy · matplotlib · seaborn
Snowflake · SQL · Jupyter
```

---

## Structure

```
MarketingScience/
├── mmm-bayesian/
│   ├── data/                  # synthetic data generation script
│   ├── notebooks/             # step-by-step analysis
│   ├── src/                   # reusable modeling code
│   └── README.md              # project-level writeup
├── geo-incrementality/
├── clv-hierarchical/
├── uplift-promotions/
├── price-elasticity/
├── segmentation-propensity/
├── mta-attribution/
├── promo-cannibalization/
├── demand-forecasting/
└── ab-testing-framework/
```

Each project folder contains its own README with the business question, modeling decisions, key findings, and limitations.

---

## About

Built by Justin — data scientist with 10+ years across retail, supply chain, marketing analytics, finance, and SaaS. Focused on Bayesian inference, causal inference, and building models that inform real decisions.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-connect-blue?style=flat&logo=linkedin)](https://linkedin.com/in/justindwall)