# Transit Need, Accessibility, and Microtransit Analysis in Brooklyn, New York

A transportation accessibility research project investigating why some high-transit-need neighborhoods experience lower access to employment opportunities and whether microtransit can improve first-mile connectivity to rapid transit.

This project integrates demographic vulnerability analysis, public transit accessibility modeling, statistical inference, and microtransit simulation into a unified analytical framework.

---

## 📌 Project Overview

Transportation disadvantage is not solely a function of transit infrastructure. Communities with higher levels of poverty, unemployment, disability, aging populations, and limited vehicle access often depend more heavily on public transportation to access jobs and essential services.

This project seeks to answer three fundamental questions:

1. **Which neighborhoods exhibit high transit need and low job accessibility?**
2. **Why do some high-need neighborhoods perform better than others?**
3. **Can microtransit improve accessibility for disadvantaged communities?**

Using census data, GTFS transit schedules, OpenStreetMap networks, and travel-time accessibility modeling, the project develops a framework for identifying transit-need neighborhoods, diagnosing accessibility disparities, and evaluating potential interventions.

---

# 🔬 Research Framework

The analysis is conducted in three stages.

<p align="center">
  <img src="figures/research_framework.png" width="85%" alt="Research Framework"/>
</p>

---

## Stage 1: Transit Need and Accessibility Assessment

### Transit Need Priority Index (TNPI)

A Transit Need Priority Index (TNPI) was developed to identify neighborhoods where residents are likely to be more dependent on public transportation.

The index incorporates six census-derived indicators:

- Households below the poverty line
- Unemployment
- Zero-vehicle ownership
- Population over age 65
- Population with disabilities
- Households without internet access

Rather than assigning subjective weights, **Principal Component Analysis (PCA)** was used to identify latent vulnerability patterns and derive statistically grounded component weights.

### Accessibility Analysis

Transit accessibility was measured as:

> Total jobs reachable within 45 minutes using public transit and walking during the morning peak period.

Travel times were calculated using:

- GTFS transit schedules
- OpenStreetMap street networks
- R5 routing engine via `r5py`

Neighborhoods were classified into four categories:

| Transit Need | Accessibility | Category |
|-------------|--------------|----------|
| High | High | High Need – High Access |
| High | Low | High Need – Low Access |
| Low | High | Low Need – High Access |
| Low | Low | Low Need – Low Access |

This classification provides a foundation for identifying neighborhoods experiencing both high transportation need and poor access to employment opportunities.

---

## Stage 2: Understanding Accessibility Disparities

Identifying low-access neighborhoods is only the first step.

The second stage investigates:

> Why do some high-transit-need neighborhoods achieve strong accessibility while others do not?

Several built-environment and transportation variables were examined:

- Distance to nearest subway station
- Distance to nearest bus stop
- Subway station density
- Bus stop density
- Distance to Manhattan CBD

The following analytical techniques were employed:

### Statistical Testing

- Mann–Whitney U Tests

### Predictive Modeling

- Logistic Regression
- Random Forest Classification

### Variable Importance Assessment

- Permutation Importance Analysis

The objective is to identify which factors most strongly differentiate high-access and low-access transit-need neighborhoods.

---

## Stage 3: Microtransit Intervention Analysis

Findings from Stage 2 suggest that first-mile access to subway stations may play a critical role in determining accessibility outcomes.

To evaluate a potential intervention, a microtransit service was simulated.

The service is conceptualized as an on-demand first-mile connection between residential areas and nearby subway stations.

### Intervention Objectives

- Improve access to rapid transit
- Increase employment accessibility
- Reduce accessibility disparities among high-need neighborhoods

### Sensitivity Analysis

Because real-world microtransit services involve operational delays, accessibility outcomes were tested under varying assumptions regarding:

- Dispatch delays
- Passenger waiting times
- Service buffering

This analysis helps identify operational conditions under which microtransit remains effective.

---

# 📊 Key Findings

### 1. Transit Need and Accessibility Are Negatively Associated

Neighborhoods with higher transit need generally exhibit lower job accessibility.

### 2. Accessibility Differences Are Not Uniform

Not all high-need neighborhoods perform equally.

Some maintain strong accessibility despite elevated transit need.

### 3. Subway Access Appears Critical

Accessibility differences among high-need neighborhoods are strongly associated with proximity to subway stations.

### 4. Microtransit Shows Promise as a First-Mile Solution

Simulated microtransit interventions produce substantial accessibility gains for selected neighborhoods.

### 5. Operational Delays Matter

Accessibility improvements decline as dispatch and waiting delays increase, highlighting the importance of service design.

---

# 🗂 Repository Structure

```text
brooklyn-transit-need-microtransit/
│
├── notebooks/
│   ├── 01_transit_need_accessibility_analysis.ipynb
│   ├── 02_accessibility_determinants_analysis.ipynb
│   └── 03_microtransit_sensitivity_analysis.ipynb
│
├── data/
│   ├── raw/
│   └── processed/
│
├── figures/
│
├── outputs/
│
├── README.md
│
└── requirements.txt
```

---

# 📁 Data Sources

### Demographic and Socioeconomic Data

- American Community Survey (ACS) 5-Year Estimates
- U.S. Census Bureau

### Employment Data

- Longitudinal Employer-Household Dynamics (LEHD)
- LODES Workplace Area Characteristics (WAC)

### Transit Data

- MTA Subway GTFS
- MTA Bus GTFS

### Network Data

- OpenStreetMap

---

# 🛠 Technologies Used

### Python

- Pandas
- NumPy
- GeoPandas
- Scikit-Learn
- SciPy
- Matplotlib
- Seaborn
- OSMnx
- NetworkX
- r5py

### Accessibility Routing

- R5 Routing Engine
- GTFS Transit Schedules
- OpenStreetMap Networks

---

# ⚠️ Limitations

Several limitations should be considered when interpreting the results.

- The Transit Need Priority Index measures transportation-related vulnerability rather than transportation service quality.
- Accessibility is evaluated using a 45-minute cumulative-opportunities measure and may vary under alternative thresholds.
- Microtransit scenarios represent modeled interventions rather than observed services.
- Operational assumptions regarding dispatch delays and routing efficiency influence simulated outcomes.

Future work may incorporate:

- Additional cities and metropolitan regions
- Alternative accessibility metrics
- Service reliability measures
- Cost-effectiveness analysis
- Observed microtransit operating data

---

# 🚀 Future Directions

This project is part of an ongoing effort to understand the relationship between transportation need, accessibility, and mobility interventions.

Potential future extensions include:

- Cross-city comparative studies
- Accessibility equity benchmarking
- Demand-responsive transit optimization
- First-mile/last-mile accessibility planning
- Transportation resilience analysis

---

# 👤 Author

**Saksham Shrestha**

Independent Transportation Researcher

Kathmandu, Nepal

---

## Citation

If you use this repository, please cite the associated research project and provide attribution to the author.
