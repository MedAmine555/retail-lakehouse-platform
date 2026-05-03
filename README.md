#  Unified Retail Lakehouse Platform

## End-to-End Data Engineering Project (Databricks + Delta Lake)

> Designed and implemented a scalable lakehouse architecture to integrate ERP, CRM, and relational data into a unified analytics platform.

---

##  Project Overview

Modern retail systems often operate with fragmented data across multiple sources (ERP, CRM, databases), making analytics complex and unreliable.

This project demonstrates how to design and build a **Lakehouse architecture on Databricks**, transforming raw multi-source data into clean, business-ready datasets.

---

## 🧱 Architecture


<img width="1536" height="1024" alt="b1b66313-5117-4e6a-8b42-7c825ddf9622" src="https://github.com/user-attachments/assets/755e8dc2-240d-487a-8466-3138ba5a9639" />



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

## 🧩 Data Modeling (Star Schema)

The Gold layer is designed using a **Star Schema** to support analytical queries and reporting.

### ⭐ Fact Table

**`gold_fact_sales`**

* `order_id`
* `order_date`
* `customer_id`
* `product_id`
* `quantity`
* `unit_price`
* `total_amount`

➡️ Contains transactional data at the order level

---

### 📐 Dimension Tables

**`dim_customer`**

* `customer_id`
* `customer_name`
* `loyalty_tier`

**`dim_product`**

* `product_id`
* `product_name`
* `category`

**`dim_date`**

* `date`
* `year`
* `month`

➡️ Provide descriptive context for analysis

---

### 🔗 Relationships

* `fact_sales.customer_id → dim_customer.customer_id`
* `fact_sales.product_id → dim_product.product_id`
* `fact_sales.order_date → dim_date.date`

---

### 🎯 Purpose

This model enables:

* Efficient aggregation (revenue, orders, KPIs)
* Simplified querying for analytics
* Clear separation between facts and dimensions

---



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

<img width="1822" height="1120" alt="Capture d&#39;écran 2026-05-03 172306" src="https://github.com/user-attachments/assets/70e517e7-a509-419d-a8a9-37971723261e" />

Orchestrated using Databricks Workflows
End-to-end execution: Bronze → Silver → Gold
Parallel ingestion and transformation tasks


## 🟡 Gold Layer — Fact Table (Sales)

<img width="1904" height="836" alt="f15927a0-086e-40b1-8a89-782c2f32306d" src="https://github.com/user-attachments/assets/422f9bea-df05-46af-84bf-1eb1fcb892de" />

Final business-ready dataset
Includes enriched fields (customer, product, category)
Supports analytical queries and reporting

## 👤 Gold Layer — Customer Performance

<img width="1860" height="585" alt="9d4444ad-ffce-474a-a33d-ff19465564b8" src="https://github.com/user-attachments/assets/a92fe652-664c-4474-9eca-c50d5213c0d0" />

Aggregated customer metrics:

Total revenue
Total orders
Customer lifetime

## 📊 Databricks Visualizations

<img width="1815" height="938" alt="Capture d&#39;écran 2026-05-03 172043" src="https://github.com/user-attachments/assets/546ced5e-4703-49d5-8b11-f085923cafd6" />


Revenue trends over time
Top-performing products
Revenue by category
Built directly using Databricks SQL visualizations






