# azure-formula1
This project endeavors to furnish a data analysis solution tailored for Formula-1 race results through the utilization of Azure Databricks. It constitutes an Extract, Transform, Load (ETL) pipeline designed to intake Formula 1 motor racing data, subsequently transforming it, and then loading it into our data warehouse to facilitate reporting and analytical pursuits. The primary data source is ergast.com, a dedicated repository for Formula 1 statistics, with storage facilitated through Azure Datalake Gen2. Leveraging Azure Databricks, both data transformation and analysis are executed seamlessly. The orchestration of the entire process is achieved through Azure Data Factory.


Architecture diagram
![1](https://github.com/Karangowda/azure-formulae1/assets/42988620/3cc004b5-12f4-45bd-a795-c64bff9e691d)


ER Diagram:
![687474703a2f2f6572676173742e636f6d2f696d616765732f6572676173745f64622e706e67](https://github.com/Karangowda/azure-formulae1/assets/42988620/16bc0dc0-c582-468a-9548-4661236eb789)


Data files
<img width="271" alt="fdsfdsfsfsdfpoiulkjmnh" src="https://github.com/Karangowda/azure-formulae1/assets/42988620/419daa31-592d-484f-b5d4-36d609e138b8">

Azure Data Factory (ADF) orchestrates the execution and monitoring of Azure Databricks notebooks. Data is sourced from the Ergast API and imported into Azure Data Lake Storage Gen2 (ADLS), residing initially in the Bronze zone (landing zone). Utilizing Azure Databricks notebooks, data in the Bronze zone undergoes ingestion and transformation, resulting in delta tables with upsert functionality. ADF then transfers this data to the Silver zone (standardization zone).

In the Silver zone, Azure Databricks SQL notebooks further transform the ingested data, performing table joins and aggregations for analytical and visualization purposes. The transformed output is subsequently loaded into the Gold zone (analytical zone).

The ETL pipeline consists of two stages:

1.Ingestion: Data from the Bronze zone is processed, read using Apache Spark, and saved into a delta table with minimal transformation. This includes actions such as column dropping, header renaming, schema application, and addition of audited columns (ingestion_date and file_source), with the file_date as a dynamic parameter in ADF.

2.Transformation: Preprocessed delta files are read by Databricks SQL, where further transformations occur, including duplicate removal, table joins, and aggregation using window functions, resulting in final dimensional model tables in delta format.

ADF is scheduled to execute every Sunday at 10 PM, with built-in logic to skip execution if no race is scheduled for that week. Additionally, a separate pipeline is established to handle the execution of the ingestion and transformation processes.

Technologies/Tools Used:
Pyspark
Spark SQL
Delta Lake
Azure Databricks
Azure Data Factory
Azure Date Lake Storage Gen2
Azure Key Fault
Power BI

