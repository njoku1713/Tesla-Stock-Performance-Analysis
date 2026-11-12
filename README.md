# Tesla Stock Performance Analysis
## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools and Technologies](#tools-and-technologies)
- [Excel for Data Formatting](#excel-for-data-formatting)
- [Dataset Breakdown](#dataset-breakdown)
- [Dataset Transformation](#dataset-transformation)
- [Power BI Dashboard](#power-bi-dashboard)
- [Insights and Findings](#insights-and-findings)
- [Recommendations](#recommendations)
- [Future Work](#future-work)

### Project Overview
This project analyses used Tesla vehicle sales data across U.S. states using interactive Power BI dashboards. It examines five models, namely: Model 3, Model Y, Model S, Model X, and Cyber-truck, to identify differences in pricing, mileage, accident history, and regional market performance. 

The analysis focuses on average vehicle prices, odometer readings, and state-level market patterns, exploring how vehicle characteristics and location relate to price variations. By presenting these findings through clear, interactive visualizations, the project helps buyers, sellers, and automotive businesses compare models and make informed decisions in the used Tesla market.

### Data Sources
The primary dataset used for this analysis is the "Tesla Stock Dataset.csv" file, containing detailed information about the five vehicle models manufactured by Tesla.

### Tools and Technologies
- Microsoft Power BI (for visualization)
- Excel (data cleaning)
- Power Query (ETL in Power BI)
- DAX (measures and KPIs)
- Adobe Illustrator / Figma (UI enhancement)

### Excel for Data Formatting
- Cleaned missing values and duplicates
- Formatted numeric fields (e.g., price, mileage, EMI)
- Standardized column names and categories
- Created calculated fields like EMI (if missing)
- Prepared structured layout for Power BI import

<img width="1080" height="447" alt="dataset" src="https://github.com/user-attachments/assets/3b2b2c0e-a520-4a9a-b522-b670dae81ce7" />


### Dataset Breakdown
The dataset used for this analysis includes:
- Tesla model name
- Average price (USD)
- Median odometer (miles)
- EMI (Estimated Monthly Installment)
- State of sale
- Accident history (% with or without)
- Driver Assist System type:
  - Full Self-Driving (FSD)
  - Enhanced Autopilot
  - Standard Autopilot

Each vehicle record was filtered and grouped to compute KPIs across states.
### Dataset Transformation
Using Power Query and DAX, the data was transformed to calculate:
- Average price per model/state
```VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    AVERAGE('Tesla Stock Dataset'[price]),
    CALCULATE(
        AVERAGE('Tesla Stock Dataset'[price]),
        'Tesla Stock Dataset'[model] = selected
    )
)
```
- Median mileage (odometer)
```MedianOdometer = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    MEDIAN('Tesla Stock Dataset'[odometer]),
    CALCULATE(
        MEDIAN('Tesla Stock Dataset'[odometer]),
        'Tesla Stock Dataset'[model] = selected
    )
)
```
- EMI estimates
```AverageEMI = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    AVERAGE('Tesla Stock Dataset'[EMI]),
    CALCULATE(
        AVERAGE('Tesla Stock Dataset'[EMI]),
        'Tesla Stock Dataset'[model] = selected
    )
)
```
- Accident percentage
``` PercentWithAccident = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    DIVIDE(
        CALCULATE(COUNTROWS('Tesla Stock Dataset'), 'Tesla Stock Dataset'[accident_history] <> "No Accidents"),
        COUNTROWS('Tesla Stock Dataset'),
        0
    ),
    DIVIDE(
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected,
            'Tesla Stock Dataset'[accident_history] <> "No Accidents"
        ),
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected
        ),
        0
    )
)
```
- Percent With No Accident
``` PercentNoAccident = 
VAR selected = [SelectedModel]
RETURN
IF(
    selected = "ALL",
    DIVIDE(
        CALCULATE(COUNTROWS('Tesla Stock Dataset'), 'Tesla Stock Dataset'[accident_history] = "No Accidents"),
        COUNTROWS('Tesla Stock Dataset'),
        0
    ),
    DIVIDE(
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected,
            'Tesla Stock Dataset'[accident_history] = "No Accidents"
        ),
        CALCULATE(
            COUNTROWS('Tesla Stock Dataset'),
            'Tesla Stock Dataset'[model] = selected
        ),
        0
    )
)
```

Visuals were color-coded by driver assist systems and filtered by model.


### Power BI Dashboard
Each Tesla model has a dedicated dashboard featuring the following key visuals:
- Average Price
- Median Odometer
- EMI
- % With/Without Accident
- Price by State
- Mileage by State
- Average Price by Driver Assist System (DAS)
- 3D Vehicle View (UI/UX enhancement)
<img width="843" height="449" alt="1" src="https://github.com/user-attachments/assets/1b85c6d0-3a99-4ea8-96a6-c38a2b58306b" />
<img width="842" height="452" alt="2" src="https://github.com/user-attachments/assets/80248e72-11ab-4c2b-a52d-b4f44a4c4b46" />
<img width="841" height="452" alt="3" src="https://github.com/user-attachments/assets/65714f3e-b6c5-4101-b44e-cdfaefe5c7db" />
<img width="842" height="449" alt="4" src="https://github.com/user-attachments/assets/88878d50-c9a9-402d-bb7f-ca3d6ec01785" />
<img width="843" height="453" alt="5" src="https://github.com/user-attachments/assets/21e965b0-8844-47a3-8393-dd100ec86823" />

### Insights and Findings
The insights and findings are summarized as follows:
1. Model Y is the most affordable option at ~$33.9K with the lowest EMI ($529.37).
2. Model X has the highest average price and EMI, making it the most premium used Tesla option.
3. Model 3 is the most balanced in terms of price vs. mileage.
4. Cybertruck data suggests it's new to the market (lower mileage) but priced mid-range.
5. States like NY, OH, NC, VA appear across the top for mileage, suggesting high usage or resales.
6. Accident-free rate is high across all models, with Model 3 and Y being the safest (>90%).

### Recommendations
#### 1. Target High-Volume States:
Focus marketing and resale efforts in states like NY, OH, NC, VA, which show high vehicle turnover and mileage demand.

#### 2. Price Based on DAS
Use Driver Assist System tiers as a pricing lever.

#### 3. Promote Lower EMI Models
Model Y and Model 3 with lower EMIs may be ideal for entry-level EV buyers or fleet purchases.

#### 4. Expand Dataset for Model Y & Cybertruck
As newer models, their datasets will grow. Monitoring pricing trends can help forecast depreciation and resale value.

### Future Work
- Integrate live market data (Tesla API, vehicle resale sites)
- Include model year and battery degradation metrics
- Predict resale value using regression models (Python + ML)
- Build a web-based dashboard using Power BI Embedded or Streamlit
- Analyze charging patterns and locations (if location data becomes available)
