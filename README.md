# Ecommerce Data Insights - Detailed Code Overview

This project analyzes the [Olist Brazilian Ecommerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) using Databricks for data engineering and business insights derivation. It follows the Udemy course "Databricks | Spark ETL & Delta Lake Data Engineering Mastery" by Oak Academy. The notebooks implement a medallion architecture (Bronze → Silver → Gold layers) for data processing, focusing on the Gold layer for business-ready analytics.

## Project Structure

- **README.md**: Project introduction and overview.
- **Data_Engineering_Gold_Layer_olist_1.ipynb**: Customer and seller distribution analysis, product category summaries.
- **Data_Engineering_Gold_Layer_olist_2.ipynb**: Order-level analytics, unified performance metrics, and user-defined functions (UDFs).

All code uses Databricks SQL and PySpark, assuming pre-processed Silver layer tables in `course_training_catalog.silver_olist`. Output tables are stored in `course_training_catalog.gold_olist` as Delta tables.

## Data_Engineering_Gold_Layer_olist_1.ipynb: Customer & Product Analytics

This notebook focuses on geographic and product-level insights from the Gold layer.

### Customer Distribution Analysis
- **Goal**: Analyze customer spread by state and city, including diversity metrics (distinct cities, average customers per city, top city per state).
- **Key Query**: Creates `customer_distribution_by_state` table with state-level summaries:
  - Distinct cities count
  - Total customers
  - Average customers per city
  - Top city and its customer count
- **SQL Logic**: Uses CTEs for city counts, ranking (ROW_NUMBER), and aggregation. Orders by total customers descending.
- **Output Table**: `course_training_catalog.gold_olist.customer_distribution_by_state`

### Seller Metrics and Pareto Analysis
- **Goal**: Analyze seller distribution by city/state, with Pareto (80/20) ranking for concentration analysis.
- **Processing**:
  - Cleans seller data (capitalizes cities, uppercases states).
  - Aggregates seller counts per city/state.
  - Computes dense ranks and cumulative shares.
- **Key Query**: Creates `seller_city_stats` with rank, cumulative share for top sellers.
- **Output Table**: `course_training_catalog.gold_olist.seller_city_stats`
- **Insights**: Top 20 cities by seller count, Pareto visualization prep.

### Product Category Summary
- **Goal**: Summarize product dimensions and density by category.
- **Processing** (PySpark):
  - Groups by `product_category_name`.
  - Calculates averages: weight, length, height, width, volume, density (weight/volume).
  - Filters out null categories, orders by descending weight.
- **Output Table**: `course_training_catalog.gold_olist.product_category_summary`
- **Enhancement**: Joins with `category_lookup` for English translations, creates display-friendly names.
- **Final Table**: `course_training_catalog.gold_olist.product_category_summary_en`
- **Insights**: Top categories by weight/volume (e.g., heavy items like furniture).

## Data_Engineering_Gold_Layer_olist_2.ipynb: Order Performance & Analytics

This notebook builds comprehensive order-level insights by unifying multiple tables.

### Data Exploration
- **Tables Covered**: `order_items`, `order_payments`, `order_reviews`, `geolocation`, `reviews_bad_rows`, `orders`.
- **Proposed Analyses** (documented in markdown):
  - Order items: Average price/freight, freight ratios, total item value, shipping trends.
  - Payments: Method distribution, installment analysis, payment volumes.
  - Reviews: Score distribution, sentiment (positive/negative), missing comments.
  - Geolocation: Zip density, centroid validation.
  - Bad rows: Error ratios, types, fixable issues.
  - Orders: Status breakdown, time-of-day/weekend patterns, timeflow integrity, completeness, seasonality, delivery benchmarks.

### Unified Order Performance Table
- **Goal**: Create a single Gold table combining financial, operational, and satisfaction data per order.
- **Components**:
  - **Item Summary**: Total order value (price + freight), item count.
  - **Payment Summary**: Payment type, total payment, average installments.
  - **Review Summary**: Average review score, review count.
  - **Base**: Orders table with purchase date, status, delivery days, delay days.
- **SQL Logic**: CTEs for summaries, LEFT JOINs on `order_id`.
- **Output Table**: `course_training_catalog.gold_olist.order_performance`
- **Sample Queries**: Full table select, average order value calculation.

### User-Defined Functions (UDFs)
- **Schema**: Creates `course_training_catalog.analytics_fn` for reusable functions.
- **Scalar Function**: `avg_order_value()` - Returns average order value.
- **Table-Valued Function**: `order_peformance_summary(payment_filter, min_order_value, max_delivery_days)` - Filtered summaries by payment type, value thresholds, delivery days.
  - Returns: Payment type, order count, avg value/review/delivery days.
  - Examples: All orders, credit card orders ≥$100 with ≤5 delivery days.
- **Purpose**: Centralized, versioned functions for consistent calculations across reports.

### Delivery Speed vs. Satisfaction Analysis
- **Goal**: Analyze how delivery time impacts customer reviews.
- **Query**: Bins delivery days into categories (0-3, 4-7, 8-14, 15+ days), aggregates orders, avg value, avg review score.
- **Insights**: Correlation between faster delivery and higher satisfaction.

## Key Technologies & Patterns
- **Databricks**: SQL analytics, PySpark for complex aggregations, Delta Lake for storage.
- **Medallion Architecture**: Builds on Silver layer, produces Gold tables for BI/reporting.
- **Best Practices**: CTEs for readability, LEFT JOINs for completeness, ROUND() for clean outputs, parameterized functions for flexibility.
- **Data Quality**: Handles nulls, averages, rankings; includes bad rows analysis.
- **Business Value**: Enables insights on customer behavior, seller performance, product trends, order efficiency, and satisfaction drivers.

## Prerequisites & Setup
- Databricks workspace with Unity Catalog enabled.
- Silver layer tables pre-processed (cleaned, typed, deduplicated).
- Run notebooks in sequence; none executed yet (as-is for learning/review).

## Next Steps
- Execute cells in Databricks to validate and visualize results.
- Extend with dashboards (e.g., Tableau/Power BI) on Gold tables.
- Add more UDFs or ML models for predictions (e.g., delivery delay forecasting).

For full code, refer to the notebooks. This provides a comprehensive view of the ETL and analytics pipeline.