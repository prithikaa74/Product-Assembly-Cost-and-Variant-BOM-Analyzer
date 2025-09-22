## Project Overview:
This project demonstrates a Medallion Architecture ETL pipeline using PySpark in Fabric, transforming raw data into actionable insights and visualizing them in Power BI.

The pipeline covers:

Bronze Layer – Raw data ingestion from Excel/CSV
Silver Layer – Cleaned and structured data
Gold Layer – Aggregated and enriched data ready for analytics
Power BI Dashboard – Interactive visuals and KPIs for insights

## Logic Explanation (ETL / Medallion Layers):

Bronze Layer

Purpose: Store raw data as-is from source files (Components.csv and ProductBOM.csv).
Actions:
Load Excel/CSV files into Spark DataFrame.
Store in Bronze folder in Fabric Lakehouse as Delta tables.
No transformations; preserves all original columns and values.

Silver Layer

Purpose: Clean and standardize the raw data for processing.
Actions:
Handle missing/null values.
Remove duplicates or invalid rows.
Rename columns for clarity (e.g., component_id, product_id).
Output: Silver Delta table with structured, validated data.

Gold Layer

Purpose: Create aggregated/enriched datasets for analytics and reporting.
Actions:
Join Silver tables to combine related datasets (Components + ProductBOM).
Compute business metrics (e.g., total cost per product, total components, quantity needed).
Filter or sort data as required for reporting.
Output: Gold Delta table ready for Power BI visualization.

## Validation of Totals
To ensure correctness of ETL transformations:

Record Count Validation:
Compare row counts between layers to ensure no unexpected loss.

print("Bronze count:", bronze_df.count())
print("Silver count:", silver_df.count())
print("Gold count:", gold_df.count())

Sum / Metric Validation:
For numerical columns (e.g., quantity, cost), validate totals:

print("Total quantity in Bronze:", bronze_df.agg({"quantity": "sum"}).show())
print("Total quantity in Silver:", silver_df.agg({"quantity": "sum"}).show())
print("Total quantity in Gold:", gold_df.agg({"quantity_needed": "sum"}).show())

Ensure the aggregated totals in Gold match expected business logic.

Spot Checks:
Randomly check a few sample products/components to ensure calculations are correct.
Compare totals in Power BI KPIs with Gold table metrics.

## Technologies Used

PySpark – Data processing & transformations
Fabric – Data storage & Medallion architecture
Power BI – Data visualization & dashboards
