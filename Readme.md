# Microsoft Certified: Azure Data Engineer Associate

Demonstrate understanding of common data engineering tasks to implement and manage data engineering workloads on Microsoft Azure, using a number of Azure services.

Last course update: **11/02/2023**

## Overview

You should have subject matter expertise in integrating, transforming, and consolidating data from various structured, unstructured, and streaming data systems into a suitable schema for building analytics solutions.

As an Azure data engineer, you help stakeholders understand the data through exploration, and build and maintain secure and compliant data processing pipelines by using different tools and techniques. You use various Azure data services and frameworks to store and produce cleansed and enhanced datasets for analysis. This data store can be designed with different architecture patterns based on business requirements, including:

 - Modern data warehouse (MDW)
 - Big data
 - Lakehouse architecture

As an Azure data engineer, you also help to ensure that the operationalization of data pipelines and data stores are high-performing, efficient, organized, and reliable, given a set of business requirements and constraints. You help to identify and troubleshoot operational and data quality issues. You also design, implement, monitor, and optimize data platforms to meet the data pipelines.

As a candidate for this certification, you must have solid knowledge of data processing languages, including:

 - SQL
 - Python
 - Scala

You need to understand parallel processing and data architecture patterns. You should be proficient in using the following to create data processing solutions:

 - Azure Data Factory
 - Azure Synapse Analytics
 - Azure Stream Analytics
 - Azure Event Hubs
 - Azure Data Lake Storage
 - Azure Databricks

### Skills measured

 - Design and implement data storage
 - Develop data processing
 - Secure, monitor, and optimize data storage and data processing


## COURSE

Training in this course with answered knowledge check

1. [Introduction to data engineering on Azure](./introduction_data_engineering.md)
2. [Introduction to Azure Data Lake Storage Gen2](./introduction_data_lake_storage.md)
3. [Introduction to Azure Synapse Analytics](./introduction_synapse_analytics.md)
4. [Use Azure Synapse serverless SQL pool to query files in a data lake](./use_synapse_sql_query_data_lake.md)
5. [Use Azure Synapse serverless SQL pool to transform data in a data lake](./use_synapse_sql_transform_data_lake.md)
6. [Create a lake database in Azure Synapse Analytics](./create_lake_db_synapse_analytics.md)
7. [Analyze data with Apache Spark in Azure Synapse Analytics](./analyze_data_apache_spark_in_synapse.md)
8. [Transform data with Spark in Azure Synapse Analytics](./transform_data_spark_in_synapse.md)
9. [Use Delta Lake in Azure Synapse Analytics](./use_delta_lake_in_synapse.md)
10. [Analyze data in a relational data warehouse](./analyze_data_in_data_warehouse.md)
11. [Load data into a relational data warehouse](./load_data_into_warehouse.md)
12. [Build a data pipeline in Azure Synapse Analytics](./build_pipeline_in_synapse.md)
13. [Use Spark Notebooks in an Azure Synapse Pipeline](./use_spark_notebook_synapse_pipeline.md)
14. [Plan hybrid transactional and ananlytical processing in Azure Synapse Analytics](./plan_hybrid_transactional_analytical_process.md)
15. [Implement Azure Synapse Link with Azure Cosmo DB](./implement_synapse_link_with_cosmodb.md)
16. [Implement Azure Synapse Link for SQL](./implement_synapse_link_for_sql.md)
18. [Get started with Azure Stream Analytics](./get_started_azure_stream_analytics.md)
19. [Ingest streaming data using Azure Stream Analytics and Azure Synapse Analytics](./ingest_streaming_data_stream_and_synapse.md)
20. [Visualize real-time data with Azure Stream Analytics and Power BI](./visualize_real_time_data_stream_and_powerbi.md)
21. [Introduction to Microsoft Purview](./introduction_microsoft_pureview.md)
22. [Integrate Microsfot Pureview and Azure Synapse Analytics](./integrate_pureview_and_synapse.md)
23. [Explore Azure Databricks](./explore_azure_databricks.md)
24. [Use Apache Spark in Azure Databricks](./use_spark_in_databricks.md)
25. [Run Azure Databricks Notebooks with Azure Data Factory](./run_databricks_notebooks_in_data_factory.md)



