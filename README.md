# ecommerce-data-insights

This folder contains Databricks notebooks for building a layered analytics pipeline on the Olist Brazilian ecommerce dataset. It is focused on data engineering, transformation, and business analytics using the Bronze → Silver → Gold medallion architecture.

## What this folder is about

The workspace is designed to help users:
- ingest and explore raw ecommerce data,
- clean and enrich intermediate Silver datasets,
- create Gold-level analytics for orders, customers, sellers, payments, and reviews,
- build cross-layer analytical joins for business insight.

It is intended for Databricks users who want to learn or implement ecommerce analytics with Delta Lake and PySpark.

## Current folder summary

Files in this folder:
- `README.md`: project overview, folder purpose, and summary.
- `Olist_Bronze.ipynb`: Bronze layer notebook for raw data ingestion and initial exploration.
- `Olist_Silver_1.ipynb`: Silver layer notebook for cleaned and enriched intermediate data.
- `Olist_Silver_2.ipynb`: Additional Silver layer transformations and data shaping.
- `Olist_Gold_1.ipynb`: Gold layer notebook for customer, seller, and product analytics.
- `Olist_Gold_2.ipynb`: Gold layer notebook for unified order performance analytics.
- `Gold_Layer_Analytical_Joins.ipynb`: Analytical joins and advanced gold-layer reporting.
- `.gitignore`: ignore rules for repository metadata.
- `.git/`: git version control metadata.

## Key focus areas

- Databricks notebooks for data engineering and analytics
- Medallion architecture: Bronze → Silver → Gold
- Ecommerce insights on orders, customers, sellers, payments, and reviews
- Delta Lake-based datasets for reporting
- Analytical joins for cross-layer business insights

## Notes

- This folder does not include raw data files.
- Notebooks are meant to be executed in Databricks with appropriate dataset access.
- Follow the notebooks in sequence for a complete ETL and analytics workflow.

## How to run

1. Open the workspace in Databricks.
2. Upload or attach the notebooks to a cluster with access to the Olist dataset.
3. Execute the notebooks in order: `Olist_Bronze.ipynb`, `Olist_Silver_1.ipynb`, `Olist_Silver_2.ipynb`, then `Olist_Gold_1.ipynb`, `Olist_Gold_2.ipynb`, and finally `Gold_Layer_Analytical_Joins.ipynb`.
4. Review the results and confirm Delta tables or analytical outputs are created successfully.
