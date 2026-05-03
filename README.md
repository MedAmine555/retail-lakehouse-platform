#  Unified Retail Lakehouse Platform

## End-to-End Data Engineering Project (Databricks + Delta Lake)

> Designed and implemented a scalable lakehouse architecture to integrate ERP, CRM, and relational data into a unified analytics platform.

---

##  Project Overview

Modern retail systems often operate with fragmented data across multiple sources (ERP, CRM, databases), making analytics complex and unreliable.

This project demonstrates how to design and build a **Lakehouse architecture on Databricks**, transforming raw multi-source data into clean, business-ready datasets.

---

## 🧱 Architecture


<img width="1536" height="1024" alt="ChatGPT Image 3 mai 2026, 17_14_00" src="https://github.com/user-attachments/assets/a5ab7cef-d238-4b4e-b246-37a5cf43ec6e" />


## ⚙️ Tech Stack

| Layer           | Technology                            |
| --------------- | ------------------------------------- |
| Processing      | PySpark                               |
| Platform        | Databricks                            |
| Storage Format  | Delta Lake                            |
| Data Lake       | Azure Data Lake Gen2 (Simulated)      |
| Ingestion       | Auto Loader (cloudFiles), Batch (CSV) |
| Transformations | Spark SQL                             |
| Orchestration   | Databricks Workflows                  |
| Serving Layer   | Databricks SQL & Visualizations       |

---
## 🧠 Key Engineering Decisions

* **Medallion architecture (Bronze → Silver → Gold)**
  Structured the pipeline to separate raw ingestion, standardized transformations, and business-ready data.

* **ADLS Gen2 (simulated) as landing zone**
  Centralized all source data before ingestion to mimic a real cloud data lake setup.

* **Auto Loader for incremental ingestion**
  Used `cloudFiles` to efficiently process new JSON data (CRM) without reprocessing existing files.

* **Append-only Bronze layer with metadata**
  Preserved raw data and added:

  * `ingestion_timestamp`
  * `source`
  * `file_name`
    to ensure traceability and auditability.

* **Centralized transformations in Silver layer**
  Implemented:

  * Null handling
  * Deduplication
  * Data type standardization
  * Joins across ERP and CRM datasets

* **Business-driven Gold layer design**
  Built curated tables (`gold_daily_sales`, `gold_customer_360`, `gold_product_performance`) aligned with analytics use cases.

* **Data quality validation layer**
  Added checks:

  * No null `customer_id`
  * Revenue ≥ 0
  * No duplicate `order_id`
    Results stored for monitoring.

* **Pipeline orchestration using Databricks Workflows**
  Automated execution sequence: Bronze → Silver → Gold.

---
## 🟤 Bronze Layer (Raw)

* Ingested data as-is from ADLS into Delta tables
* Append-only design
* Metadata added:

  * `ingestion_timestamp`
  * `source`
  * `file_name`

**Goal:** Preserve raw data for traceability and replay
## ⚪ Silver Layer (Cleaned & Conformed)

* Data cleaning:

  * Null handling
  * Deduplication

* Standardization:

  * Column naming
  * Data types

* Data integration:

  * Joined ERP and CRM datasets

**Goal:** Produce reliable and consistent datasets
## 🟡 Gold Layer (Business KPIs)

* Aggregated and business-ready tables:

  * `gold_daily_sales`
  * `gold_customer_360`
  * `gold_product_performance`

* Example KPIs:

  * Daily revenue
  * Top products
  * Customer activity

**Goal:** Enable analytics and reporting
## 🚀 Why This Project Matters

This project reflects real-world data engineering challenges:

* Integrating heterogeneous data sources (ERP, CRM, files)
* Designing scalable ingestion pipelines using Auto Loader and batch processing
* Structuring data using Medallion architecture (Bronze → Silver → Gold)
* Ensuring data quality and consistency across transformations
* Delivering analytics-ready datasets using Databricks SQL

It demonstrates the ability to move from raw data to reliable business insights using modern lakehouse principles.

---

## 🎯 Skills Demonstrated

* Data ingestion (batch + incremental)
* Data modeling (Medallion architecture)
* Data transformation using PySpark
* Handling semi-structured data (JSON)
* Data quality validation
* Pipeline orchestration (Databricks Workflows)
* Analytical data serving (Databricks SQL)

---
## 📸 Pipeline Execution & Analytics

### 🔁 End-to-End Pipeline Orchestration

![Pipeline Workflow](./docs/pipeline_workflow.png)

* Orchestrated using Databricks Workflows
* End-to-end execution: Bronze → Silver → Gold
* Parallel ingestion and transformation tasks

---

### 🟡 Gold Layer — Fact Table (Sales)

![Gold Fact Sales](./docs/gold_fact_sales.png)

* Final business-ready dataset
* Includes enriched fields (customer, product, category)
* Supports analytical queries and reporting

---

### 👤 Gold Layer — Customer Performance

![Customer Performance](./docs/customer_performance.png)

* Aggregated customer metrics:

  * Total revenue
  * Total orders
  * Customer lifetime

---

### 📊 Databricks Visualizations

![Dashboard](./docs/dashboard.png)

* Revenue trends over time

* Top-performing products

* Revenue by category

* Built directly using Databricks SQL visualizations

---
## ▶️ How to Run

### 1. Setup Environment

* Create a Databricks workspace
* Configure access to Azure Data Lake Storage Gen2 (simulated or mounted storage)

---

### 2. Load Data into ADLS (Simulated)

* Upload ERP (CSV) and CRM (JSON) files into the data lake
* Organize data by source (e.g., `/erp/`, `/crm/`)

---

### 3. Run Ingestion (Bronze Layer)

* Execute ingestion notebooks:

  * Batch ingestion for CSV files
  * Auto Loader (`cloudFiles`) for JSON data

---

### 4. Run Transformations (Silver Layer)

* Clean and standardize data:

  * Handle nulls
  * Deduplicate records
  * Cast data types
* Join datasets across sources

---

### 5. Build Business Layer (Gold)

* Execute Gold notebooks to create:

  * `gold_daily_sales`
  * `gold_customer_360`
  * `gold_product_performance`

---

### 6. Run Orchestration

* Trigger Databricks Workflow:

  * Executes full pipeline (Bronze → Silver → Gold)
  * Ensures automated and repeatable runs

---

### 7. Explore Analytics

* Query Gold tables using Databricks SQL
* Use built-in visualizations to explore KPIs

---


