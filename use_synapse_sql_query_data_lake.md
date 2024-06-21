# Use Azure Synapse serverless SQL pool to query files in a data lake

## Learning Objetives

After the completion of this module, you will be able to:

 - Identify capabilities and use cases for serverless SQL pools in Azure Synapse Analytics
 - Query CSV, JSON, and Parquet files using a serverless SQL pool
 - Create external database objects in a serverless SQL pool

## Introduction

Azure Synapse Analytics includes serverless SQL pools, which are tailored for querying data in a data lake. With a serverless SQL pool you can use SQL code to query data in files of various common formats without needing to load the file data into database storage. This capability helps data analysts and data engineers analyze and process file data in the data lake using a familiar data processing language, without the need to create or maintain a relational database store.

#
# Prerequisites

Before starting this module, you should have the following prerequisite skills and knowledge:

 - Familiarity with the Microsoft Azure portal
 - Familiarity with data lake and data warehouse concepts
 - Experience of using SQL to query database tables

## Understand Azure Synapse serverless SQL pool capabilities and use cases

**Azure Synapse Analytics is an integrated analytics service** that brings together a wide range of commonly used technologies for processing and analyzing data at scale. One of the most prevalent technologies used in data solutions is SQL - an industry standard language for querying and manipulating data.

### Serverless SQL pools in Azure Synapse Analytics

**Azure Synapse SQL is a distributed query system in Azure Synapse Analytics** that offers two kinds of runtime environments:

 - **Serverless SQL pool**: on-demand SQL query processing, primarily used to work with data in a data lake.
 - **Dedicated SQL pool**: Enterprise-scale relational database instances used to host data warehouses in which data is stored in relational tables.

In this module, we'll focus on serverless **SQL pool, which provides a pay-per-query endpoint to query the data in your data lake**. The **benefits of using serverless SQL pool include**:

 - A familiar Transact-SQL syntax to query data in place without the need to copy or load data into a specialized store.
 - Integrated connectivity from a wide range of business intelligence and ad-hoc querying tools, including the most popular drivers.
 - Distributed query processing that is built for large-scale data, and computational functions - resulting in fast query performance.
 - Built-in query execution fault-tolerance, resulting in high reliability and success rates even for long-running queries involving large data sets.
 - No infrastructure to setup or clusters to maintain. A built-in endpoint for this service is provided within every Azure Synapse workspace, so you can start querying data as soon as the workspace is created.
 - No charge for resources reserved, you're only charged for the data processed by queries you run.

### When to use serverless SQL pools

Serverless SQL pool is tailored for querying the data residing in the data lake, so in addition to eliminating management burden, it eliminates a need to worry about ingesting the data into the system. You just point the query to the data that is already in the lake and run it.

**Synapse SQL serverless resource model** is great for **unplanned or "bursty" workloads** that can be processed using the always-on serverless SQL endpoint in your Azure Synapse Analytics workspace. Using the serverless pool helps when you need to know exact **cost for each query executed** to monitor and attribute costs.

#### Note
     Serverless SQL pool is an analytics system and is NOT recommended for OLTP workloads such as databases used by applications to store transactional data. Workloads that require millisecond response times and are looking to pinpoint a single row in a data set are not good fit for serverless SQL pool.

Common use cases for serverless SQL pools include:

 - **Data exploration**: Data exploration involves browsing the data lake to get initial insights about the data, and is easily achievable with Azure Synapse Studio. You can browse through the files in your linked data lake storage, and use the built-in serverless SQL pool to automatically generate a SQL script to select TOP 100 rows from a file or folder just as you would do with a table in SQL Server. From there, you can apply projections, filtering, grouping, and most of the operation over the data as if the data were in a regular SQL Server table.
 
 - **Data transformation**: While Azure Synapse Analytics provides great data transformations capabilities with Synapse Spark, some data engineers might find data transformation easier to achieve using SQL. Serverless SQL pool enables you to perform SQL-based data transformations; either interactively or as part of an automated data pipeline.
 
 - **Logical data warehouse**: After your initial exploration of the data in the data lake, you can define external objects such as tables and views in a serverless SQL database. The data remains stored in the data lake files, but are abstracted by a relational schema that can be used by client applications and analytical tools to query the data as they would in a relational database hosted in SQL Server.

## Query files using a serverless SQL pool

You can **use a serverless SQL pool to query data files** in various common file formats, including:

 - Delimited text, such as comma-separated values (**CSV**) files.
 - JavaScript object notation (**JSON**) files.
 - **Parquet** files.

The basic syntax for querying is the same for all of these types of file, and is built on the **OPENROWSET SQL function**; which generates a tabular rowset from data in one or more files. For example, **the following query could be used to extract data from CSV files**.

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://mydatalake.blob.core.windows.net/data/files/*.csv',
        FORMAT = 'csv') AS rows
    ```

The **OPENROWSET function** includes more parameters that determine factors such as:

 -The schema of the resulting rowset
 -Additional formatting options for delimited text files.

### tip 

You'll find the full syntax for the OPENROWSET function in the A[zure Synapse Analytics documentation](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-openrowset#syntax).

The **output** from **OPENROWSET** is a **rowset** to which an alias must be assigned. In the previous example, the alias rows is used to name the resulting rowset.

The **BULK** parameter includes the full **URL to the location in the data lake containing the data files**. This can be an individual file, or a folder with a wildcard expression to filter the file types that should be included. The **FORMAT** parameter specifies the **type of data being queried**. The example above reads delimited text from all .csv files in the files folder.

#### Note

This example assumes that the user has access to the files in the underlying store, If the files are protected with a SAS key or custom identity, you would need to [create a server-scoped credential](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-storage-files-storage-access-control?tabs=shared-access-signature#server-scoped-credential).

As seen in the previous example, you can use **wildcards** in the **BULK** parameter to *include or exclude* files in the query. The following list shows a few examples of how this can be used:

 - `https://mydatalake.blob.core.windows.net/data/files/file1.csv`: Only include file1.csv in the files folder.

 - `https://mydatalake.blob.core.windows.net/data/files/file*.csv`: All .csv files in the files folder with names that start with "file".

 - `https://mydatalake.blob.core.windows.net/data/files/*`: All files in the files folder.

 - `https://mydatalake.blob.core.windows.net/data/files/**`: All files in the files folder, and recursively its subfolders.

You can also specify multiple file paths in the **BULK** parameter, separating each path with a comma.

## Querying delimited text files