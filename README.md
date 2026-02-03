# UNHCR Refugee Flows – Data Science & Statistical Analysis

This repository analyzes global refugee and asylum seeker flows using UNHCR data (1951–2016) enriched with World Bank indicators (population and GDP per capita).  
The goal is to demonstrate practical data science and statistics skills on real-world, messy data: data preparation, feature engineering, exploratory analysis, visualization, hypothesis testing, and panel/regression modeling.

## What this shows (for recruiters)

- **Data wrangling & joins:** multi-source joins (UNHCR + World Bank + GeoJSON), country name harmonization
- **Feature engineering:** normalization to compare countries fairly (per 1,000 inhabitants)
- **EDA & visualization:** time trends, maps, flowmaps, GIF exports
- **Statistical reasoning:** hypothesis testing, regression, **fixed-effects panel modeling**
- **Responsible modeling decisions:** transparent handling of missing data and outliers (see below)

## Quick preview (outputs)

### Global flow visualization

![Global flow map 2016](Output_gifs/global_flowmap_2016.gif)

### Inflow and outflow intensity

![Inflow heatmap](Output_gifs/inflow_heatmap.gif)
![Outflow heatmap](Output_gifs/outflow_heatmap.gif)

## Core questions

1. **Earlier vs. later decades:** How do refugee / asylum seeker flows change over time?
2. **Wealth and intake:** Is there an association between GDP per capita and refugee intake per capita?

## Data sources

### UNHCR (via Kaggle, stored in `archive/`)

- `time_series.csv` (main dataset, long format, 1951–2016)  
  One row per year, destination, origin, and population type (e.g., Refugees).
- Additional datasets used for context/EDA:
  - `persons_of_concern.csv`
  - `asylum_seekers.csv`
  - `asylum_seekers_monthly.csv`
  - `demographics.csv`
  - `resettlement.csv`

### World Bank (stored in `additional_data/`)

- Population totals per country-year
- GDP per capita per country-year

## Key metric (feature engineering)

Absolute refugee counts are strongly driven by country size.  
To make countries comparable, we compute:

**refugee_share_per_1000 = (refugees / population) \* 1000**

This allows meaningful comparison across countries with very different population sizes and highlights cases where smaller countries carry high relative inflows.

## Methodology overview

### 1) Data preparation

- Cleaning and harmonizing country identifiers for joins across:
  - UNHCR tables
  - World Bank tables
  - GeoJSON world geometries (for maps)
- Aggregation of origin/destination flows by country-year
- Geodata preparation (centroids) for flow visualization

### 2) Exploratory analysis (EDA)

- Coverage checks (years, countries, population types)
- Distribution inspection (heavy tails, skewness)
- Trend exploration by year, origin, destination

### 3) Visualization

- Static maps and flow arrows
- Animated flow maps and heatmaps exported as GIFs (`Output_gifs/`)

### 4) Statistical analysis

- Hypothesis testing (including permutation style approaches where appropriate)
- Linear regression for trends and simple relationships
- **Fixed-effects panel model** to estimate within-country changes over time while controlling for time-invariant country factors

## Important modeling choices (reasoning)

Real-world refugee flow data is not “textbook clean.” We made decisions to avoid misleading conclusions.

### Missing data (we did not blindly drop or impute)

In UNHCR flow data, missingness is often **structural** (not random), for example:

- origin is listed as **Various / Unknown**
- some country-year combinations have incomplete reporting

What we did:

- explicitly inspected missingness (`EDA/Missing_Values.ipynb`)
- avoided naive imputation (median/mean imputation is not meaningful for origins/flows)
- restricted certain analyses to population types/time windows where interpretation is strongest
- used filtering only when a specific method required complete cases

Why:
Imputation can create artificial flows and bias regression/panel estimates. In this setting, preserving uncertainty is often more honest than “filling” values.

### Outliers (we did not remove them by default)

Refugee flows are naturally **heavy-tailed**. Large values are frequently **true signals** caused by conflicts and geopolitical shocks, not measurement errors.

What we did:

- no default outlier deletion
- normalization per 1,000 inhabitants to reduce country-size effects
- interpretation focused on robustness and context rather than forcing normality

Why:
Removing outliers in this domain can remove the phenomenon we want to study. For sensitive models, the right approach is usually sensitivity checks (e.g., transforms or winsorization) rather than deleting high-impact years.

### Scope decisions

For interpretability of cross-border flows, the main analyses focus on:

- **Refugees** (including refugee-like situations)
- **Asylum seekers**

Groups like internally displaced persons or returnees were excluded from the main flow interpretation because they do not represent cross-border movement in the same way.

## Repository structure
