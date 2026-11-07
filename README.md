<!-- README for the Bayesian Metaverse website / GitHub organisation profile -->
<!-- Hex paths assume images are stored in man/figures/hex/ — replace with final paths -->

<p align="center">
  <img src="man/figures/hex/bayesmetaverse.png" alt="Bayesian Metaverse logo" width="160">
</p>

<h1 align="center">Bayesian Metaverse</h1>

<p align="center"><em>Tools, methods and resources for Bayesian meta-analysis.</em></p>

<p align="center">
  <img src="man/figures/hex/entry.png"       alt="data entry package hex" height="110">
  <img src="man/figures/hex/inspectR.png"    alt="inspectR hex"           height="110">
  <img src="man/figures/hex/robbR.png"       alt="robbR hex"              height="110">
  <img src="man/figures/hex/bayesma.png"     alt="bayesma hex"            height="110">
  <img src="man/figures/hex/bayesfoRest.png" alt="bayesfoRest hex"        height="110">
  <img src="man/figures/hex/bayestsa.png"    alt="Bayesian TSA hex"       height="110">
</p>

---

## Overview

Bayesian Metaverse brings together an R package suite, practical guidance and links to established software to support a transparent, reproducible workflow for Bayesian meta-analysis (Bayesian MA). The aim is to help researchers move from a clearly specified review question through data preparation, model fitting and checking, sensitivity analysis, and interpretation.