## Practice assessment for exam DP-203: Data Engineering on Microsoft Azure

[Practice assessment for exam DP-203: Data Engineering on Microsoft Azure](./prep_exam.md)

[Advanced questions](./prep_exam_2.md)

## Azure Services Map

```bash
+-----------------------------+
|         Azure Portal        |
+-----------------------------+
               |
               v
+-----------------------------+        +----------------------------+
|      Azure Active Directory | <----> | Azure Resource Manager     |
+-----------------------------+        +----------------------------+
               |                                    |
               v                                    v
+-----------------------------+        +----------------------------+
|  Azure Virtual Machines     |        | Azure Storage              |
|  (Compute Services)         | <----> | (Blob, File, Queue, Table) |
+-----------------------------+        +----------------------------+
               |                                    |
               v                                    v
+-----------------------------+        +----------------------------+
|  Azure Virtual Network      | <----> | Azure SQL Database         |
|  (Networking)               |        +----------------------------+
+-----------------------------+                |
               |                                v
               v                    +----------------------------+
+-----------------------------+     |   Azure Synapse Analytics  |
|  Azure App Service          | <-->+  (formerly SQL Data       |
|  (Web Apps, APIs)           |     |   Warehouse)              |
+-----------------------------+     +----------------------------+
               |                                |
               v                                v
+-----------------------------+        +----------------------------+
|  Azure Kubernetes Service   |        |    Azure Data Factory      |
|  (AKS)                      | <----> |   (Data Integration)       |
+-----------------------------+        +----------------------------+
               |                                |
               v                                v
+-----------------------------+        +----------------------------+
|  Azure Functions            |        |    Azure Data Lake         |
|  (Serverless Computing)     | <----> |      Storage (Gen2)        |
+-----------------------------+        +----------------------------+
               |                                |
               v                                v
+-----------------------------+        +----------------------------+
|     Azure Databricks        | <----> |     Azure Purview          |
|  (Analytics & Machine       |        | (Data Governance)          |
|   Learning)                 |        +----------------------------+
+-----------------------------+                |
               |                                v
               v                    +----------------------------+
+-----------------------------+     |      Azure Synapse         |
|       Azure Spark           | <-->+     (Big Data &            |
|  (Big Data Processing)      |     |     Analytics)             |
+-----------------------------+     +----------------------------+

```

## Diagram of Azure Data Factory

```bash
Azure Data Factory
│
├── Main Components
│   ├── Pipelines
│   │   ├── Activities
│   │   │   ├── Copy Data
│   │   │   ├── Data Flow
│   │   │   ├── Lookup
│   │   │   ├── Execute Pipeline
│   │   │   └── Web
│   │   ├── Triggers
│   │       ├── Schedule
│   │       ├── Tumbling Window
│   │       └── Event-Based
│   ├── Datasets
│   │   ├── Azure Blob Storage
│   │   ├── Azure SQL Database
│   │   ├── Azure Data Lake Storage
│   │   └── On-premise SQL Server
│   ├── Linked Services
│   │   ├── Azure Storage Account
│   │   ├── SQL Server
│   │   ├── HTTP
│   │   └── REST
│   └── Integration Runtime
│       ├── Azure Integration Runtime
│       ├── Self-Hosted Integration Runtime
│       └── Azure-SSIS Integration Runtime
│
├── Key Functionalities
│   ├── Data Movement
│   ├── Data Transformation
│   │   ├── Data Flows
│   │   │   ├── Transformation without code
│   │   │   ├── Mapping transformations
│   │   │   └── Debug transformation
│   └── Orchestration and Monitoring
│
├── Integration with other services
│   ├── Azure Synapse Analytics
│   ├── Azure Databricks
│   ├── Azure Machine Learning
│   └── Power BI
│
├── Common use cases
│   ├── ETL/ELT
│   ├── Backup and Recovery
│   ├── Data Migration
│   └── Big Data Integration
│
└── Security and Governance
    ├── Authentication and Authorization
    │   ├── Azure Active Directory (AAD)
    │   └── Role-Based Access Control (RBAC)
    ├── Auditing and Monitoring
    │   ├── Logs de actividad
    │   └── Azure Monitor
    └── Data Protection
        ├── Encryption in Transit and at Rest 
        └── Managed Identity

```


