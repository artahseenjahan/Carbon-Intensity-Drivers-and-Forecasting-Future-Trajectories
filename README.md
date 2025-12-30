
# Analyzing Carbon Intensity Drivers and Forecasting Future Trajectories

This repository analyzes **carbon intensity of GDP** (CO₂e emissions per unit of GDP) using a **global country-year panel** built from the World Bank’s **World Development Indicators (WDI)**, and applies machine learning methods to (1) identify key drivers and (2) forecast near-term carbon intensity levels. :contentReference[oaicite:0]{index=0}

---

## Project Goals

1. **Understand drivers of carbon intensity** across countries by examining structural factors such as energy systems, economic development, industrial composition, innovation capacity, and natural-resource rents. :contentReference[oaicite:1]{index=1}  
2. **Forecast carbon intensity** using a cleaned and harmonized country-year panel dataset and compare predictive performance across multiple model classes. :contentReference[oaicite:2]{index=2}

---

## Data

### Source
- **World Bank – World Development Indicators (WDI)** (public data). :contentReference[oaicite:3]{index=3}

### Time coverage
- Panel spans **2000–2023** (variable availability dependent). :contentReference[oaicite:4]{index=4}

### Response variable (target)
- **Carbon intensity of GDP**: *kg CO₂-e per constant 2021 PPP dollar of GDP* :contentReference[oaicite:5]{index=5}

### Predictors
Predictors cover six broad categories (energy, GDP, industry, population, research, and resource rents). A full list (with variable codes and units) is documented in the project report appendix. :contentReference[oaicite:6]{index=6} :contentReference[oaicite:7]{index=7}

---

## Train/Test Design

Because the panel is time-dependent, the project uses a **chronological split** (not random sampling):

- **Train:** 2000–2022  
- **Test:** 2023 only :contentReference[oaicite:8]{index=8}

---

## Data Cleaning & Preprocessing

A harmonized country-year panel is constructed by reshaping WDI indicators from wide-to-long format and merging by country code and year. :contentReference[oaicite:9]{index=9}

Missingness is handled using a three-layer approach:
1. **Within-country linear interpolation** (when bounded by existing values),
2. **Country mean imputation** when interpolation isn’t feasible,
3. **Global mean imputation** for remaining gaps. :contentReference[oaicite:10]{index=10}

Some “invalid zeros” (where zero is not economically meaningful) are treated as missing (e.g., some R&D and resource-rent indicators). :contentReference[oaicite:11]{index=11}

---

## Methods

The repository compares:
- **OLS**
- **LASSO** (two regularization levels)
- **Random Forest**
- **Gradient Boosting** :contentReference[oaicite:12]{index=12}

Tree-based ensemble methods outperform linear baselines by capturing nonlinearities and interactions across energy, economic, industrial, demographic, innovation, and resource factors. :contentReference[oaicite:13]{index=13}

---

## Key Results

### Best-performing model: Random Forest
Random Forest delivers the strongest out-of-sample performance for the 2023 test year (highest R² and lowest error). :contentReference[oaicite:14]{index=14}

Reported performance (2023 test):
- **Random Forest:** Test RMSE ≈ **0.09**, Test MAE ≈ **0.05**, Test R² ≈ **0.95** :contentReference[oaicite:15]{index=15}  
(Also summarized in the presentation.) :contentReference[oaicite:16]{index=16}

### Most important predictors (Random Forest, %IncMSE)
Top predictors include:
- **GDP growth rate**
- **Manufacturing share of GDP**
- **Fossil fuel share**
- **R&D expenditure**
- **Gas rents** :contentReference[oaicite:17]{index=17}  
(Also highlighted in the presentation.) :contentReference[oaicite:18]{index=18}

---

## Repository Contents

Typical contents in this repository:

- `Code.Rmd` — Full analysis pipeline (cleaning, modeling, evaluation, plots)
- `WDI_panel_dataset.csv` — Raw/constructed country-year panel (pre-cleaning)
- `Cleaned_panel_dataset.csv` — Cleaned/imputed dataset used for modeling
- `ML_Final_Project.pdf` — Full written report (methods, results, appendix)
- `Presentation.pdf` — Slide deck summary of the project

> If your GitHub structure differs, update this section to match your folder layout.

---

## How to Run (Reproducibility)

### Requirements
- **R (4.x recommended)**
- RStudio (optional but convenient)

### R packages used
The RMarkdown script loads packages including:
`tidyverse`, `dplyr`, `readr`, `janitor`, `zoo`, `glmnet`, `randomForest`, `gbm`, `caret`, `ggplot2`, and mapping/plot helpers (e.g., `sf`, `rnaturalearth`).  
(See `Code.Rmd` for the complete list.)

Install common dependencies:
```r
install.packages(c(
  "tidyverse","dplyr","readr","janitor","zoo","glmnet","ggplot2",
  "caret","randomForest","gbm","reshape2","readxl",
  "sf","rnaturalearth","rnaturalearthdata","rpart","rpart.plot"
))

install.packages("rmarkdown")
rmarkdown::render("Code.Rmd")

