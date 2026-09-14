# Parking Lot to Park: A GIS-Based Redevelopment Opportunity Analysis

## Overview

**Parking Lot to Park** is a geospatial analysis project that identifies and ranks existing parking lots in Downtown Dallas, Texas, based on their potential for redevelopment into public green space.

The project combines open geospatial data, transit accessibility, spatial analysis, and a **Multi-Criteria Scoring (MCS)** framework to identify locations where parking-lot redevelopment could potentially provide the greatest community and environmental benefit.

The analysis evaluates **379 parking lots** within the study area and produces a ranked set of redevelopment opportunities.

---

## Problem Statement

Large surface parking areas can contribute to land-use inefficiency and impervious surface coverage, particularly in dense urban environments.

This project asks:

> **Which existing parking lots in Downtown Dallas represent the strongest opportunities for redevelopment into public green space?**

Rather than evaluating parking lots using a single factor, the project combines five spatial criteria into a weighted suitability model.

---

## Objectives

The project aims to:

* Identify existing parking lots within the study area.
* Measure accessibility to public transit.
* Evaluate parking-lot size and redevelopment potential.
* Identify areas with relatively limited existing green-space coverage.
* Measure surrounding development intensity.
* Identify areas with high concentrations of nearby parking.
* Combine the criteria using weighted Multi-Criteria Scoring.
* Rank parking lots according to redevelopment opportunity.
* Produce an interactive web map and a professional QGIS map.

---

## Study Area

**Location:** Downtown Dallas, Texas, USA

The analysis uses a defined downtown study-area bounding box and a projected coordinate system suitable for distance and area calculations:

**CRS:** WGS 84 / UTM Zone 14N (**EPSG:32614**)

A **500-meter neighborhood context** is used for the spatial indicators that measure surrounding land use, development, and parking concentration.

---

## Data Sources

| Dataset                | Source        | Use                                           |
| ---------------------- | ------------- | --------------------------------------------- |
| Parking infrastructure | Overture Maps | Identify candidate parking lots               |
| Land use               | Overture Maps | Measure surrounding green/open-space coverage |
| Buildings              | Overture Maps | Measure surrounding development               |
| Transit stops          | DART GTFS     | Calculate transit accessibility               |
| Basemap/context        | OpenStreetMap | Cartographic context                          |

---

## Methodology

The analysis follows an end-to-end geospatial workflow:

```text
Data Acquisition
       ↓
Data Cleaning & Preparation
       ↓
Coordinate Reference System Transformation
       ↓
Parking Lot Identification
       ↓
Spatial Analysis
       ↓
Criterion Scoring
       ↓
Weighted Multi-Criteria Scoring
       ↓
Candidate Ranking
       ↓
Interactive & Static Mapping
```

### 1. Parking Lot Identification

Parking infrastructure was obtained from Overture Maps and filtered to features classified as **parking**.

Point-only features were excluded from the area-based analysis, resulting in:

**379 parking lot polygons**

---

### 2. Transit Accessibility

DART GTFS data was used to identify transit stops within the study area.

For each parking lot, the distance to its nearest DART stop was calculated.

A normalized score from 0–100 was then generated, where:

* Shorter distance → higher score
* Longer distance → lower score

**Weight: 30%**

---

### 3. Parking Lot Size

The area of each parking lot was calculated in square meters using the projected CRS.

Larger parking lots receive higher redevelopment opportunity scores because they provide greater potential land area for conversion.

The lots were divided into five percentile-based groups and assigned scores of:

**20, 40, 60, 80, or 100**

**Weight: 25%**

---

### 4. Green-Space Opportunity

Existing green/open-space land uses were identified from Overture Maps.

Green-space coverage was calculated within a **500 m buffer** around each parking lot.

The scoring is intentionally inverse:

> Areas with less existing green space receive higher redevelopment opportunity scores.

This represents the potential additional value of introducing new green space in areas with relatively limited existing coverage.

**Weight: 20%**

---

### 5. Surrounding Development

Building footprints were used to estimate development intensity within the 500 m context around each candidate.

Building coverage was calculated as the proportion of the 500 m neighborhood context occupied by building footprints.

Higher surrounding development intensity receives a higher score, reflecting the potential value of additional public green space in built-up areas.

**Weight: 15%**

---

### 6. Parking Concentration

For each candidate, the total area of **other parking lots within 500 m** was calculated.

Higher concentrations of surrounding parking indicate areas where parking land uses are particularly prevalent and where redevelopment could potentially produce a larger land-use benefit.

