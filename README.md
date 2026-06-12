# 🏭 Sales Data Warehouse — Star Schema Pipeline
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
