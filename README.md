# Spatial Transit Equity Indexing: Kings County (Brooklyn), NY

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-username/your-repo-name/blob/main/notebooks/transit_equity_analysis.ipynb)

An engineering-driven spatial analysis and data wrangling framework that synthesizes US Census Bureau TIGER/Line shapefiles and American Community Survey (ACS) data to evaluate regional transit dependency. By applying mathematical scaling, log/square-root normalization, and Principal Component Analysis (PCA), this project constructs a multi-dimensional **Transit Equity Priority Index** by census tract to map high-dependence transit zones.

---

## 🗺️ Project Overview & Methodology

Evaluating transit accessibility requires moving beyond raw averages to capture overlapping layers of systemic socio-economic vulnerability. This pipeline extracts, cleans, and integrates data across six distinct demographic metrics at the census tract level:

1. **Zero-Vehicle Ownership** (ACS Table B25044)
2. **Poverty Status** (ACS Table B17017)
3. **Unemployment Rate** (ACS Table B23025)
4. **Age Vulnerability** (Population Over 65; ACS Table B01001)
5. **Physical Mobility Constraints** (Population with a Disability; ACS Table B18101)
6. **Digital Infrastructure Disconnect** (No Internet Access; ACS Table B28011)

### 📈 Data Transformation Pipeline
To ensure robust statistical integrity and prevent extreme local outliers from distorting regional indexing, the pipeline implements rigorous spatial and data wrangling constraints:
* **Filtering Outliers:** Non-residential tracts and outliers are dynamically isolated by removing any census tracts with fewer than 100 households or total population.
* **Mitigating Skewness:** Highly skewed datasets (such as unemployment rates and digital disconnect indicators) are normalized using log transformations ($\log(1+x)$) or square-root scaling to stabilize variance.
* **Standardization:** All transformed dimensions are converted into uniform Z-scores using a `StandardScaler` to align disparate units onto a standard normalized scale centered around 0.

---

## 🧮 Principal Component Analysis (PCA) & Weighting

To minimize multi-collinearity among socioeconomic variables, **Principal Component Analysis (PCA)** is utilized to reduce dimensionality into localized equity indicators. The core pipeline maps components dynamically captured breaking at the scree plot elbow:

* **PC1: Systemic Socioeconomic Vulnerability (~36.3% Variance)** — Driven heavily by high poverty, lack of internet access, and zero car ownership. This represents core transit-dependent riders relying on public services for economic mobility.
* **PC2: Age & Physical Mobility Needs (~25.4% Variance)** — Isolated by older populations and mobility impairments. This captures areas requiring physical accessibility infrastructure (e.g., paratransit, low-floor bus integration, and ADA compliance).
* **PC3: Structural Employment Gap (~14.5% Variance)** — Driven by pockets of high unemployment despite stable localized digital infrastructure, highlighting requirements for off-peak, flexible service hours.

### Transit Equity Priority Index Formula
The final actionable priority index is calculated by scoring each census tract against the variance-explained weights of the three principal components:

$$\text{Transit Equity Score} = 0.477 \text{PC1} + 0.331 \text{PC2} + 0.192 \text{PC3}$$

Higher scores explicitly isolate high-priority census tracts requiring public transit investment and infrastructure deployment.

---

## 📂 Repository Folder Structure

```text
transit-equity-index-brooklyn/
│
├── requirements.txt         # Package dependencies (pandas, geopandas, pygris, etc.)
├── README.md                # Project documentation and interactive links
│
├── data/
│   ├── raw/                 # Input ACS Census csv sheets (Vehicle, Poverty, etc.)
│   └── processed/           # Filtered dataframes mapping Z-scores and component values
│
├── notebooks/
│   └── transit_equity_analysis.ipynb  # Core interactive analysis & visualizations
│
└── src/                     # Modular backend pipeline scripts
    ├── data_cleaning.py     # Script for data ingestion, filtering, and scaling
    └── pca_analysis.py      # Script for executing PCA and matrix loading calculations
