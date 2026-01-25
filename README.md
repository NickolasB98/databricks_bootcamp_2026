# Databricks Lakehouse Bootcamp 2026 – Sales CRM & ERP Data Lakehouse

Modern end‑to‑end **lakehouse** project on Databricks that takes raw CRM and ERP files and turns them into production‑style, analytics‑ready data products.

The solution uses the **Medallion architecture** (Bronze–Silver–Gold), orchestrated **Jobs** for running the pipeline every day, along with a **SQL-based dashboard** on top of high‑quality Gold tables that also gets refreshed with new data.  

On top of this foundation, a **Gold Sales Genie** AI assistant is trained on the Gold schema so business users can ask natural‑language questions (e.g. “Which products drive revenue growth this quarter?”) and get answers directly from governed data.  

This project demonstrates how to combine data engineering, analytics, and applied AI in a single Databricks lakehouse.

[Gold Business Dashboard 2026-01-24 22_58.pdf](https://github.com/user-attachments/files/24840502/Gold.Business.Dashboard.2026-01-24.22_58.pdf)

<img width="2522" height="1604" alt="image" src="https://github.com/user-attachments/assets/3494f29d-49f1-4f9b-8c25-57882f3c9316" />

<img width="2542" height="1120" alt="image" src="https://github.com/user-attachments/assets/9481f284-d5a3-41f6-8260-9e5387cd410e" />


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

A mid-sized retail company wants to move away from siloed CRM and ERP reports and build a single source of truth for sales and customer performance.  
Today, sales, customer, product, and location data live in separate systems, making it hard to answer simple questions like “Which products drive revenue in each region?” or “Which customers are growing vs. declining?”.

This project uses a **Databricks Lakehouse** to:

- Land raw CRM and ERP exports in a unified platform.  
- Clean and standardize them into **Silver** tables that agree on keys, naming, and data types.  
- Model a **Gold star schema** (`fact_sales`, `dim_customers`, `dim_products`, locations) that directly supports business KPIs.  
- Power a **SQL-based sales dashboard** for real-time insights on revenue, top products, and key customers.  
- Expose the Gold layer to a **Genie AI assistant**, so business users can ask natural‑language questions and get answers from governed, high‑quality data.

The result is a realistic retail analytics environment that shows how modern teams can go from raw files to automated pipelines, dashboards, and AI-assisted self‑service analytics on one lakehouse platform.


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

## 5. Jobs Orchestration

<img width="2440" height="1348" alt="image" src="https://github.com/user-attachments/assets/1ccdbe20-b4e4-4300-97fb-eed6e2b3456f" />

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

## 6. Gold Sales Genie (AI Assistant)

<img width="2444" height="1184" alt="image" src="https://github.com/user-attachments/assets/eaf04956-9b7d-4364-ac4f-e46392f80919" />

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

### 7. About

Built by Nikolas Biniaris as a combined project for the Databricks Data Engineering and Data Analytics bootcamps, organised by Data with Baara, focusing on practical databricks lakehouse architecture, SQL, and analytics.

---

## 🛡️ License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and share this project with proper attribution.
