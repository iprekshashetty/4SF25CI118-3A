# Amazon Delivery Logistics & Performance Analysis – Milestone 1 (CIE)

## Project Overview

This repository contains the exploratory data analysis (EDA), data cleaning, feature engineering, and visualization workflows performed on the **Amazon Delivery Logistics** dataset consisting of **43,739 delivery records**.

The primary objective of this milestone is to inspect delivery agent performance, analyze delivery operational variables (weather, traffic density, vehicle types, geographic regions), engineer temporal metrics (pickup and order intervals), and identify factors that influence delivery efficiency.

---

## Academic & Repository Metadata

- **Student USN / Section:** `4SF25CI075_3A`
- **Evaluation Milestone:** Milestone 1 – Continuous Internal Evaluation (CIE)
- **Dataset:** `amazon_delivery.csv`
- **Dataset Volume:** 43,739 records $\times$ 16 initial features
- **Environment:** Python 3.14+ (Jupyter Notebook, Pandas, NumPy, Matplotlib, Seaborn)

---

## Dataset Schema

| Attribute         | Data Type           | Description                                                                  |
| :---------------- | :------------------ | :--------------------------------------------------------------------------- |
| `Order_ID`        | String              | Unique alpha-numeric identifier for each delivery order                      |
| `Agent_Age`       | Integer / Float     | Age of the assigned delivery agent (15–50 years)                             |
| `Agent_Rating`    | Float               | Customer rating assigned to the agent (1.0–6.0 scale)                        |
| `Store_Latitude`  | Float               | GPS latitude of the fulfillment center / retail outlet                       |
| `Store_Longitude` | Float               | GPS longitude of the fulfillment center / retail outlet                      |
| `Drop_Latitude`   | Float               | GPS latitude of the customer destination                                     |
| `Drop_Longitude`  | Float               | GPS longitude of the customer destination                                    |
| `Order_Date`      | Datetime            | Date on which the customer placed the order                                  |
| `Order_Time`      | String / Object     | Time at which the order was booked                                           |
| `Pickup_Time`     | String / Object     | Time when the delivery agent retrieved the package                           |
| `Weather`         | String              | Environmental condition (_Sunny, Stormy, Sandstorms, Cloudy, Windy, Fog_)    |
| `Traffic`         | String              | Traffic congestion status (_Low, Medium, High, Jam_)                         |
| `Vehicle`         | String              | Vehicle mode utilized (_motorcycle, scooter_)                                |
| `Area`            | String              | Region classification (_Metropolitan, Urban, Semi-Urban_)                    |
| `Delivery_Time`   | Integer / Timedelta | Initial delivery duration in minutes; recomputed as transit delta            |
| `Order_Category`  | String              | Merchandise segment (_Electronics, Clothing, Sports, Cosmetics, Home, etc._) |

---

## Workflow & Methodology

### 1. Data Ingestion & Integrity Auditing

- Loaded 43,739 records and audited data distributions via `.describe()` and `.info()`.
- Identified null presence across coordinates (`Store_Latitude` and `Store_Longitude` missing ~8.01%), temporal timestamps (`Order_Time`, `Pickup_Time` missing <1%), and ratings (`Agent_Rating` missing ~0.13%).

### 2. Preprocessing & Data Cleaning

- **Imputation**:
  - Imputed missing `Agent_Age` entries with the global mean ($\approx 29.57$ years).
  - Imputed missing `Agent_Rating` entries using the overall mean score ($\approx 4.63$).
- **Attribute Transformations**:
  - Dropped redundant/raw numerical `Delivery_Time` column to compute precise timestamp differences.
  - Renamed `Category` $\rightarrow$ `Order_Category` for clearer domain terminology.
- **Datetime Feature Engineering**:
  - Parsed `Order_Date` into structured temporal components: `year`, `month`, and `day`.
  - Parsed `Pickup_Time` and `Order_Time` to derive `Delivery_time` (interval between order placement and fulfillment pickup).

### 3. Data Indexing & Querying

- Evaluated conditional subsets using `.loc` and `.iloc` accessors (e.g., filtering orders dispatched by agents aged $>30$).
- Filtered category-level frequencies grouped across urban and metropolitan zones.
- Computed top-performing delivery personnel filtered by highest average rating.

### 4. Exploratory Visualizations

The analysis in `milestone.ipynb` incorporates the following graphical evaluations:

- **Vehicle Mode vs. Transit Duration**: Bar plot analyzing transit duration variances across vehicle types (`motorcycle` vs. `scooter`).
- **Regional Demand Distribution**: Line plot tracking total orders across localized geographic areas (`Metropolitian`, `Urban`).
- **Seasonal Category Demand**: Distribution plot illustrating monthly fulfillment patterns across order categories.
- **Fleet Composition**: Pie chart showing the proportional share of motorcycles versus scooters across deliveries.
- **Order Trajectory over Time**: Line chart tracing day-by-day aggregate order volume across early 2022.
- **Temporal Regional Density**: Violin plot illustrating date distributions against regional delivery zones.

---

## Repository Structure

```text
├── data/
│   └── amazon_delivery.csv              # Raw logistics dataset (43,739 records)
├── notebooks/
│   └── milestone.ipynb                  # Data preprocessing, EDA, and visualization notebook
├── figures/                             # Exported charts and plot visualizations
├── README.md                            # Comprehensive project overview and documentation
└── requirements.txt                     # Python library dependencies
```

---

## Setup & Execution

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/amazon-delivery-analysis.git
cd amazon-delivery-analysis
```

### 2. Create and Activate a Virtual Environment

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

_(Minimum required libraries: `pandas`, `numpy`, `matplotlib`, `seaborn`)_

### 4. Run the Jupyter Notebook

```bash
jupyter notebook notebooks/milestone.ipynb
```

---

## Summary of Key Findings

- **High Agent Ratings**: Agent ratings are tightly clustered around a mean of $4.63$, indicating consistently high overall service scores.
- **Metropolitan Concentration**: Metropolitan zones constitute the majority of order transactions across all merchandise categories (e.g., Electronics, Toys, Books).
- **Vehicle Fleet**: Motorcycles and scooters represent the primary delivery transport fleet, with comparable operational transit durations across short urban routes.
- **Temporal Patterns**: Order volumes maintained steady momentum across February to April 2022, with peak activity observed across cosmetic and household merchandise segments.
