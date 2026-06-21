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

## 🧹 Geographic Data Filtering & Noise Reduction

Raw US Census datasets include geographic tracts that do not represent typical residential behavior (e.g., public parks, industrial maritime zones, and open water boundaries). If left unfiltered, these low-population zones create extreme mathematical outliers (division-by-zero errors or artificially inflated 100% percentages) that heavily skew the index.

To protect the integrity of the statistical pipeline, a strict filter is applied: **any census tract with fewer than 100 total households is isolated and masked out.**

| Raw Geographic Feature | Why It Is Masked Out | Impact on Transit Modeling |
| :--- | :--- | :--- |
| **Prospect Park / Green-Wood Cemetery** | High land area, near-zero resident households. | Eliminates zero-denominator inflation spikes. |
| **Jamaica Bay / Gateway National Area** | Open water boundaries and protected wetlands. | Prevents empty geographic polygons from skewing regional averages. |
| **Brooklyn Navy Yard / Industrial Edges** | High economic activity but minimal permanent residency. | Ensures index purely reflects consumer transit-dependency. |

### Visual Proof: Study Area Stabilization

By contrasting a reference map against our filtered spatial layout, you can see how the 100-household threshold perfectly identifies and black-boxes non-residential noise without losing critical community data:

<p align="center">
  <img src="images/maps/reference_google_map.jpg" width="45%" alt="Brooklyn Reference Map" />
  <img src="images/maps/initial_data_cleanup.jpg" width="45%" alt="Filtered Census Tracts" />
</p>

* **Purple Zone (Valid Tracts):** Stable, high-density residential tracts matching the core living areas seen on the map (Flatbush, Bedford-Stuyvesant, Borough Park).
* **Dark Dark Blue/Black Zone (Masked Outliers):** Automatically isolates Jamaica Bay (bottom right), Prospect Park (center blank spot), and the industrial shipping ports along the western coastline.

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
