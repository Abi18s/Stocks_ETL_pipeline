# Stocks_ETL_pipeline
# 📈 Stock Market ETL Pipeline & Analytics Dashboard

## Overview

This project implements an end-to-end ETL pipeline for ingesting, transforming, and analyzing daily stock market data. The solution automates data loading from cloud storage into Snowflake, applies data quality validations and dimensional modeling, and exposes analytics-ready datasets for Power BI dashboards.

### Technologies Used

- Apache Airflow
- Python
- Snowflake
- AWS S3 / Azure Data Lake Storage (ADLS)
- SQL
- Power BI
- Slack Notifications

---

## Architecture

```text
Stock Market Data Files
          │
          ▼
 AWS S3 / Azure ADLS
          │
          ▼
 External Stage
          │
          ▼
     RAW Layer
          │
          ▼
    CORE Layer
          │
          ▼
  DIM / FACT Tables
          │
          ▼
   Analytics Views
          │
          ▼
    Power BI Dashboard
```

---

## Tech Stack

| Component | Technology |
|------------|------------|
| Orchestration | Apache Airflow |
| Programming | Python |
| Data Warehouse | Snowflake |
| Storage | AWS S3 / Azure ADLS |
| Reporting | Power BI |
| Monitoring | Slack |
| Version Control | GitHub |

---

## Project Structure

```text
stock-market-etl/
│
├── dags/
│   └── stock_etl_dag.py
│
├── sql/
│   ├── raw/
│   ├── core/
│   ├── dimensions/
│   ├── facts/
│   └── views/
│
├── utils/
│   ├── snowflake_utils.py
│   └── slack_utils.py
│
├── config/
│
├── docs/
│
├── requirements.txt
│
└── README.md
```

---

## Data Pipeline Flow

### Step 1: Data Ingestion

Daily stock market files are delivered to:

- AWS S3 Bucket
- Azure Data Lake Storage

Snowflake accesses these files using:

- Storage Integration
- External Stage

Example:

```sql
COPY INTO RAW.RAW_EOD_PRICES
FROM @EOD_STAGE;
```

---

### Step 2: RAW Layer

Purpose:

- Store source data
- Preserve audit trail
- Support troubleshooting

Example Table:

```sql
RAW.RAW_EOD_PRICES
```

Columns:

- Trade Date
- Symbol
- Open
- High
- Low
- Close
- Volume

---

### Step 3: CORE Layer

Purpose:

- Standardize source data
- Remove duplicates
- Apply business rules
- Perform data validation

Example Table:

```sql
CORE.EOD_PRICES
```

Transformations:

- Symbol normalization
- Date standardization
- Data quality validation
- Duplicate removal

---

### Step 4: Dimensional Modeling

#### DIM_SECURITY

Stores unique securities.

| SECURITY_ID | SYMBOL |
|-------------|---------|
| 1 | AAPL |
| 2 | MSFT |

#### DIM_DATE

Stores calendar attributes.

| DATE_KEY | TRADE_DATE |
|----------|------------|
| 20260602 | 2026-06-02 |

#### FACT_EOD_PRICES

Stores daily market activity.

Measures:

- Open Price
- High Price
- Low Price
- Close Price
- Volume
- Traded Value
- Daily Return

---

## Business Metrics

### Trade Volume

Number of shares traded.

```text
Volume = Shares Traded
```

### Traded Value

Dollar value traded during the day.

```text
Traded Value = Close Price × Volume
```

### Daily Return

Daily stock performance.

```text
Daily Return =
(Current Close / Previous Close) - 1
```

### Liquidity

Measures ease of buying and selling securities.

Higher traded value generally indicates higher liquidity.

---

## Analytics Views

### VW_SECURITY_DAILY_PRICES

Provides:

- Security Information
- Sector
- Industry
- Daily Return
- Trading Metrics

---

### VW_WATCHLIST_HISTORY

Tracks watchlist securities:

- AAPL
- MSFT
- NVDA
- AMZN
- GOOGL
- META
- TSLA
- JPM
- XOM
- UNH

Used for:

- Price Trends
- Watchlist Analysis
- Performance Monitoring

---

### VW_SECTOR_LIQUIDITY_LATEST

Calculates:

- Sector Traded Value
- Sector Contribution %
- Number of Securities per Sector

Used for:

- Sector Liquidity Dashboard
- Market Concentration Analysis

---

## Airflow Orchestration

Pipeline Tasks:

```text
Extract Data
      │
      ▼
Load RAW Layer
      │
      ▼
Transform CORE Layer
      │
      ▼
Load Dimensions
      │
      ▼
Load Facts
      │
      ▼
Refresh Analytics Views
      │
      ▼
Slack Notification
```

---

## Monitoring & Alerting

Slack alerts are generated for:

- Task Failures
- DAG Failures
- Pipeline Status Updates

Example:

```text
❌ Airflow Task Failed

DAG: stock_market_etl
Task: load_fact_prices
Run: scheduled_run
```

---

## Data Quality Checks

Implemented validations include:

- Null Checks
- Duplicate Checks
- Price Validation
- Volume Validation
- Trade Date Validation
- Symbol Standardization

---

## Security

### AWS

- IAM Roles
- Storage Integrations

### Azure

- Service Principals
- Storage Integrations

No credentials are hardcoded in source code.

---

## Future Enhancements

- Snowpipe Auto Ingestion
- Incremental Loading
- dbt Transformations
- CI/CD Pipeline
- Real-Time Market Data
- Portfolio Analytics

---

## Dashboard Features

### Market Overview

- Total Securities
- Total Volume
- Total Traded Value
- Average Daily Return

### Sector Analysis

- Sector Liquidity Contribution
- Sector Performance
- Sector Traded Value

### Watchlist Analysis

- Price Trends
- Volume Trends
- Return Analysis

### Security Analysis

- Top Gainers
- Top Losers
- Most Active Stocks

---

## Author

**Abinaya Sundari**

### Skills Demonstrated

- Snowflake
- SQL
- Python
- Apache Airflow
- AWS S3
- Azure ADLS
- Data Warehousing
- Dimensional Modeling
- Power BI
- ETL Development
