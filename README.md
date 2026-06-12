1) 🏭 Sales Data Warehouse — Star Schema Pipeline
Built on **Databricks** using SQL | Star Schema | Incremental Load

## 📐 Architecture
Source Orders → Staging → Transformation → Dimensions → FactSales

## 📂 Notebooks
| File | What it does |
|------|-------------|
| `Data_modeling.ipynb` | Builds 4 Dimension tables + FactSales |


## 🛠️ Tech Stack
Databricks · SQL · Delta Lake · Star Schema · ETL

## 🔄 Pipeline
1. Stage raw orders
2. Transform (filter nulls)
3. Build DimCustomers, DimProducts, DimRegions, DimTime
4. Build FactSales by joining all dimension surrogate keys
5. Incremental load — only new records processed each run


2)# Slowly Changing Dimensions (SCD) Type 1 Implementation in Databricks

This repository contains a Databricks notebook (`Scd_Data_Warehouse.ipynb`) that demonstrates how to design and execute a **Slowly Changing Dimension (SCD) Type 1** pipeline using SQL and Delta Lake functionality (like the `MERGE` statement).

## 📌 Project Overview

In data warehousing, a **Slowly Changing Dimension (SCD)** handles data that changes over time. 
* **SCD Type 1** focus on **overwriting** existing data. 
* When a record's attributes change in the source system, the old records in the dimension table are replaced with the new values.
* **History is not preserved.** The dimension table always reflects the most up-to-date "current state" of the data.

This project simulates an incremental data load of a `Product` dimension from an incoming transactional `Orders` table.

---

## 🏗️ Architecture & Flow

The pipeline follows a classic ELT (Extract, Load, Transform) pattern leveraging Databricks Delta Lake features:

1. **Source Data (`sales_scd.Orders`)**: A transactional table that stores daily sales order records containing customer, product, and regional attributes.
2. **Staging / Transformation View (`sales_scd.view_DimProducts`)**: Isolates new or updated product records based on specific business rules (e.g., filtering for recent dates and pulling unique `ProductID` combinations).
3. **Target Dimension (`sales_scd.DimProducts`)**: The final, clean, enterprise dimension table containing a unique list of products.
4. **The Merge Engine (SCD 1 Logic)**: A `MERGE INTO` script that synchronizes the target table with the source view.

---

## 💻 Technical Implementation (SQL Logic)

The core engine of this project relies on the Delta Lake `MERGE` statement to handle the "upsert" (Update + Insert) logic seamlessly:

```sql
MERGE INTO sales_scd.DimProducts AS target
USING sales_scd.view_DimProducts AS source
ON target.ProductID = source.ProductID
WHEN MATCHED THEN
  UPDATE SET *
WHEN NOT MATCHED THEN
  INSERT *
