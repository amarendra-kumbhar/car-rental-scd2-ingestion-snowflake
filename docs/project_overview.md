# Car Rental Data Batch Ingestion with SCD2 Merge in Snowflake

## Overview
This project implements a batch ingestion pipeline for car rental data using PySpark, Airflow, and Snowflake. 
The pipeline reads daily car rental and customer data from a Google Storage bucket, performs transformations, and uses Slowly Changing Dimension Type 2 (SCD2) logic to maintain data history in Snowflake.

## Project Components

### 1. **Data Ingestion and Transformation with PySpark**
- The PySpark script is responsible for reading data from Snowflake, performing necessary transformations, and appending processed data to the `rentals_fact` table in Snowflake.
- **JAR Files for Spark:** 
  - To enable Snowflake integration with PySpark, necessary JAR files (`snowflake-jdbc`, `spark-snowflake`) are required. These JAR files are uploaded to a Google Cloud Storage (GCS) bucket.
  - In the Spark code, the location of these JAR files is specified using the `--jars` option.
- **Snowflake Connectivity:**
  - The PySpark script includes all the necessary Snowflake connection parameters (account, user, password, database, schema, warehouse) to establish a connection and perform the required data operations.

### 2. **Airflow Orchestration**
- An Airflow DAG is used to orchestrate the entire ETL process, including:
  - Reading parameterized daily car rentals and customer data from the GCS bucket.
  - Performing an SCD2 merge operation on the `customer_dim` table in Snowflake.
  - Triggering a PySpark job on a Dataproc cluster to execute transformations and data ingestion.
- **Snowflake Connection in Airflow:**
  - To execute Snowflake operations, create a Snowflake connection in Airflow using the "Connections" tab. Provide all the Snowflake connection details (connection ID, account, user, password, etc.) under a unique connection name.

### 3. **Configuration Details**

#### **PySpark Configuration**
- **JAR Files:** Ensure the following JAR files are available in the GCS bucket:
  - `snowflake-jdbc.jar`
  - `spark-snowflake.jar`
- **Sample Configuration in PySpark Code:**
  ```python
  spark = SparkSession.builder \
      .appName("Car Rental Data Ingestion") \
      .config("spark.jars", "gs://your-bucket-name/snowflake-jdbc.jar,gs://your-bucket-name/spark-snowflake.jar") \
      .getOrCreate()

  # Snowflake connection properties
  sfOptions = {
      "sfURL": "https://<account>.snowflakecomputing.com",
      "sfUser": "<user>",
      "sfPassword": "<password>",
      "sfDatabase": "<database>",
      "sfSchema": "<schema>",
      "sfWarehouse": "<warehouse>"
  }
