# 🛍️ Cloud Azure Retails — End-to-End Data Engineering Pipeline

A cloud-based data engineering project built on **Azure Databricks** and **Azure Data Lake Storage Gen2**, implementing a full **Medallion Architecture** (Bronze → Silver → Gold) for a multi-country retail business across the Middle East.

---

## 📌 Project Overview

This project ingests raw retail data from Azure Data Lake, transforms it through layered processing, and produces business-ready analytics tables ready for BI tools like Power BI.

**Regions covered:** Egypt 🇪🇬 · Saudi Arabia 🇸🇦 · UAE 🇦🇪 · Qatar 🇶🇦 · Kuwait 🇰🇼

---

## 🏗️ Architecture

```
Azure Data Lake (ADLS Gen2)
        │
        ▼
   ┌─────────┐
   │ BRONZE  │  Raw Parquet files loaded into Databricks tables
   └────┬────┘
        │
        ▼
   ┌─────────┐
   │ SILVER  │  Cleaned, cast, and joined into a unified fact table
   └────┬────┘
        │
        ▼
   ┌─────────┐
   │  GOLD   │  Aggregated analytical tables for reporting
   └─────────┘
```

---

## 📂 Dataset

| Table | Description |
|---|---|
| `stores` | 5 stores across UAE, Saudi Arabia, Qatar, Egypt, Kuwait |
| `products` | 10 products across Electronics, Fitness, Accessories, Stationery |
| `transactions` | Sales transactions with quantity and date |
| `customers` | 50 customers with country and registration info |

---

## 🔄 Pipeline Layers

### 🟤 Bronze Layer
Raw data loaded directly from ADLS Gen2 into Databricks Delta tables:
- `retails_section.sales.stores`
- `retails_section.sales.products`
- `retails_section.sales.transactions`
- `retails_section.sales.customers` *(with type casting)*

### 🥈 Silver Layer
All four tables joined into one enriched fact table:
- `retails_section.sales.ratials` — unified transactions with full customer, product, and store details

### 🥇 Gold Layer — Analytics Tables

| Gold Table | Description |
|---|---|
| `sales_by_country_gold` | Total sales count per country |
| `product_performance_gold` | Revenue and quantity sold per product |
| `store_product_performance_gold` | Revenue per product per store |
| `store_customer_analysis_gold` | Unique customers and avg transactions per store |
| `customer_product_affinity_gold` | Purchase frequency per customer-product pair |
| `monthly_sales_gold` | Revenue and transaction trends by month/quarter |
| `country_detailed_analysis_gold` | Full KPIs per country (revenue, customers, AOV) |
| `category_country_performance_gold` | Revenue by product category per country |
| `price_segment_analysis_gold` | Budget / Mid-Range / Premium segment breakdown |
| `seasonal_analysis_gold` | Revenue by season (Spring/Summer/Fall/Winter) |
| `customer_registration_trends_gold` | New customer signups by month and country |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Azure Databricks | Compute & notebook environment |
| Azure Data Lake Gen2 | Raw data storage |
| Apache Spark (PySpark) | Distributed data processing |
| Delta Lake | Managed table storage |
| Python | Transformation logic |

---

## 🚀 How to Run

1. **Set up Azure resources**
   - Create an ADLS Gen2 storage account
   - Upload the Parquet files to a container named `retails`
   - Create a Databricks workspace

2. **Configure credentials securely**
   ```python
   # Use Databricks Secrets (recommended)
   spark.conf.set(
     "fs.azure.account.key.<your-account>.dfs.core.windows.net",
     dbutils.secrets.get(scope="your-scope", key="storage-key")
   )
   ```

3. **Run the notebook** `retails_pipeline.ipynb` top to bottom

4. **Query Gold tables** in Databricks SQL or connect Power BI

---

## 📁 Repository Structure

```
cloud-azure_retails/
│
├── dataset/
│   ├── customers.parquet
│   ├── stores.parquet (via dbo.stores)
│   ├── products.parquet (via dbo.products)
│   └── transactions.parquet (via dbo.transactions)
│
├── retails_pipeline.ipynb     ← Main Databricks notebook
└── README.md
```

---

## ⚠️ Security Note

Never hardcode storage account keys in your notebooks. Use **Azure Key Vault** integrated with **Databricks Secret Scopes** to manage credentials safely.

---

