# Databricks Lakehouse Bootcamp 2026 – Engineering & Analytics

Modern end‑to‑end **lakehouse** project on Databricks that takes raw CRM and ERP files and turns them into production‑style, analytics‑ready data products.  
The solution uses the **Medallion architecture** (Bronze–Silver–Gold), orchestrated **Jobs** for automated pipelines, and a real‑time **SQL dashboard** on top of high‑quality Gold tables.  

On top of this foundation, a **Gold Sales Genie** AI assistant is trained on the Gold schema so business users can ask natural‑language questions (e.g. “Which products drive revenue growth this quarter?”) and get answers directly from governed data.  
This project demonstrates how to combine data engineering, analytics, and applied AI in a single Databricks lakehouse.

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

### Bronze layer

Raw ingestion of CSV files into Delta tables.

Preserves original structure for traceability.

### Silver layer

Cleans and standardizes CRM and ERP data.

Handles types, null values, naming, and duplicates.

Applies basic data quality checks.

### Gold layer

Builds a star schema with:

* fact_sales

* dim_customers
  
* dim_products

ERP-based dimensions (e.g., locations, product categories)

Optimized for BI tools and ad‑hoc analytics.

## 4. Tech Stack

* Databricks 

* PySpark & Spark SQL

* Delta Lake

* Databricks notebooks
  
* Jobs for orchestration

## 10. Jobs Orchestration

The full pipeline is orchestrated with a Databricks Job:

- **Bronze task**  
  - Runs `script/bronze/Bronze layer (dictionary).ipynb`.  
  - Ingests CRM and ERP CSV files into Bronze Delta tables using a parameterized dictionary.

- **Silver task**  
  - Runs `script/silver/erp/silver_orchestration.ipynb`.  
  - Executes the Silver CRM notebooks and ERP transformations to produce cleaned, conformed tables.

- **Gold task**  
  - Runs `script/gold/gold_orchestration.ipynb`.  
  - Builds `dim_customers`, `dim_products`, and `fact_sales` in the Gold schema.

- **Sales_Dashboard task**  
  - Refreshes a Databricks SQL dashboard built on top of the Gold tables.  
  - The dashboard uses SQL queries against `gold.fact_sales`, `gold.dim_customers`, and `gold.dim_products` to show revenue trends, top products, and key customer metrics.

The Job runs these tasks in sequence: **Bronze → Silver → Gold → Sales_Dashboard**.

---

## 11. Gold Sales Genie (AI Assistant)

To enable natural-language access for business users, the project also includes a Databricks Genie AI assistant:

- The **Gold Sales Genie** agent is connected to the three Gold tables:
  - `gold.dim_products`
  - `gold.dim_customers`
  - `gold.fact_sales`
- Business users can ask questions in plain English, for example:
  - “What are the top 10 products by total sales amount this quarter?”  
  - “Which customer segments generate the highest revenue?”  
  - “Describe recent trends in monthly sales.”  
- Genie generates SQL on top of the curated Gold data, ensuring answers are based on high‑quality, governed tables rather than raw sources.

### 11. About

Built by Nikolas Biniaris as a combined project for the Databricks Data Engineering and Data Analytics bootcamps, focusing on practical lakehouse architecture, SQL, and analytics for sales data.

---

## 🛡️ License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and share this project with proper attribution.
