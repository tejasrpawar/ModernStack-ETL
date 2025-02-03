# ModernStack ETL
# ETL to ELT Pipeline using dbt, Snowflake, and Airflow

This project demonstrates building a modern ETL pipeline using dbt, Snowflake, and Apache Airflow. It showcases dimensional modeling, data transformation, testing, and orchestration using popular data engineering tools.

## Overview

The pipeline transforms TPC-H sample data from Snowflake into analytics-ready fact and dimension tables using dbt for transformations and Airflow for orchestration.

## Prerequisites

- Snowflake Account
- Python 3.7+
- pip
- Apache Airflow
- Astronomer CLI
- dbt Core

## Project Structure

```
├── dbt/
│   ├── models/
│   │   ├── staging/
│   │   │   ├── staging_tpch_orders.sql
│   │   │   └── staging_tpch_line_items.sql
│   │   ├── marts/
│   │   │   ├── int_order_items.sql
│   │   │   ├── int_order_items_summary.sql
│   │   │   └── fct_orders.sql
│   │   └── tpch_sources.yml
│   ├── macros/
│   │   └── pricing.sql
│   ├── tests/
│   │   ├── generic/
│   │   │   └── generic_tests.yml
│   │   └── singular/
│   │       ├── fact_orders_discount.sql
│   │       └── fact_orders_date_valid.sql
│   ├── dbt_project.yml
│   └── packages.yml
└── airflow/
    └── dags/
        └── dbt_dag.py
```

## Setup Instructions

### 1. Snowflake Setup

```sql
-- Run as ACCOUNTADMIN
CREATE WAREHOUSE dbt_warehouse WITH WAREHOUSE_SIZE = 'XSMALL';
CREATE DATABASE dbt_db;
CREATE ROLE dbt_role;

-- Grant necessary privileges
GRANT USAGE ON WAREHOUSE dbt_warehouse TO ROLE dbt_role;
GRANT ALL ON DATABASE dbt_db TO ROLE dbt_role;

-- Create schema
USE ROLE dbt_role;
CREATE SCHEMA dbt_db.dbt_schema;
```

### 2. dbt Setup

1. Install dbt Core:
```bash
pip install dbt-core
```

2. Initialize dbt project:
```bash
dbt init
```

3. Install dependencies:
```bash
dbt deps
```

### 3. Airflow Setup

1. Install Astronomer CLI:
```bash
brew install astro
```

2. Initialize Airflow project:
```bash
mkdir dbt_d
cd dbt_d
astro dev init
```

3. Add to requirements.txt:
```
astronomer-cosmos
apache-airflow-providers-snowflake
```

4. Configure Snowflake connection in Airflow:
- Connection ID: snowflake_con
- Connection Type: Snowflake
- Account: your_account
- Warehouse: dbt_warehouse
- Database: dbt_db
- Role: dbt_role
- Login: your_username
- Password: your_password

## Running the Pipeline

### Local Development

1. Run dbt models:
```bash
dbt run
```

2. Run dbt tests:
```bash
dbt test
```

### Airflow Orchestration

1. Start Airflow:
```bash
astro dev start
```

2. Access Airflow UI at http://localhost:8080
   - Username: admin
   - Password: admin

3. Trigger the DAG manually or wait for the scheduled run

## Project Components

### Data Models

- **Staging Models**: Clean, renamed versions of source tables
- **Intermediate Models**: Transformed and aggregated data
- **Fact Tables**: Final analytical tables with business metrics

### Tests

- **Generic Tests**: Column constraints like uniqueness and referential integrity
- **Singular Tests**: Custom SQL tests for business rules validation

### Macros

Custom functions for reusable business logic, including:
- Discount calculations
- Price transformations


Airflow window: (/dag.png)
