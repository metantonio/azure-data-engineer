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

## Create Delta Lake tables

Delta lake is built on tables, which provide a relational storage abstraction over files in a data lake.

## Creating a Delta Lake table from a dataframe

One of the easiest ways to **create a Delta Lake table is to save a dataframe in the delta format**, specifying a path where the data files and related metadata information for the table should be stored.

For example, the following PySpark code loads a dataframe with data from an existing file, and then saves that dataframe to a new folder location in delta format:

```py
# Load a file into a dataframe
df = spark.read.load('/data/mydata.csv', format='csv', header=True)

# Save the dataframe as a delta table
delta_table_path = "/delta/mydata"
df.write.format("delta").save(delta_table_path)
```

After saving the delta table, the path location you specified includes **parquet files for the data** (regardless of the format of the source file you loaded into the dataframe) and a **_delta_log** folder containing the transaction log for the table.

You can **replace** *an existing* **Delta Lake** table with the contents of a dataframe by using the **overwrite** mode, as shown here:

```py
new_df.write.format("delta").mode("overwrite").save(delta_table_path)
```

You can also **add rows from a dataframe** *to an existing table* by using the **append** mode:

```py
new_rows_df.write.format("delta").mode("append").save(delta_table_path)
```

### Making conditional updates

While you can make data modifications in a dataframe and then replace a Delta Lake table by overwriting it, a more common pattern in a database is to **insert, update or delete rows** in an existing table *as discrete transactional operations*. To make such modifications to a **Delta Lake table**, you can **use the DeltaTable object** in the Delta Lake API, which supports update, delete, and merge operations. For example, you could use the following code to update the price column for all rows with a category column value of "Accessories":

```py
from delta.tables import *
from pyspark.sql.functions import *

# Create a deltaTable object
deltaTable = DeltaTable.forPath(spark, delta_table_path)

# Update the table (reduce price of accessories by 10%) affect column Price if column Category == 'Accessories'
deltaTable.update(
    condition = "Category == 'Accessories'",
    set = { "Price": "Price * 0.9" })
```

