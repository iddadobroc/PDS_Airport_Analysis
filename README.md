# ✈️ Does Lisbon Really Need a New Airport?
 
[![Load Factor Heatmap](plots/Load%20Factor%20Heatmap%20-%20Top%2010%20Lisbon%20Routes%20by%20Passenger%20Volume.png)](plots/Load%20Factor%20Heatmap%20-%20Top%2010%20Lisbon%20Routes%20by%20Passenger%20Volume.png)
 
## Programming for Data Science Project (NOVA IMS)
 
### Authors
 
* Artur Dias
* Davide Corbo
* Hugo Caetano
* Monica Moreno
---
 
## Project Overview
 
In the last years, Portugal has debated whether Lisbon needs a new international airport.
 
The increasing number of passengers, flight operations, and recurring reports of congestion at Humberto Delgado Airport have often been used as evidence that Lisbon has reached its operational limits.
 
However, before investing billions of euros in a new airport infrastructure, an important question must be answered:
 
> Is Lisbon Airport truly operating beyond its capacity, or could existing traffic be optimized more efficiently?
 
This project explores historical air transportation data to understand the evolution of Lisbon Airport, evaluate route utilization, identify underperforming connections, and assess whether current traffic patterns support the argument for a new airport.
 
The analysis focuses on passenger demand, seat availability, route efficiency, seasonality, and long-term traffic trends.
 
---
 
## Research Questions
 
The project aims to answer the following questions:
 
### Airport Growth
 
* How has Lisbon Airport evolved over time?
* Has passenger traffic grown continuously or through specific turning points?
* Which routes have contributed most to this growth?
### Capacity Utilization
 
* Are flights operating close to full capacity?
* How has load factor evolved over the years?
* Are there routes with consistently low occupancy rates?
### Route Optimization
 
* Can low-performing routes be optimized?
* Could some routes operate with lower frequencies?
* Are there opportunities to improve airport efficiency without expanding infrastructure?
### Traffic by Route Type
 
* How different are domestic, European and intercontinental routes in terms of:
  * Passenger demand
  * Load factor
  * Capacity utilization
---
 
## Datasets
 
This project combines **two** data sources.
 
### 1. Eurostat — Air Passenger Transport (primary)
 
Historical air passenger transportation records involving Portuguese airports and their international partners (`avia_par_pt`), served as a raw CSV from the project's GitHub repository.
 
### 2. OurAirports — Reference Database (enrichment)
 
