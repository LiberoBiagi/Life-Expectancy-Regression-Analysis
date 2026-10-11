# 🌍 Beyond Wealth: Determinants of Global Life Expectancy in 2022

**Cross-country regression analysis of life expectancy in the post-pandemic world**
Statistics project in R · NOVA Information Management School (NOVA IMS) · 2025–2026
**Authors:** Libero Biagi · Moritz Baldauf · Marcell Hajdú

---

## 📌 Overview

Wealth matters for health (the *Preston Curve*), but it is not the only determinant of how long people live. This project asks:

> **How did economic, institutional and demographic factors influence global life expectancy in the 2022 post-pandemic landscape?**

We build a cross-sectional dataset of countries from [Our World in Data](https://ourworldindata.org/), estimate multiple linear regressions, run diagnostic tests, and map where observed life expectancy diverges from the model's prediction.

**Main findings**
- **HDI** (positive) and **fertility rate** (negative) are the significant predictors at the 5% level.
- GDP per capita, water access, polio vaccination and political regime are not significant once HDI is controlled for.
- Democracies cluster at the high-longevity end, but the direct effect of regime is absorbed by HDI.
- Cross-sectional data cannot establish causality (lower fertility may also be a consequence of longer life).

## 🗂️ Data

Country-level data for **2022**, the most recent year in which all variables were available, from several Our World in Data indicators joined by country. Only entities with an ISO code are kept (world aggregates are removed) and countries with missing values are dropped.

| Theory | Variable | Description |
|---|---|---|
| Outcome | `lifeexp` | Life expectancy at birth (years) |
| Demography | `fertility` | Fertility rate |
| Economic | `gdppercap` | GDP per capita (Maddison Project) |
| Human capital | `hdi` | Human Development Index |
| Infrastructure | `water` | % of population with at least basic drinking water |
| Institutions | `regime` | Political regime, factor: 0 closed autocracy, 1 electoral autocracy, 2 electoral democracy, 3 liberal democracy |
| Health system | `polio` | Polio vaccination coverage (%) |

Hepatitis B and measles vaccination were dropped because they are highly correlated with polio (multicollinearity risk; polio has the highest correlation with life expectancy). Military spending was dropped because of too many missing values.

The cleaned dataset is saved as `World_Dataset.csv` (and as `data_group_29.RData`).

## 🔬 Methodology

1. **Exploration** – summary statistics, histograms on original and log scale (fertility and GDP per capita become more symmetric after the log transformation), correlation matrix.
2. **Models** – three multiple linear regressions compared by adjusted R²:
   - Model 1: level-level (all variables in levels)
   - Model 2: log-linear for fertility and GDP per capita
   - Model 3: level-level with the interaction `regime × gdppercap`
3. **Final model** (Model 1, best adjusted R²):

   `lifeexp ~ fertility + gdppercap + hdi + water + regime + polio`
4. **Diagnostics** – Breusch–Pagan test, special White test for heteroskedasticity, and Ramsey **RESET** test for functional form.
5. **Residual analysis** – a world map of residuals shows countries that live longer (blue) or shorter (red) than predicted.

## 📁 Repository structure

All files sit at the same level, so that the relative paths used by the poster (`style.css`, `World_Dataset.csv`) work without changes.

```
.
├── README.md
├── .gitignore
├── stat_project_4.Rmd       # full analysis: data download, cleaning, models, diagnostics
├── poster.qmd               # A1 poster (Quarto), reads World_Dataset.csv
├── style.css                # poster styling (A1 page, colours, tables)
├── World_Dataset.csv        # cleaned dataset, created by stat_project_4.Rmd
└── data_group_29.RData      # same cleaned dataset as an R object
```

## ▶️ How to run

**Packages**

```r
install.packages(c(
  "dplyr", "tidyr", "ggplot2", "readr", "corrplot", "lmtest", "car",
  "jsonlite", "stargazer", "maps", "ggrepel", "rmarkdown", "quarto"
))
```

**1. Analysis** – open `stat_project_4.Rmd` in RStudio and click *Knit*, or:

```r
rmarkdown::render("stat_project_4.Rmd")
```

The report downloads the Our World in Data tables live and writes `World_Dataset.csv` in the same folder. If you do not want to rely on the online sources, use the saved copy: `load("data_group_29.RData")`.

**2. Poster** – needs `World_Dataset.csv` in the same folder and [Quarto](https://quarto.org/):

```bash
quarto render poster.qmd
```

This produces a self-contained `poster.html`. To get the PDF, open it in a browser and print on A1 paper (the page size is set in `style.css`) with background graphics enabled.

## ⚠️ Limitations

- Cross-sectional data: associations, not causal effects; possible reverse causality between fertility and life expectancy.
- Only 2022 and countries with complete data are included, so the results do not describe countries with missing indicators.
- The residual map highlights unexplained gaps (cultural, inequality, local crises), but the model does not measure those factors.

**Future work:** panel data for causal inference, inequality measures and cultural factors to explain regional residuals.

## 🛠️ Tech stack

R · tidyverse (dplyr, tidyr, ggplot2) · lmtest · car · stargazer · corrplot · Quarto / R Markdown

## 📚 References

- Besley, T., & Kudamatsu, M. (2006). Health and Democracy. *American Economic Review*.
- Cutler, D., Deaton, A., & Lleras-Muney, A. (2006). The Determinants of Mortality. *Journal of Economic Perspectives*.
- Lleras-Muney, A. (2005). The Relationship Between Education and Adult Mortality in the United States. *Review of Economic Studies*.
- Our World in Data (2022). [ourworldindata.org](https://ourworldindata.org/).