The **data modifications are recorded in the transaction log**, and new parquet files are created in the table folder as required.

 Tip: For more information about using the Delta Lake API, see the [Delta Lake API documentation](https://docs.delta.io/latest/delta-apidoc.html).

### Querying a previous version of a table

Delta Lake tables **support versioning** *through the* **transaction log**. The transaction log records modifications made to the table, noting the timestamp and version number for each transaction. You can use this logged version data to view previous versions of the table - a feature known as time travel.

You can retrieve data from a specific version of a Delta Lake table by reading the data from the delta table location into a dataframe, specifying the version required as a **versionAsOf** option:

```py
df = spark.read.format("delta").option("versionAsOf", 0).load(delta_table_path)
```

Alternatively, you can specify a timestamp by using the timestampAsOf option:

```py
df = spark.read.format("delta").option("timestampAsOf", '2022-01-01').load(delta_table_path)
```

## Create catalog tables

So far we've considered Delta Lake table instances created from dataframes and modified through the Delta Lake API. You **can also define Delta Lake tables as catalog tables** in the **Hive metastore** for your Spark pool, and **work with them using SQL**.

### External vs managed tables

Tables in a Spark catalog, including Delta Lake tables, can be **managed or external**; and it's important to understand the distinction between these kinds of table.

 - **A managed table** is defined **without a specified location**, and the data files are stored within the storage used by the metastore. **Dropping the table** not only **removes its metadata** from the catalog, **but also deletes the folder** in which its data files are stored.
 - **An external table** is defined for a **custom file location**, where the data for the table is stored. The metadata for the table is defined in the Spark catalog. **Dropping the table deletes the metadata** from the catalog, **but doesn't affect the data files**.

### Creating catalog tables

There are several ways to create catalog tables.

#### Creating a catalog table from a dataframe

You can **create managed tables** by writing a dataframe using the saveAsTable operation as shown in the following examples:

```py
# Save a dataframe as a managed table
df.write.format("delta").saveAsTable("MyManagedTable")

## specify a path option to save as an external table
df.write.format("delta").option("path", "/mydata").saveAsTable("MyExternalTable")
```

#### Creating a catalog table using SQL

You can also **create a catalog table** by using the CREATE TABLE SQL statement with the USING DELTA clause, and an optional LOCATION parameter for external tables. You can run the statement using the SparkSQL API, like the following example:

```py
spark.sql("CREATE TABLE MyExternalTable USING DELTA LOCATION '/mydata'")
```

Alternatively you can use the native SQL support in Spark to run the statement:

```sql
%%sql

CREATE TABLE MyExternalTable
USING DELTA
LOCATION '/mydata'
```

 Tip: The **CREATE TABLE** statement returns an error if a table with the specified name already exists in the catalog. To mitigate this behavior, you can use a **CREATE TABLE IF NOT EXISTS** statement or the **CREATE OR REPLACE TABLE** statement.

### Defining the table schema

In all of the examples so far, the table is created without an explicit schema. In the case of tables created by writing a dataframe, the table schema is inherited from the dataframe. **When creating an external table, the schema is inherited from any files that are currently stored in the table location**. However, **when creating a new managed table, or an external table with a currently empty location, you define the table schema** by specifying the column names, types, and nullability as part of the CREATE TABLE statement; as shown in the following example:

```sql
%%sql

CREATE TABLE ManagedSalesOrders
(
    Orderid INT NOT NULL,
    OrderDate TIMESTAMP NOT NULL,
    CustomerName STRING,
    SalesTotal FLOAT NOT NULL
)
USING DELTA
```

When using Delta Lake, table schemas are enforced - all inserts and updates must comply with the specified column nullability and data types.

### Using the DeltaTableBuilder API (PySpark)

You can use the **DeltaTableBuilder API** (part of the Delta Lake API) to create a catalog table, as shown in the following example:

```py
from delta.tables import *

DeltaTable.create(spark) \
  .tableName("default.ManagedProducts") \
  .addColumn("Productid", "INT") \
  .addColumn("ProductName", "STRING") \
  .addColumn("Category", "STRING") \
  .addColumn("Price", "FLOAT") \
  .execute()
```

Similarly to the **CREATE TABLE SQL** statement, the **create** method returns an error if a table with the specified name already exists. You can mitigate this behavior by using the **createIfNotExists** or **createOrReplace** method.

### Using catalog tables

You can use catalog tables like tables in any SQL-based relational database, querying and manipulating them by using standard SQL statements. For example, the following code example uses a SELECT statement to query the **ManagedSalesOrders** table

```sql
%%sql

SELECT orderid, salestotal
FROM ManagedSalesOrders
```

 TIP: For more information about working with Delta Lake, see [Table batch reads and writes](https://docs.delta.io/latest/delta-batch.html) in the Delta Lake documentation.

## Use Delta Lake with streaming data

All of the data we've explored up to this point has been static data in files. However, many data analytics scenarios involve streaming data that must be processed in near real time. For example, you might need to capture readings emitted by internet-of-things (IoT) devices and store them in a table as they occur.

### Spark Structured Streaming

A typical stream processing solution involves constantly reading a stream of data from a source, optionally processing it to select specific fields, aggregate and group values, or otherwise manipulate the data, and writing the results to a sink.

Spark includes native support for **streaming data** through **Spark Structured Streaming**, an API that is based on a boundless dataframe in which streaming data is captured for processing. A Spark Structured Streaming dataframe can read data from many different kinds of streaming source, including network ports, real time message brokering services such as Azure Event Hubs or Kafka, or file system locations.

 Tip: For more information about Spark Structured Streaming, see [Structured Streaming Programming Guide](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) in the Spark documentation.

### Streaming with Delta Lake tables

You can use a Delta Lake table as a source or a sink for Spark Structured Streaming. For example, you could capture a stream of real time data from an IoT device and write the stream directly to a Delta Lake table as a sink - enabling you to query the table to see the latest streamed data. Or, you could read a Delta Table as a streaming source, enabling you to constantly report new data as it is added to the table.

#### Using a Delta Lake table as a streaming source

In the following PySpark example, a Delta Lake table is used to store details of Internet sales orders. A stream is created that reads data from the Delta Lake table folder as new data is appended.

```py
from pyspark.sql.types import *
from pyspark.sql.functions import *

# Load a streaming dataframe from the Delta Table
stream_df = spark.readStream.format("delta") \
    .option("ignoreChanges", "true") \
    .load("/delta/internetorders")

# Now you can process the streaming data in the dataframe
# for example, show it:
stream_df.writeStream \
    .outputMode("append") \
    .format("console") \
    .start()
```

 Note: When using a **Delta Lake table as a streaming source**, **only append operations can be included in the stream**. Data modifications will cause an error unless you specify the **ignoreChanges** or **ignoreDeletes** option.

After reading the data from the Delta Lake table into a streaming dataframe, you can use the Spark Structured Streaming API to process it. In the example above, the dataframe is simply displayed; but you could use Spark Structured Streaming to aggregate the data over temporal windows (for example to count the number of orders placed every minute) and send the aggregated results to a downstream process for near-real-time visualization.

#### Using a Delta Lake table as a streaming sink

In the following PySpark example, a stream of data is read from JSON files in a folder. The JSON data in each file contains the status for an IoT device in the format {"device":"Dev1","status":"ok"} New data is added to the stream whenever a file is added to the folder. The input stream is a boundless dataframe, which is then written in delta format to a folder location for a Delta Lake table.

```py
from pyspark.sql.types import *
from pyspark.sql.functions import *

# Create a stream that reads JSON data from a folder
inputPath = '/streamingdata/'
jsonSchema = StructType([
    StructField("device", StringType(), False),
    StructField("status", StringType(), False)
])
stream_df = spark.readStream.schema(jsonSchema).option("maxFilesPerTrigger", 1).json(inputPath)

# Write the stream to a delta table
table_path = '/delta/devicetable'
checkpoint_path = '/delta/checkpoint'
delta_stream = stream_df.writeStream.format("delta").option("checkpointLocation", checkpoint_path).start(table_path)
```

 Note: The checkpointLocation option is used to write a checkpoint file that tracks the state of the stream processing. This file enables you to recover from failure at the point where stream processing left off.

*After the streaming process has started*, you can query the Delta Lake table to which the streaming output is being written to see the latest data. For example, the following code creates a catalog table for the Delta Lake table folder and queries it:

```sql
%%sql

CREATE TABLE DeviceTable
USING DELTA
LOCATION '/delta/devicetable';

SELECT device, status
FROM DeviceTable;
```

To stop the stream of data being written to the Delta Lake table, you can use the stop method of the streaming query:

```py
delta_stream.stop()
```

 Tip: For more information about using Delta Lake tables for streaming data, see [Table streaming reads and writes](https://docs.delta.io/latest/delta-streaming.html) in the Delta Lake documentation.


## Use Delta Lake in a SQL pool

Delta Lake is designed as a transactional, relational storage layer for Apache Spark; including Spark pools in Azure Synapse Analytics. However, Azure Synapse Analytics also includes a serverless SQL pool runtime that enables data analysts and engineers to run SQL queries against data in a data lake or a relational database.

 Note: You **can only query data from Delta Lake tables in a serverless SQL pool**; you can't update, insert, or delete data.


### Querying delta formatted files with OPENROWSET

The serverless SQL pool in Azure Synapse Analytics includes support for reading delta format files; enabling you to use the SQL pool to query Delta Lake tables. This approach can be useful in scenarios where you want to use Spark and Delta tables to process large quantities of data, but use the SQL pool to run queries for reporting and analysis of the processed data.

In the following example, a SQL **SELECT** query reads delta format data using the **OPENROWSET** function.

```sql
SELECT *
FROM
    OPENROWSET(
        BULK 'https://mystore.dfs.core.windows.net/files/delta/mytable/',
        FORMAT = 'DELTA'
    ) AS deltadata
```

You could run this query in a serverless SQL pool to retrieve the latest data from the Delta Lake table stored in the specified file location.

You could also create a database and add a data source that encapsulates the location of your Delta Lake data files, as shown in this example:

```sql
CREATE DATABASE MyDB
      COLLATE Latin1_General_100_BIN2_UTF8;
GO;

USE MyDB;
GO

CREATE EXTERNAL DATA SOURCE DeltaLakeStore
WITH
(
    LOCATION = 'https://mystore.dfs.core.windows.net/files/delta/'
);
GO

SELECT TOP 10 *
FROM OPENROWSET(
        BULK 'mytable',
        DATA_SOURCE = 'DeltaLakeStore',
        FORMAT = 'DELTA'
    ) as deltadata;
```

 Note: When working with Delta Lake data, which is stored in Parquet format, it's generally best to create a database with a UTF-8 based collation in order to ensure string compatibility.

### Querying catalog tables

The serverless SQL pool in Azure Synapse Analytics has shared access to databases in the Spark metastore, so you can query catalog tables that were created using Spark SQL. In the following example, a SQL query in a serverless SQL pool queries a catalog table that contains Delta Lake data:

```sql
-- By default, Spark catalog tables are created in a database named "default"
-- If you created another database using Spark SQL, you can use it here
USE default;

SELECT * FROM MyDeltaTable;
```

 Tip: For more information about using Delta Tables from a serverless SQL pool, see[ Query Delta Lake files using serverless SQL pool in Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/query-delta-lake-format) in the Azure Synapse Analytics documentation.

## Exercise - Use Delta Lake in Azure Synapse Analytics