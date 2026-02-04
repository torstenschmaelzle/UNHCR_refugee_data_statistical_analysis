# UNHCR Refugee Flows – Data Science & Statistical Analysis

This repository analyzes global refugee and asylum seeker flows using UNHCR time series data (1951–2016), enriched with World Bank indicators (population and GDP per capita). The focus is a clean end-to-end workflow: data preparation, feature engineering, exploratory analysis, geospatial visualization, hypothesis testing, regression, and fixed-effects panel modeling.

## Quick preview of results

### Flow visualizations (GIFs)

<p>
  <img src="Output_gifs/global_flowmap_2016.gif" width="98%" />
</p>

<p float="left">
  <img src="Output_gifs/outflow_heatmap.gif" width="49%" />
  <img src="Output_gifs/inflow_heatmap.gif" width="49%" />
</p>

**What you’re seeing:**

- The **global flow map** visualizes directed movements between origins and destinations for a selected year.
- The **inflow/outflow heatmaps** show relative intensity of refugee movements by country.

### Fixed-effects panel model outputs (PNG)

<p float="left">
  <img src="output/Panelmodell_Flüchtlingsaufnahme.png" width="49%" />
  <img src="output/Panelmodell_Flüchtlingsabwanderung.png" width="49%" />
</p>

**Interpretation (high-level):**

- **Left:** within-country changes over time in refugee _intake_ per 1,000 inhabitants.
- **Right:** within-country changes over time in refugee _outflow_ per 1,000 inhabitants.  
  Fixed effects isolate trends **within the same country** over time, instead of mixing cross-country level differences into the estimate.

## Core questions

1. **Earlier vs. later decades:** How do refugee and asylum seeker flows change over time?
2. **Wealth and intake:** Is there an association between GDP per capita and refugee intake per capita?

---

## Data sources

### UNHCR (via Kaggle, stored in `archive/`)

- `time_series.csv` (main dataset, long format, 1951–2016)
- additional datasets for context/EDA:  
  `persons_of_concern`, `asylum_seekers`, `asylum_seekers_monthly`, `demographics`, `resettlement`

### World Bank (stored in `additional_data/`)

- population totals by country-year
- GDP per capita by country-year

---

## Key metric (feature engineering)

### Why normalization is necessary

Absolute refugee counts are not comparable across countries because population size strongly influences the level of flows. A fair comparison requires a denominator.

### Metric used

A per-capita metric is computed:

**share_per_1000 = (refugees / population) × 1000**

This makes country-year values comparable even when population sizes differ by orders of magnitude.

---

## GDP enrichment (why and how)

### Why include GDP per capita

GDP per capita is used as a broad proxy for economic capacity and living standard. The intention is not to claim causality, but to test whether there is an empirical association between relative intake and economic context.

### How GDP is joined to the flow metric

High-level steps:

1. Aggregate refugee (and asylum seeker) flows by **country-year**
2. Load World Bank GDP per capita by **country-year**
3. Harmonize country identifiers (name mapping where required)
4. Merge on `(country, year)` and keep the computed `share_per_1000`
5. Use the merged dataset for regression and panel modeling

This produces datasets such as:

- `Destination_refugees_per_capita_with_gdp.csv`
- `output_with_continent.csv` (for continent-based comparisons)

---

## Important design choices (and the reasoning)

### Missing values

In UNHCR flow data, missingness is often **structural** and not missing at random:

- origins can be `Various/Unknown`
- some country-year combinations are partially reported
- coverage varies by dataset and period

What was done:

- missingness is inspected explicitly (`EDA/Missing_Values.ipynb`)
- naive imputation is avoided (mean/median imputation is not meaningful for origins or flows)
- some analyses are limited to population types and time windows where interpretation is strongest
- rows are filtered only when a method requires complete cases

Why:
Imputation can fabricate flows that never existed and bias downstream inference. In this domain, preserving uncertainty is often more honest than filling unknowns.

### Outliers

Refugee flows are naturally **heavy-tailed** and driven by true shocks (conflicts, crises). Extreme values are often the signal rather than noise.

What was done:

- no default outlier removal
- per-capita normalization is used to reduce country-size effects
- interpretation is preferred over deletion

Why:
Removing outliers risks removing the most meaningful events in the data. A better approach (if needed) is robustness checks or alternative transformations, not automatic deletion.

### Scope restrictions

For cross-border flow interpretation, the core analyses focus on:

- **Refugees** (including refugee-like situations)
- **Asylum seekers**

Groups like internally displaced persons and returnees are excluded from the main flow interpretation because they do not represent cross-border movement in the same way.

---

## Repository structure

```text
archive/                 UNHCR datasets (raw)
additional_data/         World Bank population and GDP per capita
EDA/                     exploratory notebooks and missingness analysis
Hypothesen/              hypothesis notebooks and short READMEs
output_csv_files/        processed tables used for maps and models
Output_gifs/             exported animated visualizations (GIFs)
output/                  exported model figures (PNGs)

Regression.ipynb         regression analysis
Panelmodell.ipynb        fixed effects panel model
Static_flowmaps.ipynb    static maps
Dynamic_Flowmaps.ipynb   animated maps and GIF export
geodata_preperation.ipynb geodata preprocessing
Time_Series.ipynb        time series analysis
```

## Contact

Torsten Schmälzle
GitHub: https://github.com/torstenschmaelzle

```

```
