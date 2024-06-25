# Use Azure Synapse serverless SQL pools to transform data in a data lake

## Learning Objetives

By using a serverless SQL pool in Azure Synapse Analytics, you can use the ubiquitous SQL language to transform data in files in a data lake.

After completing this module, you'll be able to:

 - Use a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement to transform data.
 - Encapsulate a CETAS statement in a stored procedure.
 - Include a data transformation stored procedure in a pipeline.

## Introduction

While SQL is commonly used by data analysts to query data and support analytical and reporting workloads, data engineers often **need to use SQL to transform data**; often as part of a data ingestion pipeline or extract, transform, and load **(ETL) process**.

In this module, you'll learn **how to use CREATE EXTERNAL TABLE AS SELECT (CETAS) statements** to transform data, and **store the results in files in a data lake** that can be queried through a relational table in a serverless SQL database or processed directly from the file system.

## Transform data files with the CREATE EXTERNAL TABLE AS SELECT statement

The SQL language includes many features and functions that enable you to manipulate data. For example, you can use SQL to:

 - **Filter** rows and columns in a dataset.
 - **Rename** **data** fields **and convert** between data types.
 - **Calculate** derived data fields.
 - **Manipulate** string values.
 - **Group and aggregate data**.

**Azure Synapse serverless SQL pools** can be **used to run SQL statements** that **transform** **data** and persist the results **as a file** **in a data lake** for further processing or querying. If you're familiar with Transact-SQL syntax, you can craft a SELECT statement that applies the specific transformation you're interested in, and store the results of the SELECT statement in a selected file format with a metadata table schema that can be queried using SQL.

You can **use a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement** in a dedicated SQL pool or serverless SQL pool **to persist the results of a query in an external table**, which stores its data in a file **in the data lake**.

The CETAS statement includes a SELECT statement that queries and manipulates data from any valid data source (which could be an existing table or view in a database, or an OPENROWSET function that reads file-based data from the data lake). The results of the SELECT statement are then persisted in an external table, which is a metadata object in a database that provides a relational abstraction over data stored in files. The following diagram illustrates the concept visually:

<a href="#">
    <img src="./img/create-external-table-as-select.png" />
</a>

By applying this technique, you can use SQL to extract and transform data from files or tables, and store the transformed results for downstream processing or analysis. Subsequent operations on the transformed data can be performed against the relational table in the SQL pool database or directly against the underlying data file

### Creating external database objects to support CETAS

To use **CETAS expressions**, you must create the following types of object in a database for either a serverless or dedicated SQL pool. When using a serverless SQL pool, create these objects in a custom database (created using the CREATE DATABASE statement), not the **built-in** database.

#### External data source

**An external data source encapsulates a connection to a file system location in a data lake**. You can then use this connection to specify a relative path in which the data files for the external table created by the CETAS statement are saved.

If the source data for the CETAS statement is in files in the same data lake path, you can use the same external data source in the **OPENROWSET** function used to query it. Alternatively, you can create a separate external data source for the source files or use a fully qualified file path in the OPENROWSET function.

**To create an external data source**, use the **CREATE EXTERNAL DATA SOURCE statement**, as shown in this example:

```sql
-- Create an external data source for the Azure storage account
CREATE EXTERNAL DATA SOURCE files
WITH (
    LOCATION = 'https://mydatalake.blob.core.windows.net/data/files/',
    TYPE = HADOOP, -- For dedicated SQL pool
    -- TYPE = BLOB_STORAGE, -- For serverless SQL pool
    CREDENTIAL = storageCred
);
```