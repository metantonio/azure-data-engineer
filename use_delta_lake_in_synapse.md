# Use Delta Lake in Azure Synapse Analytics

Delta Lake is an open source relational storage area for Spark that you can use to implement a data lakehouse architecture in Azure Synapse Analytics.

## Learning objectives

In this module, you'll learn how to:

 - Describe core features and capabilities of Delta Lake.
 - Create and use Delta Lake tables in a Synapse Analytics Spark pool.
 - Create Spark catalog tables for Delta Lake data.
 - Use Delta Lake tables for streaming data.
 - Query Delta Lake tables from a Synapse Analytics SQL pool.

## Introduction

**Linux foundation Delta Lake** is an **open-source storage layer** for Spark that **enables relational database capabilities for batch and streaming data**. By using Delta Lake, you can implement a data lakehouse architecture in Spark to support SQL_based data manipulation semantics with support for transactions and schema enforcement. The result is an analytical data store that offers many of the advantages of a relational database system with the flexibility of data file storage in a data lake.

In this module, you'll learn how to:

 - Describe core features and capabilities of Delta Lake.
 - Create and use Delta Lake tables in a Synapse Analytics Spark pool.
 - Create Spark catalog tables for Delta Lake data.
 - Use Delta Lake tables for streaming data.
 - Query Delta Lake tables from a Synapse Analytics SQL pool.

 Note: The version of Delta Lake available in an Azure Synapse Analytics pool depends on the version of Spark specified in the pool configuration. The information in this module reflects Delta Lake version 1.0, which is installed with Spark 3.1.

## Understand Delta Lake

**Delta Lake** is an open-source storage layer that **adds relational database semantics to Spark-based data lake processing**. Delta Lake is supported in Azure Synapse Analytics Spark pools for PySpark, Scala, and .NET code.

The **benefits** of using Delta Lake in a Synapse Analytics Spark pool include:

 - **Relational tables that support querying and data modification**. With Delta Lake, you can store data in tables that support CRUD (create, read, update, and delete) operations. In other words, you can select, insert, update, and delete rows of data in the same way you would in a relational database system.

 - **Support for ACID transactions**. Relational databases are designed to support transactional data modifications that provide atomicity (transactions complete as a single unit of work), consistency (transactions leave the database in a consistent state), isolation (in-process transactions can't interfere with one another), and durability (when a transaction completes, the changes it made are persisted). Delta Lake brings this same transactional support to Spark by implementing a transaction log and enforcing serializable isolation for concurrent operations.

 - **Data versioning and time travel**. Because all transactions are logged in the transaction log, you can track multiple versions of each table row and even use the time travel feature to retrieve a previous version of a row in a query.

 - **Support for batch and streaming data**. While most relational databases include tables that store static data, Spark includes native support for streaming data through the Spark Structured Streaming API. Delta Lake tables can be used as both sinks (destinations) and sources for streaming data.

 - **Standard formats and interoperability**. The underlying data for Delta Lake tables is stored in **Parquet format**, which is commonly used in data lake ingestion pipelines. Additionally, you can use the serverless SQL pool in Azure Synapse Analytics to query Delta Lake tables in SQL.

 TIP: For more information about Delta Lake in Azure Synapse Analytics, see [What is Delta Lake](https://learn.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-what-is-delta-lake) in the Azure Synapse Analytics documentation.