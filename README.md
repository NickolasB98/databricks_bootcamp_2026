# Databricks Lakehouse Bootcamp 2026 – Engineering & Analytics

End-to-end Databricks lakehouse project that combines the **Data Engineering** and **Data Analytics** bootcamps into a single workflow.  
The project builds a Medallion architecture on top of CRM and ERP data to deliver sales and customer analytics.

---

## 1. Repository Structure

```
datasets/
  engineering/
    source_crm/
      cust_info.csv
      prd_info.csv
      sales_details.csv
    source_erp/
      CUST_AZ12.csv
      LOC_A101.csv
      PX_CAT_G1V2.csv

script/
  init_lakehouse.ipynb
  Explore SalesDB.dbquery.ipynb

  bronze/
    Bronze layer (dictionary).ipynb
    Bronze layer (hard-coded).ipynb

  silver/
    crm/
      silver_crm_cust_info.ipynb
      silver_crm_prd_info.ipynb
      silver_crm_sales_details.ipynb
    erp/
      silver_orchestration.ipynb
      silver_utils.py

  gold/
    gold_dim_customers.ipynb
    gold_dim_products.ipynb
    gold_fact_sales.ipynb
    gold_orchestration.ipynb
datasets/
  engineering/
    source_crm/
      cust_info.csv
      prd_info.csv
      sales_details.csv
    source_erp/
      CUST_AZ12.csv
      LOC_A101.csv
      PX_CAT_G1V2.csv

script/
  init_lakehouse.ipynb
  Explore SalesDB.dbquery.ipynb

  bronze/
    Bronze layer (dictionary).ipynb
    Bronze layer (hard-coded).ipynb

  silver/
    crm/
      silver_crm_cust_info.ipynb
      silver_crm_prd_info.ipynb
      silver_crm_sales_details.ipynb
    erp/
      silver_orchestration.ipynb
      silver_utils.py

  gold/
    gold_dim_customers.ipynb
    gold_dim_products.ipynb
    gold_fact_sales.ipynb
    gold_orchestration.ipynb
```
    
## 2. Business Scenario

This project simulates a retail company that collects data from two operational systems:

* CRM source: customer master data, product master data, and sales transactions.

* ERP source: locations and product hierarchy, used to enrich the CRM sales data.

The goal is to centralize these sources in a Databricks Lakehouse and expose clean, well-modeled tables for sales and customer performance reporting.

## 3. Architecture Overview

The solution follows the Medallion architecture:

- Bronze layer

Raw ingestion of CSV files into Delta tables.

Preserves original structure for traceability.

- Silver layer

Cleans and standardizes CRM and ERP data.

Handles types, null values, naming, and duplicates.

Applies basic data quality checks.

- Gold layer

Builds a star schema with:

- fact_sales

- dim_customers

- dim_products

ERP-based dimensions (e.g., locations, product categories)

Optimized for BI tools and ad‑hoc analytics.

## 4. Engineering Track (Pipelines)

### 4.1 Initialization

Notebook: script/init_lakehouse.ipynb

Configures catalog / schema for the project.

Sets storage locations (e.g., DBFS paths) for tables and checkpoints.

Creates the base database used by all subsequent notebooks.

### 4.2 Bronze Layer

Notebooks:

script/bronze/Bronze layer (dictionary).ipynb

script/bronze/Bronze layer (hard-coded).ipynb

These notebooks:

Ingest CSV files from datasets/engineering/source_crm and datasets/engineering/source_erp.

Load them into Bronze Delta tables.

Demonstrate two ingestion patterns:

Hard‑coded file paths.

Dictionary-driven configuration for reusable pipelines.

### 4.3 Silver Layer

* CRM notebooks:

script/silver/crm/silver_crm_cust_info.ipynb

script/silver/crm/silver_crm_prd_info.ipynb

script/silver/crm/silver_crm_sales_details.ipynb

* ERP notebooks and utilities:

script/silver/erp/silver_orchestration.ipynb

script/silver/erp/silver_utils.py

These components:

Clean and standardize Bronze tables for customers, products, sales, locations, and product categories.

Apply reusable checks defined in silver_utils.py (nulls, duplicates, valid ranges, etc.).

Produce conformed Silver tables that are ready to be joined in the Gold layer.

### 4.4 Gold Layer

Notebooks:

* script/gold/gold_dim_customers.ipynb

* script/gold/gold_dim_products.ipynb

* script/gold/gold_fact_sales.ipynb

* script/gold/gold_orchestration.ipynb

These notebooks:

Build dimension tables for customers and products by combining CRM and ERP attributes.

Build fact_sales by joining Silver CRM sales with ERP enrichment tables.

Use gold_orchestration.ipynb to orchestrate creation of all Gold tables into a consistent star schema.

### 5. Analytics Track
   
Notebook: script/Explore SalesDB.dbquery.ipynb

This notebook focuses on business analysis on top of the Gold layer:

Uses Spark SQL / Databricks SQL to query fact_sales and dimensions.

Computes metrics such as:

Revenue by date, product, customer, and location.

Top‑selling products and top customers.

Monthly and weekly sales trends.

Provides example queries that can be turned into Databricks dashboards or exported to Tableau / Power BI.

### 6. Tech Stack

Databricks (Community or Workspace)

PySpark & Spark SQL

Delta Lake

Databricks notebooks and (optionally) Jobs for orchestration

### 7. How to Run

Clone or import the repository

Clone this repo locally or import it into Databricks Repos.

Upload datasets

Upload the contents of datasets/engineering to your Databricks workspace or DBFS.

Update paths in the configuration cell if needed.

Initialize the lakehouse

Open script/init_lakehouse.ipynb.

Set the database name and storage paths.

Run all cells.

Run the pipelines

Run the Bronze notebooks.

Run the Silver CRM and ERP notebooks.

Run the Gold notebooks, or run gold_orchestration.ipynb to build all Gold tables.

Explore the data

Open script/Explore SalesDB.dbquery.ipynb.

Execute the sample queries and adapt them to your own questions.

### 8. Potential Extensions

Add Databricks Jobs to schedule Bronze → Silver → Gold pipelines.

Add richer data quality checks and monitoring.

Connect Tableau or Power BI directly to the Gold tables.

Extend the model with simple forecasting models for sales and demand.

### 9. About

Built by Nikolas Biniaris as a combined project for the Databricks Data Engineering and Data Analytics bootcamps, focusing on practical lakehouse architecture, SQL, and analytics for sales data.

---

## 🛡️ License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and share this project with proper attribution.
