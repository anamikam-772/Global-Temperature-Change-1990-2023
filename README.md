# Quantifying the Impact of CO₂ Emissions and Renewable Energy Adoption on Global Temperature Change (1990–2023)

**Published:** December 17, 2025  
**Authors:** Anamika Kumari Mishra , Afreen Sorathiya 

---

## Table of Contents
1. [Introduction](#1-introduction)  
2. [Data--preprocessing](#2-data--preprocessing)  
3. [Exploratory-analysis](#3-exploratory-analysis)  
4. [Final-project-conclusion](#4-final-project-conclusion)

---

## 1. Introduction
This project examines how rising **CO₂ emissions** and changing patterns of **renewable-energy adoption** relate to **global surface-temperature anomalies** between **1990 and 2023**.

By integrating **NASA global temperature records** with country-level indicators such as **CO₂ emissions**, **GDP per capita**, **population**, and **renewable-energy share**, the study aims to:

- quantify relationships between emissions, renewables, and warming
- compare differences across countries and regions
- detect structural patterns (clusters of emitters, fairness gaps, and inequality)
- build predictive models to understand how multiple factors jointly influence warming outcomes

A key theme of the project is **climate inequality**: many low-emitting countries still experience rising temperatures, highlighting an uneven burden.

---

## 2. Data & Preprocessing

### 2.1 Loading and Preparing Analytical Libraries
The workflow begins by loading key R libraries for:
- data wrangling and cleaning (`tidyverse`, `janitor`)
- visualization (`ggplot2`, `GGally`, `plotly`, `patchwork`)
- geospatial mapping (`sf`, `rnaturalearth`, `cartogram`)
- modeling and interpretability (`randomForest`, `lime`, `iml`, `mlr3`)

### 2.2 Importing and Cleaning NASA GISTEMP Temperature Data
- Source: **NASA GISTEMP**
- Monthly anomalies are cleaned by replacing missing placeholders (e.g., `"***"`) with `NA`
- Monthly data are averaged to create an **annual temperature anomaly**
- Restricted to **1990–2023** for alignment with other datasets

Output:
- `gistem_annual.rds`

### 2.3 Cleaning and Filtering OWID CO₂ Emissions Dataset
- Source: **Our World in Data (OWID)**
- Filtered to include **1990 onward**
- Provides emissions and related climate indicators (e.g., methane)

### 2.4 Reshaping WDI Population Data
- Source: **World Bank (WDI)**
- Converted from wide year-columns → long format (country–year rows)

### 2.5 Preparing WDI GDP per Capita Data
- Also reshaped wide → long format
- Used as a proxy for industrialization and development patterns

### 2.6 Processing WDI Renewable-Energy Share Data
- Reshaped wide → long format
- Captures annual renewable adoption by country

### 2.7 Merging WDI Indicators into a Unified Dataset
Population + GDP per capita + renewable share merged into one WDI table using:
- country code
- year

Restricted to **1990–2023**

### 2.8 Integrating WDI + OWID + NASA into One Analytical Frame
Final merge steps:
1. OWID emissions + WDI socio-economic indicators (country-year alignment)
2. Merge NASA annual temperature anomalies by year

Final output:
- `combined.rds`

---

## 3. Exploratory Analysis

### 3.1 Correlation Matrix
A correlation matrix is used to understand relationships among:
- temperature anomaly
- CO₂
- methane
- GDP per capita
- renewable energy share

Key pattern:
- CO₂ and methane show strong correlation
- renewable share tends to correlate negatively with CO₂ and GDP
- temperature anomaly correlations appear low due to global-vs-country granularity mismatch

### 3.2 CO₂ Emissions vs Temperature Anomaly
A scatterplot + regression line indicates:
- weak positive relationship
- reflects time trends + concentrated high-emitter influence
- highlights low-emitting countries clustered near zero emissions

### 3.3 GDP per Capita vs Renewable-Energy Share
A downward trend suggests:
- higher GDP countries may still depend heavily on fossil fuels
- lower-income countries may show high renewable shares due to hydro/biomass reliance

### 3.4 Population vs CO₂ Emissions
Strong positive relationship:
- large populations → higher total emissions
- reveals emissions intensity differences across countries

### 3.5 Acceleration of Global Warming (Second Difference)
The second-difference plot highlights:
- volatility in warming acceleration
- high acceleration spikes in recent years (2020–2023)

### 3.6 Time-Series Decomposition (STL)
STL decomposition shows:
- a persistent upward warming trend
- small seasonal pattern
- irregular remainder linked to natural variability (e.g., El Niño)

### 3.7–3.8 Parallel Coordinate Plots (Static + Interactive)
Top emitters are compared across:
- CO₂
- GDP per capita
- renewable share
- temperature anomaly

Plotly version allows interactive exploration.

### 3.9 Global CO₂ Choropleth Map
A log-scaled map highlights:
- CO₂ concentration in major industrial regions
- global inequality in emissions responsibility

### 3.10 Two-Panel Map: Who Produces CO₂ vs Who Suffers Warming?
This visualization compares:
- countries with high emissions
- countries experiencing higher warming impacts

It emphasizes climate injustice: low emitters often suffer disproportionately.

### 3.11 Indexed Global Comparison (1990 = 100)
Compares growth trajectories of:
- global CO₂
- global temperature anomaly
- renewable energy share

Key insight:
- CO₂ and temperature rise sharply
- renewables increase only modestly and late

### 3.12–3.14 Global Trends Over Time
Separate time-series plots for:
- temperature anomaly
- global CO₂ totals
- renewable share

---

## 4. Final Project Conclusion
This project shows that global temperature change is closely linked to how countries grow, consume energy, and develop over time.

Main conclusions:
- **CO₂ emissions rise alongside temperature anomalies**, reinforcing the human-driven warming link.
- **Renewable energy adoption reduces warming pressure**, but the global pace is not yet sufficient.
- Countries fall into distinct patterns:
  - high-emitting fast-growth economies
  - low-emitting countries still experiencing rising temperatures
  - a smaller set that show early signs of cleaner growth via renewables

Overall, the project highlights that climate change is global, but responsibility and impact are uneven—supporting the case for renewable investment, energy efficiency, and climate justice policies.

---

## Reproducibility Notes
- Scripts assume local CSV downloads for NASA, OWID, and WDI sources.
- Intermediate datasets are saved as `.rds` in `/data`.

Suggested folder structure:
