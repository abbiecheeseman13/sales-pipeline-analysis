# Mastercard Pipeline Analytics

Quarterly Business Review style analysis of a B2B sales pipeline, built in Python using a simulated Mastercard Identity Revenue Operations dataset.

This project walks through an end-to-end analysis of commercial pipeline data, including data cleaning, pipeline health KPIs, revenue forecasting, performance by owner and region, and executive-ready visualizations and insights.

> Note: All data in this project is fully simulated for portfolio and learning purposes. It is not sourced from Mastercard or any real customer system.

---

## 1. Project Goals

This notebook is designed to answer the kinds of questions a Revenue Operations or Sales Operations team would bring to a Quarterly Business Review:

- How healthy is the current pipeline in terms of total value and weighted forecast?
- Do we have enough coverage versus targets, and where are the gaps?
- Which stages, regions, and owners are driving performance, and where are bottlenecks?
- How is win rate trending over time?
- Which deals are at risk and should be prioritized before the quarter ends?

The focus is on translating raw opportunity data into clear, decision-ready metrics and visuals that a revenue leader could use in a QBR.

---

## 2. Data

The notebook expects a CSV file named:

- `mastercard_pipeline_dataset.csv`

Key columns used in the analysis:

- `opportunity_id`: Unique identifier for each opportunity  
- `deal_value`: Numeric deal value (pipeline amount)  
- `probability`: Current close probability (0–1)  
- `stage`: Sales stage (for example: Prospecting, Qualified, Proposal, Negotiation, Closed Won, Closed Lost)  
- `region`: Geographic region for the opportunity  
- `owner`: Opportunity owner (rep or pod)  
- `created_date`: Date the opportunity was created  
- `expected_close_date`: Current expected close date  
- `days_in_stage`: Days spent in the current stage  

The notebook also engineers additional features:

- `weighted_pipeline` = `deal_value * probability`
- `days_to_close` = days between today and `expected_close_date`
- `pipeline_age` = days since `created_date`
- `is_overdue` flag for deals beyond expected close date
- `is_stuck` flag for deals sitting in stage longer than a threshold (for example, > 60 days)
- `at_risk` flag combining overdue and stuck deals
- Time features: `created_year`, `created_quarter`, `expected_close_year`, `expected_close_quarter`

---

## 3. Analysis Overview

The notebook is organized into the following sections:

### 1) Setup and Imports

- Loads core analytics stack: `pandas`, `numpy`, `matplotlib`, `seaborn`
- Applies a consistent plotting style for all charts

### 2) Data Loading and Initial Exploration

- Reads `mastercard_pipeline_dataset.csv` into a DataFrame
- Prints:
  - Shape and data types
  - Column names
  - Head of the dataset
  - Categorical distributions for `stage`, `region`, and `owner`

### 3) Data Cleaning and Transformation

- Converts date columns to `datetime`
- Ensures numeric columns are properly typed
- Engineers key features:
  - `weighted_pipeline`
  - `days_to_close`
  - `pipeline_age`
  - Risk flags (`is_overdue`, `is_stuck`, `at_risk`)
  - Quarter and year fields for time-series analysis

### 4) Pipeline Health KPIs

Focuses on *active* opportunities (excluding Closed Won and Closed Lost):

- Total active pipeline value
- Total weighted pipeline
- Active opportunity count
- Pipeline coverage ratio (weighted vs total)
- Stage, region, and owner distributions

This section gives a snapshot of pipeline health at a point in time.

### 5) Revenue Forecasting KPIs

Segments the pipeline into forecast “buckets”:

- **Commit**: High probability deals (for example, ≥ 70 percent)
- **Best Case**: Mid-probability deals (for example, 40–70 percent)
- **Upside**: Low probability deals (< 40 percent)

For each bucket, the notebook calculates:

- Deal count
- Total deal value
- Weighted forecast

This mimics the way a revenue leader would think about “floor vs upside” for the quarter.

### 6) Performance by Dimension

Breaks down performance across:

- **Owner**: Total pipeline, average deal size, weighted pipeline, win rate
- **Region**: Pipeline value, deal count, and win rate by region
- **Time**: Trends by creation quarter and expected close quarter:
  - Pipeline creation over time
  - Closed deals over time
  - Win rate trends by quarter

These tables help answer “who” and “where” questions for pipeline performance.

### 7) Visualizations

Creates QBR-ready visualizations, including:

- Pipeline funnel chart (value by stage)
- Bar charts of opportunity count by stage
- Time-series charts:
  - Pipeline creation trend by quarter
  - Opportunities created by quarter
  - Closed-won value and counts over time
  - Win rate trends by quarter

All charts are intended to be screenshot-ready for slide decks.

### 8) QBR Insights and Recommendations

Automatically prints an **Executive Summary** that pulls together the metrics above:

- Key pipeline totals and coverage ratio
- Top performing owners and regions
- Win rate levels and trends
- Risk summary (overdue and stuck deals)

Then it surfaces concrete recommendations such as:

- Addressing at-risk deals that are overdue or stuck in stage
- Improving probability on low-probability, high-value deals
- Prioritizing high-value late-stage deals to accelerate closing