## Diagram of Azure Synapse Analytics

```bash
Azure Synapse Analytics
│
├── Main Components
│   ├── SQL Data Warehouse
│   ├── Spark Pools
│   ├── Synapse Pipelines
│   │   ├── Activities
│   │   │   ├── Copy Data
│   │   │   ├── Data Flow
│   │   │   ├── Lookup
│   │   │   ├── Execute Pipeline
│   │   │   └── Web
│   │   ├── Triggers
│   │       ├── Schedule
│   │       ├── Tumbling Window
│   │       └── Event-Based
│   ├── Data Integration
│   │   ├── Linked Services
│   │   │   ├── Azure Storage Account
│   │   │   ├── SQL Server
│   │   │   ├── HTTP
│   │   │   └── REST
│   │   └── Datasets
│   │       ├── Azure Blob Storage
│   │       ├── Azure SQL Database
│   │       ├── Azure Data Lake Storage
│   │       └── On-premise SQL Server
│   ├── Data Exploration
│   │   ├── SQL On-Demand
│   │   ├── Spark Notebooks
│   │   └── Data Explorer Pools
│   └── Data Management
│       ├── Integration Runtime
│       │   ├── Azure Integration Runtime
│       │   ├── Self-Hosted Integration Runtime
│       │   └── Azure-SSIS Integration Runtime
│       └── Data Governance
│           ├── Azure Purview
│           └── Data Lineage
│
├── Key Functionalities
│   ├── Data Ingestion
│   ├── Data Transformation
│   │   ├── SQL-based transformations
│   │   └── Spark-based transformations
│   ├── Data Integration
│   ├── Data Exploration and Analysis
│   └── Orchestration and Monitoring
│
├── Integration with other services
│   ├── Power BI
│   ├── Azure Machine Learning
│   ├── Azure Data Lake Storage
│   └── Azure Cosmos DB
│
├── Common use cases
│   ├── Big Data Analytics
│   ├── Data Warehousing
│   ├── Real-Time Analytics
│   └── Data Integration
│
└── Security and Governance
    ├── Authentication and Authorization
    │   ├── Azure Active Directory (AAD)
    │   └── Role-Based Access Control (RBAC)
    ├── Auditing and Monitoring
    │   ├── Activity Logs
    │   └── Azure Monitor
    └── Data Protection
        ├── Encryption in Transit and at Rest 
        └── Managed Identity

```

## Azure Databricks

```bash
Azure Databricks
│
├── Key Components
│   ├── Clusters
│   │   ├── Interactive Clusters
│   │   └── Job Clusters
│   ├── Notebooks
│   │   ├── Collaborative Notebooks
│   │   └── Notebook Workflows
│   ├── Jobs
│   │   ├── Job Scheduling
│   │   └── Job Monitoring
│   ├── Delta Lake
│   │   ├── ACID Transactions
│   │   ├── Schema Enforcement
│   │   └── Time Travel
│   ├── Databricks SQL
│   │   ├── SQL Endpoints
│   │   └── Query Editor
│   └── Workflows
│       ├── Data Engineering Workflows
│       └── Machine Learning Workflows
│
├── Core Features
│   ├── Data Processing
│   │   ├── Batch Processing
│   │   └── Stream Processing
│   ├── Machine Learning
│   │   ├── Collaborative ML Workflows
│   │   ├── MLflow Integration
│   │   └── Model Deployment
│   ├── Data Analysis
│   │   ├── Interactive Analysis
│   │   └── BI Tool Integration
│   └── Data Lake Integration
│       ├── Delta Lake
│       └── Data Lakehouse
│
├── Integration with Other Services
│   ├── Azure Data Lake Storage
│   ├── Azure Synapse Analytics
│   ├── Power BI
│   ├── Azure Machine Learning
│   └── Azure DevOps
│
├── Common Use Cases
│   ├── Big Data Analytics
│   ├── ETL Pipelines
│   ├── Real-Time Analytics
│   └── Advanced Analytics and AI
│
└── Security and Governance
    ├── Authentication and Authorization
    │   ├── Azure Active Directory (AAD)
    │   └── Role-Based Access Control (RBAC)
    ├── Auditing and Monitoring
    │   ├── Activity Logs
    │   └── Azure Monitor
    └── Data Protection
        ├── Encryption in Transit and at Rest
        └── Managed Identity

```

