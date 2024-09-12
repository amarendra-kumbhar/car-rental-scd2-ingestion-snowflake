## Project: Car Rental Data Batch Ingestion with SCD2 Merge in Snowflake

### Problem Statement
This project aims to build a batch ingestion pipeline for car rental data with SCD2 merge logic in Snowflake. 
The pipeline reads daily car rental and customer data from Google Storage, performs transformations using PySpark and updates the data in Snowflake using Airflow.

### Tech Stack
- **Python**
- **PySpark**
- **GCP Dataproc**
- **Airflow**
- **Snowflake**

### Project Overview
1. **Static Table Creation:** Create `location_dim`, `date_dim`, `car_dim` static tables in Snowflake.
2. **SCD2 Merge Table:** Create table and schema for `customer_dim` to perform SCD2 merge in Snowflake.
3. **Fact Table:** Create table and schema for `rentals_fact` in Snowflake.
4. **PySpark Job:** Read dimension tables from Snowflake, transform data, and append processed data to `rentals_fact`.
5. **Airflow DAG:** Orchestrate the workflow to:
   - Read daily data from Google Storage.
   - Perform SCD2 merge on `customer_dim`.
   - Trigger PySpark job on Dataproc.

### Project Structure
- **`docs/`**: Contains documentation and diagrams.
- **`src/`**: Source code for data processing and transformations.
- **`dags/`**: Airflow DAGs for orchestration.
- **`data/`**: Contains sample data files for testing.

### Installation and Setup
1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/car-rental-scd2-ingestion-snowflake.git
