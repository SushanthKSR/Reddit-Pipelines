# Redit Data Pipelining
---
### Sushanth Kesamreddy ###
---
This project provides a comprehensive data pipeline solution to extract, transform, and load (ETL) Reddit data into a Redshift data warehouse. The pipeline leverages a combination of tools and services including Apache Airflow, Celery, PostgreSQL, Amazon S3, AWS Glue, Amazon Athena, and Amazon Redshift.

The pipeline is designed to:

1. Extract data from Reddit using its API.
2. Store the raw data into an S3 bucket from Airflow.
3. Transform the data using AWS Glue and Amazon Athena.
4. Load the transformed data into Amazon Redshift for analytics and querying.

## Architecture

flowchart LR
    %% Left: Data Source & Orchestration
    R[Reddit (Data Source)] --> A[Apache Airflow]
    A --> PG[(PostgreSQL)]
    A --> C[Celery (Docker)]
    A --> S3Raw((S3 Raw Storage))

    %% AWS Subgraph
    subgraph AWS [AWS Services & Data Lake]
        S3Raw --> GC1[Glue Crawler (Raw)]
        S3Trans((S3 Transformed Storage)) --> GC2[Glue Crawler (Transformed)]
        
        GC1 --> DC[Data Catalog]
        GC2 --> DC
        DC --> G[Glue ETL/Transformations]
        G --> S3Trans
        G --> Athena[Amazon Athena]
        G --> Redshift[Amazon Redshift]
    end

    %% Visualization Tools
    subgraph BI [Visualization / Analytics]
        PBI[Power BI]
        QS[Amazon QuickSight]
        TB[Tableau]
        LS[Looker Studio]
    end

    Athena --> PBI
    Athena --> QS
    Athena --> TB
    Athena --> LS

    Redshift --> PBI
    Redshift --> QS
    Redshift --> TB
    Redshift --> LS

## System Setup
1. Clone the repository.
   ```bash
    git clone https://github.com/SushanthKSR/Reddit-Pipelines
   ```
2. Create a virtual environment.
   ```bash
    python3 -m venv venv
   ```
3. Activate the virtual environment.
   ```bash
    source venv/bin/activate
   ```
4. Install the dependencies.
   ```bash
    pip install -r requirements.txt
   ```
5. Rename the configuration file and the credentials to the file.
   ```bash
    mv config/config.conf.example config/config.conf
   ```
6. Starting the containers
   ```bash
    docker-compose up -d
   ```
7. Launch the Airflow web UI.
   ```bash
    open http://localhost:8080
   ```
