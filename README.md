# MGNREGA Theory of Change Monitor

An interactive dashboard, written in R, that organises state-level MGNREGA data (15 states, FY 2022-23 to 2025-26) around a five-level **theory of change** and tests how far the data can support each link in the chain.

**Live dashboard:** `https://ananyarathour09.github.io/mgnrega-theory-of-change-monitor/` 


## 1. The proposed theory of change

| Level | Elements |
|---|---|
| **01 Inputs** | Government funding and approved labour budgets; administrative staff and Panchayat capacity; worksite infrastructure; technical and institutional support |
| **02 Activities** | Registration and job card issuance; identification and planning of works; processing employment demand; allocation of work; execution of public works; processing wage payments |
| **03 Outputs** | Households and individuals employed; person-days generated; women's share of person-days; SC/ST participation; works completed |
| **04 Outcomes** | Greater access to wage employment; increased women's participation; more sustained employment opportunities; timelier wage processing; improved implementation efficiency |
| **05 Long-term impact** | Improved rural livelihood security; greater economic participation of women; improved household economic security; strengthened rural asset base |

These are **proposed causal pathways, not findings established by the dataset.** Long-term impacts need household and asset-level evidence.

## 2. What the dashboard contains

1. **Programme logic and evidence map.** Each element of the theory of change is marked *observed directly*, *proxy only* or *not in dataset*, with the indicator used and its FY 2022-23 and FY 2025-26 values (searchable table).
2. **Indicators by level.** For every level: a national view, a state explorer (dropdown to switch indicator, click legend to hide or isolate states) and a state × year heatmap.
3. **Outcome benchmark gap.** Attainment of scheme norms (100 days, 33% women, 6% administration cap, 15-day payments) with year buttons.
4. **Testing the links in the chain.**
   - Animated cross-state bubble chart (budget achieved vs days per household).
   - Correlation matrix ordered along the chain.
   - Two-way fixed-effects regressions (state and year effects, state-clustered standard errors, standardised coefficients).
   - Decomposition of the change in person-days into a households (extensive) effect and a days-per-household (intensive) effect, using the log-mean Divisia method.
   - Sigma convergence (coefficient of variation across states) and beta convergence (initial level vs change).
5. **State profiles.** Radar chart of each state's percentile position and a full data table.

## 3. How to run it

1. Install R and RStudio.
2. Keep this folder structure:
   ```
   mgnrega-toc-monitor/
   ├── MGNREGA_ToC_dashboard.Rmd    (the dashboard code)
   ├── build_dashboard.R            (installs packages and builds the page)
   ├── index.html                   (the built dashboard)
   ├── data/MGNREGA_Master_Dataset.xlsx
   └── screenshots/
   ```
3. In RStudio, open `MGNREGA_ToC_dashboard.Rmd` and click **Knit**, or run `source("build_dashboard.R")`.
4. Settings are at the top of the Rmd (chunk `setup`):
   - `include_wb <- FALSE` keeps West Bengal out (flagged source data); set `TRUE` to include it.
   - `first_yr` and `latest` set the comparison years.

R packages: readxl, dplyr, tidyr, plotly, DT, htmltools, sandwich, scales, knitr, rmarkdown.

## 4. Publish on GitHub Pages

1. Create a public repository and upload all files (including `index.html`).
2. Go to **Settings → Pages**, choose **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
3. After a minute your dashboard is live at `https://YOUR-USERNAME.github.io/REPO-NAME/`.

## 5. Method notes

- **Data:** MGNREGA Master Dataset (state-at-a-glance MIS), 15 states × 4 financial years = 60 state-year rows.
- **Aggregation:** amounts are summed across states. Women, SC, ST share and wage rate are weighted by person-days. Payments within 15 days, e-FMS share and agriculture share are weighted by expenditure. Budget achieved, days per household, utilisation, completion rate and administration share are recomputed from the summed amounts.
- **Budget units:** the workbook labels the approved labour budget as ₹ lakh, but it reconciles exactly with person-days in lakh (person-days ÷ budget = "% of total labour budget"), so it is treated as lakh person-days.
- **West Bengal** is excluded by default: the source shows zero approved budgets in all four years and zero employment indicators in FY 2024-25 and 2025-26. Verify against the original MIS downloads before including it.
- **Nominal values:** wages and expenditure are in current rupees, not adjusted for inflation.
- **Values above 100%** (utilisation, budget achieved) are kept as reported.
- **Norms** used for the benchmark view (100 days per household, 33% women, 6% administration cap, 15-day payment rule) are scheme rules, not values from the workbook. Check them against the MGNREGA guidelines.

## 6. Limits of the evidence

- The panel is state-level administrative data. It does **not** observe job-card registration, employment demand, worksite infrastructure, staff capacity or any household-level outcome. Those links are shown as gaps, not filled with weak proxies.
- Impact indicators (wages per household, agriculture and allied spend, completed works) are proxies and say nothing about income, asset quality or use.
- With about 14 states and 4 years, the regressions are small-sample and descriptive. Omitted factors (state politics, demand, rainfall, administrative reform) are not controlled. Read them as consistency checks on the proposed logic, not as causal estimates.
- Demand-side data (district-level employment demand tables) were not part of this panel.

## 7. Selected descriptive results (14 states, FY 2025-26 vs FY 2022-23)

- Person-days generated fell from 23,865 to 18,320 lakh (-23.2%), with budget achieved at 93.5% in FY 2025-26.
- Average days per household fell from 48.0 to 42.0, against the 100-day guarantee.
- Women's share of person-days was 57.6%, well above the 33% minimum.
- Payments generated within 15 days rose from 93.8% to 96.4%.
- Of the 5,546 lakh fall in person-days across states, about 2,920 lakh came from fewer households working and about 2,626 lakh from fewer days per household.

## Built with
R, plotly, DT, rmarkdown.
