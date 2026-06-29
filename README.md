Azure-Sales-Analytics-Platform-An-End-to-End-Data-Warehouse-Project
##Project Summary This project demonstrates a complete, end-to-end data engineering pipeline built entirely on the Microsoft Azure cloud platform. The goal is to ingest raw, transactional sales data from multiple sources, process it through a multi-layered data lake, model it into an analytics-ready Star Schema, and serve it for business intelligence and reporting.

Architecture The pipeline follows the modern ELT (Extract, Load, Transform) paradigm using a Medallion Architecture to ensure data quality and governance, moving data from a raw Bronze layer to a clean Silver layer, and finally to an aggregated Gold layer ready for analytics.

Project Structure
Technologies Used

image
How It Works

Data Ingestion (Incremental Loading)
An Azure Data Factory pipeline orchestrates the entire process. It uses a watermark technique to incrementally load only new or updated data from the source, making the process efficient and scalable. The pipeline includes lookups to determine the data range, a copy activity to move the data, and a stored procedure to update the watermark upon successful completion. connection to make parquet

Incremental pipeline
Data Transformation (Bronze to Gold) Once the raw data is landed in the Bronze layer of the Data Lake, Azure Databricks takes over the transformation using PySpark notebooks.
Silver Layer: The raw data is cleaned, validated, and enriched. It's queried here directly from the Parquet files stored in the data lake. image Databricks querying on silver data

Gold Layer: The Silver layer data is then modeled into a Star Schema, consisting of four dimension tables and one central fact table. This is orchestrated using a Databricks Job that runs the notebooks in the correct order of dependency.

Azure pipeline image
The final fact_sales table contains surrogate keys and numeric measures, optimized for fast BI queries.

fact table
Data Serving & Visualization The final, aggregated Gold layer data is now fully prepared for analytics and reporting. The star schema model allows for high-performance querying and is served through two primary methods:
Ad-hoc Analysis with Databricks SQL: For technical users and data analysts, the Gold layer tables can be queried directly within the Databricks environment using standard SQL to perform complex, ad-hoc analysis. databricks sales visualization

Business Intelligence with Power BI: For business stakeholders, the data was connected to Microsoft Power BI to build a professional, interactive dashboard. This dashboard provides a user-friendly interface to explore key metrics, filter by different dimensions, and derive actionable insights from the data warehouse.

powerbi complete vis
Key Learnings & Concepts Implemented Incremental Data Loading: Designing efficient pipelines that only process new data.

Medallion Architecture: Structuring the data lake into Bronze (raw), Silver (clean), and Gold (aggregated) layers.

Dimensional Modeling: Building a Star Schema with Fact and Dimension tables to optimize for analytics.

Slowly Changing Dimensions (SCD Type 1): Using MERGE operations in Databricks to handle updates to dimension data.

Infrastructure as Code (IaC) Principles: The entire environment can be defined and deployed through templates.

Cloud Data Governance: Using Unity Catalog to manage tables and security.

Repository Contents
Notebooks: This repository contains the complete PySpark source code (.py files) for all the Azure Databricks notebooks used to perform the data transformations.

Screenshots: The README.md file is illustrated with screenshots that showcase the pipeline architecture, successful job runs, and final data models to provide a visual walkthrough of the project from end to end.
