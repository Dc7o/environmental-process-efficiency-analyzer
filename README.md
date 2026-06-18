# Environmental Process Efficiency Analyzer

A end-to-end data analytics project applying the **DMAIC (Define, Measure, Analyze, Improve, Control)** framework to identify drivers of energy consumption and validate process efficiency gains in a manufacturing facility.

Built as part of a Lean Six Sigma Green Belt portfolio, this project demonstrates SQL data engineering, Python statistical analysis, and Tableau dashboarding across a unified ESG-focused workflow.

---

## Business Question

> **Which operational and environmental factors drive energy consumption in a manufacturing facility, and did the 2024 process improvement intervention reduce energy and water usage without impacting production output?**

This question sits at the intersection of operational efficiency and ESG reporting — identifying energy drivers enables targeted reduction strategies that lower both cost and carbon footprint.

---

## DMAIC Framework

| Phase | Question | Method |
|---|---|---|
| **Define** | What problem are we solving? | Business question scoping, KPI identification |
| **Measure** | What is currently happening? | SQL data extraction, time-series visualization |
| **Analyze** | What is causing it? | Correlation matrix, simple & multiple linear regression, VIF, standardized betas |
| **Improve** | Did the intervention work? | Pre vs Post comparison, statistical validation |
| **Control** | How do we sustain it? | DMAIC Tableau dashboard for ongoing monitoring |

---

## Tech Stack

| Tool | Purpose |
|---|---|
| **MySQL** | Data storage, extraction, CTEs, window functions |
| **Python** | Statistical analysis — regression, correlation, VIF |
| **statsmodels** | OLS regression, standardized betas |
| **scipy** | Pearson correlation matrix with p-values |
| **seaborn / matplotlib** | Correlation heatmap, scatter matrix, regression diagnostics |
| **Tableau Public** | Interactive DMAIC dashboard |

---

## Dataset

Synthetic manufacturing dataset simulating one facility, three production lines (Line A, B, C), across January–November 2024 with a built-in pre/post process improvement phase split at July 2024.

| Table | Description |
|---|---|
| 1,000 rows | Daily shift-level operational records |
| 17 columns | Date, production line, shift, output, energy, water, emissions (BOD, COD, NOx, SO₂, PM2.5), breach flags |

---

## Key Statistical Findings

### Block 1 — Simple Linear Regression
- **DV:** Energy (kWh) | **IV:** Production Output (kg)
- **R² = 0.000 | p = 0.988** — No significant relationship
- Output alone explains 0% of energy variance

### Block 2 — Multiple Linear Regression
- **DV:** Energy (kWh) | **IVs:** All 7 operational predictors
- **R² = 0.117 | F p < 0.001** — Model is significant
- Only **water usage** (β* = 0.343, p < .001) and **output** (β* = 0.307, p < .001) are significant predictors

### Why Block 1 Failed — Suppression Effect
Output and water usage are negatively correlated (r = −0.660) due to the pre/post improvement phase structure. Higher output rows are post-improvement rows which also have lower water usage — creating a suppressor effect that masked output's true relationship with energy in Block 1. Block 2 removed this suppression by controlling for water simultaneously.

### VIF Results
All predictors scored VIF < 5 — no multicollinearity concern confirmed.

### Correlation Matrix Key Findings
| Pair | r | Interpretation |
|---|---|---|
| water_usage ↔ energy | +0.250 | More water = more energy ✓ |
| cod_mgl ↔ water_usage | +0.770 | High COD and water co-occur |
| output_kg ↔ water_usage | −0.660 | Suppressor — driven by phase structure |
| output_kg ↔ energy | −0.000 | Masked by suppression |

---

## Pre vs Post Improvement Results

| Metric | Pre | Post | Change |
|---|---|---|---|
| Avg Energy (kWh) | 2,473 | 2,182 | **↓ 11.8%** |
| Avg Water (litres) | 12,425 | 9,701 | **↓ 21.9%** |
| Avg Output (kg) | 741.7 | 740.9 | **≈ Stable** |

The facility reduced energy and water consumption significantly while maintaining production output — confirming the process improvement was efficiency-driven, not volume-driven.

---

## Tableau Dashboard

**[View Live Dashboard on Tableau Public →](#)**

Three-chart DMAIC dashboard:
- **MEASURE** — Energy consumption trend by production line with July 2024 improvement phase marker
- **ANALYZE** — Water usage vs energy scatter plot by improvement phase with trend lines
- **IMPROVE** — Pre vs Post average energy and water usage comparison

---

## Project Structure

```
├── README.md
├── data/
│   └── env_spss.csv
├── sql/
│   └── environmental_analysis.sql
├── python/
│   └── env_analysis.py
└── outputs/
    ├── block1_simple_regression.png
    ├── block2_regression_diagnostics.png
    ├── correlation_heatmap.png
    └── scatter_matrix.png
```

---

## Analytical Limitations

- R² = 0.117 indicates the model explains 11.7% of energy variance — additional unmeasured factors (equipment age, ambient temperature, operator behavior, maintenance cycles) likely account for the remaining 88.3%
- Dataset is synthetic — the negative correlation between output and water usage is an artifact of the phase structure, not a real operational relationship
- Correlation matrix EDA should precede IV selection in future analyses to avoid suppressor effects

---

## Author

**Dhvani Chaudhari**
Dual Master's — Environmental Science & MBA Business Analytics
Lean Six Sigma Green Belt (in progress)

[GitHub](https://github.com/Dc7o) | [Tableau Public](#) | [Portfolio](#)