The candidate lot itself was excluded from this calculation.

**Weight: 10%**

---

# Multi-Criteria Scoring

The final suitability score is calculated using:

**Final Score =**

```text
Transit Accessibility × 0.30
+ Parking Lot Size × 0.25
+ Green-Space Opportunity × 0.20
+ Surrounding Development × 0.15
+ Parking Concentration × 0.10
```

The resulting score ranges from **0 to 100**, with higher values indicating greater redevelopment opportunity according to the model.

---

## Results

The analysis evaluated:

| Metric                       |         Result |
| ---------------------------- | -------------: |
| Parking lots analyzed        |        **379** |
| DART stops in study area     |        **222** |
| High Priority candidates     |         **72** |
| Moderate Priority candidates |        **286** |
| Low Priority candidates      |         **21** |
| Neighborhood context         |      **500 m** |
| Highest MCS score            | **98.0 / 100** |

The highest-ranked candidate was:

**DAL-PARK-001 — MCS: 98.0**

The model identified a concentration of high-scoring opportunities in areas combining strong transit accessibility, relatively large parking areas, high surrounding development, limited existing green-space coverage, and substantial nearby parking.

---

## Priority Classification

Candidates were classified as:

| Priority          | MCS Score |
| ----------------- | --------: |
| High Priority     |      ≥ 80 |
| Moderate Priority |  50–79.99 |
| Low Priority      |      < 50 |

This classification provides a simple way to communicate the continuous suitability scores to non-technical audiences.

---

## Outputs

### Interactive Map

The project includes a **Folium interactive map** showing:

* Parking redevelopment candidates
* Candidate priority
* Candidate IDs
* Multi-Criteria Scores
* DART transit stops
* Study-area boundary

### Static Map

A professional QGIS cartographic output was produced containing:

* Redevelopment priority symbology
* Top candidate labels
* DART transit stops
* Study-area boundary
* Multi-Criteria Scoring weights
* Top-ranked opportunities
* Scale bar and north arrow
* Data sources and methodology information

---

## Technology Stack

**Programming & Analysis**

* Python
* GeoPandas
* Pandas
* NumPy

**Geospatial Data**

* Overture Maps
* GeoParquet
* DART GTFS
* OpenStreetMap

**GIS & Visualization**

* QGIS
* Folium
* Spatial joins
* Buffer analysis
* Overlay analysis
* Proximity analysis
* Multi-Criteria Scoring

---

## Project Structure

```text
Parking-Lot-to-Park-Dallas/
│
├── README.md
│
├── data/
│
├── notebooks/
│
├── scripts/
│
├── outputs/
│   ├── maps/
│   └── tables/
│
├── images/
│
└── docs/
```

---

## Limitations

This project is an **opportunity-screening model**, not a final feasibility or development assessment.

Important limitations include:

* Parking ownership and parcel ownership were not incorporated.
* Zoning and development regulations were not used in the suitability score.
* Land acquisition costs were not included.
* Existing parking demand was not modeled.
* Environmental and engineering feasibility were not assessed.
* Green-space and building indicators use a 500 m neighborhood context.
* The model relies on the completeness and positional accuracy of the underlying datasets.
* A high score indicates spatial suitability according to the selected criteria, not guaranteed feasibility.

The results should therefore be interpreted as **candidate locations for further investigation** rather than definitive redevelopment recommendations.

---

## Future Improvements

Potential extensions include:

* Incorporating parcel ownership and assessor data.
* Adding land value and acquisition-cost indicators.
* Including zoning and land-use regulations.
* Modeling pedestrian accessibility.
* Incorporating population and demographic data.
* Estimating impervious-surface reduction.
* Adding heat-island and environmental vulnerability indicators.
* Performing sensitivity analysis on the MCS weights.
* Developing an interactive dashboard.
* Comparing redevelopment scenarios and estimated green-space gains.

---

## Key Takeaway

This project demonstrates an end-to-end geospatial workflow for converting diverse spatial datasets into a decision-support product.

Rather than simply mapping parking lots, the analysis combines **transit accessibility, land area, green-space availability, urban development, and parking concentration** to identify where parking-to-park redevelopment may have the greatest potential impact.

**379 parking lots → 5 spatial criteria → weighted MCS → ranked redevelopment opportunities.**

---

## Author

**Gunjan Gautam**

GIS Analyst | Environmental Science | Geospatial Analysis

**Tools:** Python • QGIS • GeoPandas • Overture Maps • GTFS • Folium
