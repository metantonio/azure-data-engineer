# Ingest streaming data using Azure Stream Analytics and Azure Synapse Analytics

Azure Stream Analytics provides a real-time data processing engine that you can use to ingest streaming event data into Azure Synapse Analytics for further analysis and reporting.

## Learning objectives

After completing this module, you'll be able to:

 - Describe common stream ingestion scenarios for Azure Synapse Analytics.
 - Configure inputs and outputs for an Azure Stream Analytics job.
 - Define a query to ingest real-time data into Azure Synapse Analytics.
 - Run a job to ingest real-time data, and consume that data in Azure Synapse Analytics.

## Introduction

Suppose a retail company **captures real-time** sales transaction **data** from an e-commerce website, and wants to analyze this data along with more static data related to products, customers, and employees. A common way to approach this problem is to ***ingest the stream of real-time data into a data lake or data warehouse***, where it can be queried together with data that is loaded using batch processing techniques.

Microsoft Azure Synapse Analytics provides a comprehensive enterprise data analytics platform, into which real-time data captured in Azure Event Hubs or Azure IoT Hub, and processed by Azure Stream Analytics can be loaded.

<a href="#">
    <img src="./img/stream-ingestion.png" />
</a>

A typical pattern for real-time data ingestion in Azure consists of the following sequence of service integrations:

 1. A real-time source of data is captured in an ***event ingestor***, such as **Azure Event Hubs or Azure IoT Hub**.
 2. The captured data is *perpetually filtered and aggregated* by an **Azure Stream Analytics query**.
 3. The results of the **query are loaded into a data lake or data warehouse** in Azure Synapse Analytics for subsequent analysis.

In this module, you'll explore multiple ways in which you can use Azure Stream Analytics to ingest real-time data into Azure Synapse Analytics.

## Stream ingestion scenarios

Azure Synapse Analytics provides multiple ways to analyze large volumes of data. ***Two*** of the most common ***approaches*** to large-scale data analytics are:

 - **Data warehouses** - relational databases, optimized for distributed storage and query processing. Data is stored in tables and queried using SQL.
 - **Data lakes** - distributed file storage in which data is stored as files that can be processed and queried using multiple runtimes, including Apache Spark and SQL.

### Data warehouses in Azure Synapse Analytics
Azure Synapse Analytics provides dedicated SQL pools that you can use to implement enterprise-scale relational data warehouses. Dedicated SQL pools are based on a *massively parallel processing* (**MPP**) instance of the Microsoft SQL Server relational database engine in which data is stored and queried in tables.

To ingest real-time data into a relational data warehouse, your Azure Stream Analytics query must write its results to an output that references the table into which you want to load the data.

A diagram of a stream of data being ingested into a dedicated SQL pool in Azure Synapse Analytics.

### Data lakes in Azure Synapse Analytics

An Azure Synapse Analytics workspace typically includes at least one storage service that is used as a data lake. Most commonly, the data lake is hosted in an Azure Storage account using a container configured to support Azure Data Lake Storage Gen2. Files in the data lake are organized hierarchically in directories (folders), and can be stored in multiple file formats, including delimited text (such as comma-separated values, or CSV), Parquet, and JSON.

When ingesting real-time data into a data lake, your Azure Stream Analytics query must write its results to an output that references the location in the Azure Data Lake Gen2 storage container where you want to save the data files. Data analysts, engineers, and scientists can then process and query the files in the data lake by running code in an Apache Spark pool, or by running SQL queries using a serverless SQL pool.

## Configure inputs and outputs

**All Azure Stream Analytics jobs include at least one input and output**. In most cases, ***inputs reference sources of streaming data*** (though you can also define inputs for static reference data to augment the streamed event data). ***Outputs determine where the results of the stream processing query will be sent***. In the case of data ingestion into Azure Synapse Analytics, the output usually references an Azure Data Lake Storage Gen2 container or a table in a dedicated SQL pool database.