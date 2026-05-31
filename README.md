# ✈️ Does Lisbon Really Need a New Airport?

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

### Domestic vs International Traffic

* How different are domestic and international routes in terms of:

  * Passenger demand
  * Load factor
  * Capacity utilization

---

## Dataset

### Source

The dataset was obtained from Eurostat's aviation statistics database and includes historical air passenger transportation records involving Portuguese airports and their international partners.

### Scope

The analysis focuses on routes connected to Lisbon Airport (Humberto Delgado Airport) between 1996 and 2024.

### Scale

The original Eurostat extracts contained millions of aviation records.

After filtering, cleaning, and selecting the relevant observations for Lisbon Airport, the working dataset contained approximately **58,000 route-year observations**.

### Main Variables

* Year
* Route
* Passengers on Board
* Available Seats
* Number of Commercial Flights
* Arrival / Departure Classification
* Load Factor

---

## Project Pipeline

The project follows a complete Data Science workflow, emphasizing data acquisition, cleaning, transformation, and exploratory analysis.

### 1. Data Collection

The first stage involved collecting and combining multiple Eurostat datasets into a unified analytical dataset.

Main activities:

* Dataset inspection
* Schema validation
* Dataset concatenation
* Lisbon route filtering

---

### 2. Data Cleaning

Significant preprocessing was required before performing any analysis.

#### Removal of Non-Informative Columns

Several columns contained either only missing values or a single repeated value throughout the dataset.

Examples include:

* STRUCTURE
* STRUCTURE_ID
* STRUCTURE_NAME
* freq
* Time frequency

Removing these columns simplified the dataset without losing analytical information.

#### Missing Value Handling

* Detection of null values
* Removal of unusable records
* Validation of critical variables


#### Route Standardization

Airport names were standardized to improve readability and consistency.

Example:

```text
Humberto Delgado Airport ↔ Porto Airport
```

became:

```text
Lisbon ↔ Porto
```

The cleaning process preserved route directionality while simplifying airport naming conventions.

---

### 3. Feature Engineering

Several derived variables were created to support the analysis.

#### Load Factor

The primary metric used throughout the project:

```text
Load Factor = (Passengers on Board / Available Seats) × 100
```

Load factor measures the percentage of available seats occupied by passengers and serves as a proxy for route efficiency.

#### Route Classification

Routes were categorized as:

* Domestic
* International

#### Aggregated Metrics

Yearly and route-level aggregations were calculated for:

* Passengers
* Seats
* Flights
* Load Factors

---

## Exploratory Data Analysis

### Evolution of Load Factor

One of the most important findings concerns the long-term evolution of capacity utilization.

Key observations:

* Load factors remained around 65–70% during the late 1990s.
* Significant improvements appeared after 2008.
* Values consistently exceeded 80% after 2014.
* The highest load factors were observed after the pandemic recovery.

The sharp decline in 2020 reflects the impact of COVID-19 on global aviation.

Subsequent years show a rapid recovery, reaching record occupancy levels.

---

### Arrivals vs Departures

The analysis compared:

* ARR (Arrivals)
* DEP (Departures)
* GENERAL traffic

The three series follow nearly identical trends throughout the entire period.

This suggests a balanced flow of inbound and outbound traffic and indicates that congestion is not caused by directional imbalances.

---

### Domestic vs International Routes

A substantial difference emerges between domestic and international operations.

#### Average Load Factor

| Route Type    | Average Load Factor |
| ------------- | ------------------- |
| Domestic      | 66.3%               |
| International | 76.2%               |

International routes operate significantly closer to full capacity.

#### Passenger Traffic

| Route Type    | Total Passengers |
| ------------- | ---------------- |
| Domestic      | 63.4 Million     |
| International | 328.7 Million    |

International traffic clearly dominates Lisbon Airport operations.

This confirms Lisbon's strategic role as an international aviation hub rather than a domestic transportation center.

---

### Evolution of Passenger Demand

Passenger traffic experienced strong growth between 2013 and 2019.

Both:

* Available Seats
* Passengers on Board

increased steadily and almost proportionally.

This suggests airlines successfully adjusted supply to increasing demand.

The only major disruption occurred during the COVID-19 pandemic.

Traffic recovered rapidly between 2022 and 2024, exceeding pre-pandemic levels.

---

## Route-Level Analysis

### Top Routes by Passenger Volume

The busiest routes throughout the study period include:

1. Lisbon ↔ Madrid
2. Lisbon ↔ Paris (Orly)
3. Lisbon ↔ Porto
4. Lisbon ↔ London (Heathrow)
5. Lisbon ↔ Funchal

Madrid emerged as the largest route by passenger volume.

These routes represent critical components of Lisbon Airport's network.

---

### Highest Load Factor Routes

The routes with the highest average seat occupancy are:

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

The routes with the lowest average occupancy rates are:

| Route                     | Average Load Factor |
| ------------------------- | ------------------- |
| Lisbon ↔ Porto            | 59.9%               |
| Lisbon ↔ Faro             | 60.4%               |
| Lisbon ↔ Praia da Vitória | 65.0%               |
| Lisbon ↔ Casablanca       | 66.0%               |
| Lisbon ↔ Madrid           | 66.1%               |

Several domestic routes appear among the lowest-performing connections.

This may be partially explained by competition from alternative transportation modes such as trains, private vehicles, and increasingly popular long-distance bus services.

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

The results indicate that Lisbon Airport is experiencing substantial demand pressure, particularly on international routes.

However, the data also suggests that not all routes are operating at maximum efficiency.

Several domestic and regional connections continue to display relatively low load factors, indicating that operational optimization measures could improve overall capacity utilization before additional infrastructure becomes strictly necessary.

While long-term traffic growth supports discussions regarding future airport expansion, the evidence suggests that route optimization and capacity management strategies should also be considered as part of the solution.

---

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Jupyter Notebook

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