The project is organised around a **workflow rather than a single model**. Different reviews need different likelihoods, effect measures, priors and sensitivity analyses; the workflow makes those choices explicit and documents why they were made. It builds on the [bayesma workflow diagram](https://blmoran.github.io/bayesma/articles/workflow-diagram.html).

---

## The package suite

| Hex | Package | Workflow stage | Purpose | Status |
|:---:|:--------|:---------------|:--------|:-------|
| <img src="man/figures/hex/entry.png" width="60"> | **entRy** *(name TBC)* | Data preparation | Shiny app for structured data extraction and entry (one row per study; binary, continuous or count outcomes), exporting analysis-ready data. | In development · [preview](https://blmoran.github.io/bayesma/articles/data-entry.html) |
| <img src="man/figures/hex/inspectR.png" width="60"> | **inspectR** | Trustworthiness | Assess and document study trustworthiness using the [INSPECT-SR](https://doi.org/10.1136/bmj-2024-082233) framework. Complements, but does not replace, risk-of-bias assessment. | In development |
| <img src="man/figures/hex/robbR.png" width="60"> | **robbR** | Risk of bias | Create and present risk-of-bias visualisations (traffic-light and summary plots). | Available · repository TBA |
| <img src="man/figures/hex/bayesma.png" width="60"> | **bayesma** | Modelling | Fit, compare, check and interpret Bayesian meta-analysis models in R using Stan. | Available · [docs](https://blmoran.github.io/bayesma/) |
| <img src="man/figures/hex/bayesfoRest.png" width="60"> | **bayesfoRest** | Visualisation | Forest plots and other displays that bring posterior results and study-level information together. | Available · [docs](https://blmoran.github.io/bayesfoRest/) |
| <img src="man/figures/hex/bayestsa.png" width="60"> | **Bayesian TSA** *(name TBC)* | Sequential evidence | Bayesian sequential assessment of accumulating meta-analytic evidence, informed by Spence, Steinsaltz & Fanshawe (2016). | In development |

The packages are designed to work together while remaining useful as standalone tools.

---

## Installation

Development versions of the available packages can be installed from GitHub:

```r
install.packages("remotes")

remotes::install_github("BLMoran/bayesma")
remotes::install_github("BLMoran/bayesfoRest")
```

<!-- Future: a meta-package that installs and attaches the suite, e.g.
```r
remotes::install_github("bayesian-metaverse/bayesmetaverse")
library(bayesmetaverse)
```
-->

---

## A principled Bayesian meta-analysis workflow

Each stage lists the package(s) you would typically use. Grey nodes are related external software.

```mermaid
flowchart TD
    classDef bmv fill:#2C5F8A,stroke:#1B3D5A,color:#fff
    classDef ext fill:#E6E6E6,stroke:#999,color:#333
    classDef stage fill:#fff,stroke:#2C5F8A,color:#1B3D5A

    Q[/"Review question & protocol<br/>(PICO, effect measure, priors pre-specified)"/]:::stage

    S1["1 · Data preparation"]:::stage
    S2["2 · Trustworthiness assessment<br/>INSPECT-SR"]:::stage
    S3["3 · Risk of bias assessment"]:::stage
    S4["4 · Preliminary exploration<br/>PRIMED"]:::stage
    S5["5 · Model specification<br/>likelihood · priors (μ, τ) · RE distribution<br/>prior predictive checks"]:::stage
    S6["6 · Model fitting<br/>Stan MCMC"]:::stage
    S7["7 · Diagnostics<br/>R̂ · ESS · trace · posterior predictive"]:::stage
    S8["8 · Model comparison<br/>LOO / LOSO-CV · ELPD · CRPS"]:::stage
    S9["9 · Small-study & publication bias<br/>selection models · PET-PEESE · RoBMA"]:::stage
    S10["10 · Heterogeneity<br/>τ posterior · predictive distribution"]:::stage
    S11["11 · Subgroups & meta-regression"]:::stage
    S12["12 · Sensitivity analysis<br/>priors · exclusions · RE distribution"]:::stage
    S13["13 · Sequential evidence<br/>Bayesian TSA"]:::stage
    S14["14 · Interpretation & reporting<br/>posterior · CrI · P(direction) · BF"]:::stage

    Q --> S1 --> S2 --> S3 --> S4 --> S5 --> S6 --> S7
    S7 -- "fails" --> S5
    S7 --> S8 --> S9 --> S10 --> S11 --> S12 --> S13 --> S14

    P1["entRy (TBC)"]:::bmv --- S1
    P2["inspectR"]:::bmv --- S2
    X2["scrutiny"]:::ext -.- S2
    P3["robbR"]:::bmv --- S3
    P5["bayesma"]:::bmv --- S5 & S6 & S7 & S8 & S10 & S11 & S12
    X5["brms · Stan · bayesmeta · MetaStan"]:::ext -.- S6
    X9["RoBMA"]:::ext -.- S9
    P5 --- S9
    P13["Bayesian TSA (TBC)"]:::bmv --- S13
    P14["bayesfoRest"]:::bmv --- S10 & S11 & S12 & S14
```

> A static version of the diagram (with hexes in place of node labels) can be swapped in once the artwork is final:
> `<img src="man/figures/workflow.png" alt="Bayesian Metaverse workflow">`

### Stage → package lookup

| Stage | Bayesian Metaverse | Key functions | Related software |
|:------|:-------------------|:--------------|:-----------------|
| Data preparation | entRy *(TBC)* | `launch_data_entry()` | — |
| Trustworthiness | inspectR | `inspect_sr()`, `inspect_sr_table()`, `inspect_sr_plot()`, `filter_trustworthy()` | scrutiny |
| Risk of bias | robbR | `rob_plot()` | — |
| Preliminary exploration | bayesma | `primed()` | — |
| Model specification | bayesma | `bayesma(sample_prior = TRUE)`, `pp_check()` | brms |
| Model fitting | bayesma | `bayesma()` | Stan, brms, bayesmeta, MetaStan |
| Diagnostics | bayesma | `diagnostics()`, `pp_check()` | — |
| Model comparison | bayesma | `compare_models()`, `compare_table()`, `compare_plot()` | — |
| Publication bias | bayesma | `egger()`, `funnel_plot()`, `robma()` | RoBMA |
| Heterogeneity | bayesma, bayesfoRest | `bayesma(re_dist = "mixture")`, `forest()` | bayesmeta |
| Subgroups / meta-regression | bayesma, bayesfoRest | `meta_reg()`, `metareg_mod_plot()`, `forest(subgroup = TRUE)` | brms |
| Sensitivity analysis | bayesma, bayesfoRest | `sensitivity_plot()` | — |
| Sequential evidence | Bayesian TSA *(TBC)* | TBA | — |
| Interpretation & reporting | bayesma, bayesfoRest | `interpret()`, `overall_plot()`, `forest()` | — |

---

## Related software

The Bayesian Metaverse builds on, and is designed to sit alongside, established tools. These packages are maintained independently by their authors and are listed here with permission. <!-- confirm permission before publishing -->

| Hex | Package | Description | Link |
|:---:|:--------|:------------|:-----|
| <img src="man/figures/hex/stan.png" width="60"> | **Stan** | Probabilistic programming language for Bayesian inference; the engine behind bayesma. | [mc-stan.org](https://mc-stan.org/) |
| <img src="man/figures/hex/brms.png" width="60"> | **brms** | Bayesian (multilevel) regression models via Stan using formula syntax; flexible for bespoke meta-analytic models. | [paul-buerkner.github.io/brms](https://paul-buerkner.github.io/brms/) |
| <img src="man/figures/hex/bayesmeta.png" width="60"> | **bayesmeta** | Fast, semi-analytical Bayesian random-effects meta-analysis for the normal–normal hierarchical model. | [CRAN](https://cran.r-project.org/package=bayesmeta) |
| <img src="man/figures/hex/metastan.png" width="60"> | **MetaStan** | Bayesian meta-analysis via Stan, including binomial–normal models and model-based meta-analysis. | [CRAN](https://cran.r-project.org/package=MetaStan) |
| <img src="man/figures/hex/robma.png" width="60"> | **RoBMA** | Robust Bayesian meta-analysis: Bayesian model averaging over effect, heterogeneity and publication bias models. | [CRAN](https://cran.r-project.org/package=RoBMA) |
| <img src="man/figures/hex/scrutiny.png" width="60"> | **scrutiny** | Error detection in summary statistics (GRIM, GRIMMER, DEBIT); supports trustworthiness checks. | [lhdjung.github.io/scrutiny](https://lhdjung.github.io/scrutiny/) |

---

## Guidance and resources

- **Workflow** — [Bayesian MA workflow diagram](https://blmoran.github.io/bayesma/articles/workflow-diagram.html)
- **Data entry** — [Data entry app](https://blmoran.github.io/bayesma/articles/data-entry.html)
- **Trustworthiness** — INSPECT-SR: Wilkinson J, et al. *BMJ* (2025)
- **Sequential analysis** — Spence GT, Steinsaltz D, Fanshawe TR. A Bayesian approach to sequential meta-analysis. *Stat Med*. 2016;35(29):5356–5375.
- **Further reading** — *(add tutorials, reporting guidelines, e.g. ROBUST/BayesMA reporting checklists)*

---

## Contributing

Contributions, issues and feature requests are welcome. Please open an issue in the relevant package repository, or start a discussion in the [Bayesian Metaverse organisation](https://github.com/bayesian-metaverse).

## Citation

If you use the Bayesian Metaverse packages, please cite the individual packages you use (`citation("bayesma")`) and the underlying methods papers.

## Authors

Benjamin Moran · Thomas Payne · *(add contributors)*

## Licence

*(add licence)*