## Azure Data Lake

```bash
Azure Data Lake
│
├── Key Components
│   ├── Data Lake Storage
│   │   ├── Gen1
│   │   └── Gen2
│   ├── Data Ingestion
│   │   ├── Batch Ingestion
│   │   └── Stream Ingestion
│   ├── Data Catalog
│   │   ├── Metadata Management
│   │   └── Data Lineage
│   └── Data Security
│       ├── Access Control
│       │   ├── Role-Based Access Control (RBAC)
│       │   └── Access Control Lists (ACLs)
│       └── Data Encryption
│
├── Core Features
│   ├── Scalability
│   │   ├── Unlimited Storage
│   │   └── High Throughput
│   ├── Data Management
│   │   ├── Data Tiering
│   │   └── Lifecycle Management
│   ├── Data Integration
│   │   ├── Azure Data Factory
│   │   ├── Databricks
│   │   └── HDInsight
│   └── Advanced Analytics
│       ├── Machine Learning
│       └── Big Data Analytics
│
├── Integration with Other Services
│   ├── Azure Synapse Analytics
│   ├── Azure Databricks
│   ├── Power BI
│   ├── Azure Machine Learning
│   └── Azure HDInsight
│
├── Common Use Cases
│   ├── Big Data Analytics
│   ├── Data Warehousing
│   ├── Machine Learning
│   └── IoT Analytics
│
└── Security and Governance
    ├── Authentication and Authorization
    │   ├── Azure Active Directory (AAD)
    │   └── Role-Based Access Control (RBAC)
    ├── Auditing and Monitoring
    │   ├── Activity Logs
    │   └── Azure Monitor
    └── Data Protection
        ├── Encryption in Transit and at Rest
        └── Managed Identity

```

## Azure Data Warehouse

```bash
Azure Data Warehouse
│
├── Key Components
│   ├── Data Storage
│   │   ├── Columnar Storage
│   │   └── Data Compression
│   ├── Compute
│   │   ├── Scalable Compute
│   │   └── Query Optimization
│   ├── Data Ingestion
│   │   ├── Batch Loading
│   │   └── Real-Time Ingestion
│   └── Data Security
│       ├── Access Control
│       │   ├── Role-Based Access Control (RBAC)
│       │   └── User-Defined Roles
│       └── Data Encryption
│
├── Core Features
│   ├── Scalability
│   │   ├── On-Demand Scaling
│   │   └── Elastic Compute
│   ├── Performance
│   │   ├── Parallel Processing
│   │   └── Caching
│   ├── Data Management
│   │   ├── Data Partitioning
│   │   └── Indexing
│   └── Advanced Analytics
│       ├── SQL-Based Analytics
│       └── Integration with BI Tools
│
├── Integration with Other Services
│   ├── Azure Data Lake
│   ├── Azure Synapse Analytics
│   ├── Power BI
│   ├── Azure Machine Learning
│   └── Azure Databricks
│
├── Common Use Cases
│   ├── Enterprise Data Warehousing
│   ├── Business Intelligence
│   ├── Advanced Analytics
│   └── Data Integration
│
└── Security and Governance
    ├── Authentication and Authorization
    │   ├── Azure Active Directory (AAD)
    │   └── Role-Based Access Control (RBAC)
    ├── Auditing and Monitoring
    │   ├── Activity Logs
    │   └── Azure Monitor
    └── Data Protection
        ├── Encryption in Transit and at Rest
        └── Managed Identity

```