The open [OurAirports](https://ourairports.com/) `airports.csv` reference database, used to enrich Eurostat airport codes with clean airport names and geographic information.
 
### Scope
 
The analysis focuses on routes connected to Lisbon Airport (Humberto Delgado Airport) between **1996 and 2024**.
 
### Scale
 
The original Eurostat extracts contained millions of aviation records. After filtering, cleaning, code reconciliation and selecting the relevant observations for Lisbon Airport, the working dataset contained approximately **58,000 route-year observations**.
 
### Main Variables
 
* Year
* Route
* Passengers on Board
* Available Seats
* Number of Commercial Flights
* Arrival / Departure Classification
* Route Type (Domestic / European / Intercontinental)
* Load Factor
> **Reproducibility note:** both datasets are downloaded automatically from their URLs when the notebook runs. No manual data download is required.
 
---
 
## Project Pipeline
 
The project follows a complete Data Science workflow, emphasizing data acquisition, cleaning, transformation, and exploratory analysis.
 
### 1. Data Collection
 
The first stage loads the raw Eurostat dataset and the OurAirports reference database directly from their online sources and inspects them.
 
Main activities:
 
* Dataset inspection (dimensions, dtypes, missing values, unique values, sampling)
* Schema validation
* Loading of both sources for later enrichment
---
 
### 2. Data Cleaning
 
Significant preprocessing was required before any analysis. This was the most demanding part of the project, because the Eurostat route identifiers are inconsistent and mix real airports with non-airport entities.
 
#### Removal of Non-Informative Columns
 
Several columns contained either only missing values or a single repeated value throughout the dataset (e.g. `STRUCTURE`, `STRUCTURE_ID`, `STRUCTURE_NAME`, `freq`, `Time frequency`). Removing them simplified the dataset without losing analytical information.
 
#### Airport Code Extraction & Inconsistency Detection
 
* Portuguese and non-Portuguese airport codes were parsed out of the Eurostat route identifier.
* The same physical airport frequently appeared under **multiple codes**.
* Eurostat also reports **non-airport entities** in place of real airports: FIR centres, area control centres, and meteorological offices.
* A dedicated check reconciled `MADEIRA` / `FUNCHAL` entries that refer to the same airport under different names and codes.
#### Manual Correction & Consolidation
 
FIR/area-control codes, control-area variants and historical/obsolete duplicates were corrected and consolidated so that each physical airport is represented by a single code. The data was then filtered to keep only routes where **Lisbon is the origin airport**.
 
#### Geographic Enrichment (OurAirports)
 
The cleaned codes were merged against OurAirports to attach standardized airport names and geography. Entries that could not be matched — **16 codes / 630 rows**, corresponding to non-commercial FIR/control-area identifiers — were excluded from the analysis.
 
#### Deduplication & Schema Simplification
 
Duplicate route-year observations were verified and aggregated, values were rounded to integers, and the verbose Eurostat columns were renamed to a clean lowercase schema with a fixed column order.
 
#### Route Standardization
 
Airport names were standardized to improve readability. For example:
 
```text
Humberto Delgado Airport ↔ Porto Airport
```
 
became:
 
```text
Lisbon ↔ Porto
```
 
The process preserved route directionality while simplifying airport naming conventions, and disambiguated cities served by more than one airport.
 
---
 
### 3. Feature Engineering
 
Several derived variables were created to support the analysis.
 
#### Load Factor
 
The primary metric used throughout the project:
 
```text
Load Factor = (Passengers on Board / Available Seats) × 100
```
 
Load factor measures the percentage of available seats occupied by passengers and serves as a proxy for route efficiency. Values are capped at 100 and rounded, and years with anomalous or missing data are removed (see the data-quality note on 2000–2001 in the notebook).
 
#### Route Classification
 
Each route is classified by the destination country code into:
 
* **Domestic** — destination in Portugal (`PT`)
* **European** — destination in another European country
* **Intercontinental** — destination outside Europe
#### Aggregated Metrics
 
Yearly and route-level aggregations were calculated for passengers, seats, flights, and load factors.
 
---
 
## Exploratory Data Analysis
 
### Evolution of Load Factor
 
One of the most important findings concerns the long-term evolution of capacity utilization.
 
Key observations:
 
* Load factors remained around 65–70% during the late 1990s.
* Significant improvements appeared after 2008.
* Values consistently exceeded 80% after 2014.
* The highest load factors were observed after the pandemic recovery.
The sharp decline in 2020 reflects the impact of COVID-19 on global aviation, followed by a rapid recovery to record occupancy levels.
 
---
 
### Arrivals vs Departures
 
The analysis compared Arrivals (ARR), Departures (DEP) and GENERAL traffic. The three series follow nearly identical trends throughout the entire period, suggesting a balanced flow of inbound and outbound traffic and indicating that congestion is not caused by directional imbalances.
 
---
 
### Domestic, European & Intercontinental Routes
 
Substantial differences emerge across route types. **European routes dominate** passenger traffic at Lisbon, while **intercontinental** traffic grew steadily — reflecting Lisbon's role as a hub connecting Europe with Portuguese-speaking markets in Brazil and Africa. **Domestic** traffic remained essentially flat.
 
Rolling the European and intercontinental groups together as "international", the contrast with domestic traffic is clear:
 
#### Average Load Factor
 
| Route Type    | Average Load Factor |
| ------------- | ------------------- |
| Domestic      | 66.3%               |
| International | 76.2%               |
 
#### Passenger Traffic
 
| Route Type    | Total Passengers |
| ------------- | ---------------- |
| Domestic      | 63.4 Million     |
| International | 328.7 Million    |
 
This confirms Lisbon's strategic role as an international aviation hub rather than a domestic transportation center.
 
---
 
### Evolution of Passenger Demand
 
Passenger traffic experienced strong growth between 2013 and 2019. Both Available Seats and Passengers on Board increased steadily and almost proportionally, suggesting airlines successfully adjusted supply to increasing demand. The only major disruption occurred during the COVID-19 pandemic, after which traffic recovered rapidly between 2022 and 2024, exceeding pre-pandemic levels.
 
---
 
## Route-Level Analysis
 
### Top Routes by Passenger Volume
 
The busiest routes throughout the study period include:
 
1. Lisbon ↔ Madrid
2. Lisbon ↔ Paris (Orly)
3. Lisbon ↔ Porto
4. Lisbon ↔ London (Heathrow)
5. Lisbon ↔ Funchal
Madrid and Paris have consistently dominated Lisbon's connectivity over 25 years, reflecting the strategic importance of these European corridors.
 
---
 
### Highest Load Factor Routes
 
| Route                     | Average Load Factor |
| ------------------------- | ------------------- |
| Lisbon ↔ Rome (Ciampino)  | 89.0%               |
| Lisbon ↔ Basel / Mulhouse | 88.7%               |
| Lisbon ↔ Orio al Serio    | 88.7%               |
| Lisbon ↔ London (Luton)   | 88.4%               |
| Lisbon ↔ Campinas         | 87.6%               |
 
These routes consistently operate near capacity and demonstrate highly efficient seat utilization.
 
---
 
### Lowest Load Factor Routes
 
| Route                     | Average Load Factor |
| ------------------------- | ------------------- |
| Lisbon ↔ Porto            | 59.9%               |
| Lisbon ↔ Faro             | 60.4%               |
| Lisbon ↔ Praia da Vitória | 65.0%               |
| Lisbon ↔ Casablanca       | 66.0%               |
| Lisbon ↔ Madrid           | 66.1%               |
 
Several domestic routes appear among the lowest-performing connections. This may be partially explained by competition from alternative transportation modes such as trains, private vehicles, and increasingly popular long-distance bus services.
 
---
 
### Load Factor Heatmap
 
The route-level heatmap reveals several important patterns:
 
* Continuous improvement in occupancy rates over time.
* Strong growth in international demand.
* Significant disruption during the pandemic years.
* Rapid post-pandemic recovery.
* Consistently stronger performance among international routes.
---
 
### Seats vs Passengers Analysis
 
A scatter plot comparing available seats and passenger volume reveals a strong positive relationship.
 
Key observations:
 
* Most points remain below the theoretical full-capacity line.
* Airlines generally align seat supply with passenger demand.
* Persistent large-scale overcapacity is uncommon.
The results suggest that capacity management is generally effective across the Lisbon route network.
 
---
 
## Main Findings
 
### Evidence Supporting Capacity Constraints
 
* Passenger traffic has increased dramatically over the last two decades.
* Load factors now frequently exceed 80%.
* International demand continues to expand.
* Post-pandemic passenger volumes reached historical highs.
### Evidence Supporting Optimization Opportunities
 
* Many routes still operate below full capacity.
* Several domestic routes exhibit relatively low occupancy rates.
* Capacity utilization varies significantly across routes.
* Opportunities remain for route optimization and frequency adjustments.
---
 
## Conclusion
 
The results indicate that Lisbon Airport is experiencing substantial demand pressure, particularly on international routes. However, the data also suggests that not all routes are operating at maximum efficiency.
 
Several domestic and regional connections continue to display relatively low load factors, indicating that operational optimization measures could improve overall capacity utilization before additional infrastructure becomes strictly necessary.
 
While long-term traffic growth supports discussions regarding future airport expansion, the evidence suggests that route optimization and capacity management strategies should also be considered as part of the solution.
 
---
 
## Technologies Used
 
* **Python**
* **pandas**, **NumPy** — data loading, cleaning and aggregation
* **Matplotlib** — static line and bar charts
* **seaborn** — heatmaps and statistical plots
* **Plotly** (with **kaleido**) — interactive charts and network maps, exported as static images
* **Pillow (PIL)** — image handling for exported figures
* **Jupyter Notebook**
---
 
## How to Run
 
1. Install the dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn plotly kaleido pillow
   ```
 
2. Open the notebook:
   ```bash
   jupyter notebook Notebooks/Lisbon_Airport_Integrated_Final_v03.ipynb
   ```
 
3. Run all cells top to bottom. Both datasets are downloaded automatically from their URLs, so no local data files are required.
> `lisbon_airport_integrated_final_v03.py` is a plain-Python export of the notebook (same code) and is provided for version control and quick inspection.
 
---
 
## Repository Structure
 
```text
PDS_Airport_Analysis/
│
├── Datasets/
│   ├── Raw_Data/
│   └── Processed_Data/
│
├── Notebooks/
│   ├── Lisbon_Airport_Integrated_Final_v03.ipynb
│   └── lisbon_airport_integrated_final_v03.py
│
├── Plots/
│   └── Data Visualizations
│
├── Report/
│   └── Final Report
│
├── notebook_contents.txt
└── README.md
```
 
---
 
## Academic Context
 
This project was developed as part of the **Programming for Data Science** course at **NOVA IMS**.
 
The project emphasizes:
 
* Data acquisition
* Data cleaning
* Data transformation
* Exploratory Data Analysis (EDA)
* Data visualization
* Storytelling through data
 
