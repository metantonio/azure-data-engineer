# Analyze data with Apache Spark in Azure Synapse Analytics

Apache Spark is a core technology for large-scale data analytics. Learn how to use Spark in Azure Synapse Analytics to analyze and visualize data in a data lake.

## Learning Objectives

After completing this module, you will be able to:

 - Identify core features and capabilities of Apache Spark.
 - Configure a Spark pool in Azure Synapse Analytics.
 - Run code to load, analyze, and visualize data in a Spark notebook.

## Introduction

Apache Spark is an **open source parallel processing framework for large-scale data processing and analytics**. Spark has become extremely popular in "big data" processing scenarios, and **is available in multiple platform** implementations; including **Azure HDInsight, Azure Databricks, and Azure Synapse Analytics**.

This module explores **how you can use Spark** in Azure Synapse Analytics **to ingest, process, and analyze data from a data lake**. While the core techniques and code described in this module are common to all Spark implementations, the integrated tools and ability to work with Spark in the same environment as other Synapse analytical runtimes are specific to Azure Synapse Analytics.

## Get to know Apache Spark

Apache Spark is distributed data processing framework that enables large-scale data analytics by coordinating work across multiple processing nodes in a cluster.

### How Spark works

Apache Spark **applications run as independent sets of processes on a cluster**, **coordinated** by the SparkContext object in your main program (called the driver program). The SparkContext connects to the cluster manager, which allocates resources across applications using an implementation of Apache Hadoop YARN. Once connected, Spark acquires executors on nodes in the cluster to run your application code.

The **SparkContext runs the main function and parallel operations on the cluster nodes**, and **then collects the results of the operations**. The nodes read and write data from and to the file system and cache transformed data in-memory as Resilient Distributed Datasets (RDDs).

<a href="#">
    <img src="./img/synapse-spark-architecture.png" />
</a>

The SparkContext is responsible for converting an application to a **directed acyclic graph (DAG)**. The graph consists of individual tasks that get executed within an executor process on the nodes. Each application gets its own executor processes, which stay up for the duration of the whole application and run tasks in multiple threads.

### Spark pools in Azure Synapse Analytics

In Azure Synapse Analytics, **a cluster is implemented as a Spark poo**l, which provides a runtime for Spark operations. You can create one or more Spark pools in an Azure Synapse Analytics workspace by using the Azure portal, or in Azure Synapse Studio. When defining a Spark pool, you can specify configuration options for the pool, including:

 - A name for the spark pool.
 - The size of virtual machine (VM) used for the nodes in the pool, including the option to use hardware [accelerated GPU-enabled nodes](https://learn.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-overview).
 - The number of nodes in the pool, and whether the pool size is fixed or individual nodes can be brought online dynamically to auto-scale the cluster; in which case, you can specify the minimum and maximum number of active nodes.
 - The version of the Spark Runtime to be used in the pool; which dictates the versions of individual components such as Python, Java, and others that get installed.

#### Tip [Apache Spark pool configurations](https://learn.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-pool-configurations)

    For more information about Spark pool configuration options, see Apache Spark pool configurations in Azure Synapse Analytics in the Azure Synapse Analytics documentation.

## Use Spark in Azure Synapse Analytics

You can run many different kinds of application on Spark, including **code in Python or Scala scripts**, **Java code compiled as a Java Archive (JAR)**, and others. Spark is commonly used in two kinds of workload:

 - Batch or stream processing jobs to ingest, clean, and transform data - often running as part of an automated pipeline.
 - Interactive analytics sessions to explore, analyze, and visualize data.


### Running Spark code in notebooks

Azure Synapse Studio includes an integrated notebook interface for working with Spark. Notebooks provide an intuitive way to combine code with Markdown notes, commonly used by data scientists and data analysts. The look and feel of the integrated notebook experience within Azure Synapse Studio is similar to that of Jupyter notebooks - a popular open source notebook platform.

<a href="#">
    <img src="./img/spark-notebook.png" />
</a>

 Note: While usually used interactively, notebooks can be included in automated pipelines and run as an unattended script.

Notebooks consist of one or more *cells*, each containing either code or markdown. Code cells in notebooks have some features that can help you be more productive, including:

 - Syntax highlighting and error support.
 - Code auto-completion.
 - Interactive data visualizations.
 - The ability to export results.

#### Tip [article link](https://learn.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-development-using-notebooks)

 To learn more about working with notebooks in Azure Synapse Analytics, see the Create, develop, and maintain Synapse notebooks in Azure Synapse Analytics article in the Azure Synapse Analytics documentation.

### Accessing data from a Synapse Spark pool

You can use Spark in Azure Synapse Analytics to work with data from various sources, including:

 - A data lake based on the primary storage account for the Azure Synapse Analytics workspace.
 - A data lake based on storage defined as a linked service in the workspace.
 - A dedicated or serverless SQL pool in the workspace.
 - An Azure SQL or SQL Server database (using the Spark connector for SQL Server)
 - An Azure Cosmos DB analytical database defined as a linked service and configured using Azure Synapse Link for Cosmos DB.
 - An Azure Data Explorer Kusto database defined as a linked service in the workspace.
 - An external Hive metastore defined as a linked service in the workspace.

One of the most **common uses of Spark is to work with data in a data lake**, where you can read and write files in multiple commonly used formats, including delimited text, Parquet, Avro, and others.

## Analyze data with Spark

One of the **benefits of using Spark is that you can write and run code in various programming languages**, enabling you to use the programming skills you already have and to use the most appropriate language for a given task. The default language in a new Azure Synapse Analytics **Spark notebook is PySpark** - a Spark-optimized version of **Python**, which is commonly used by data scientists and analysts due to its strong support for data manipulation and visualization. Additionally, **you can use languages such as Scala** (a Java-derived language that can be used interactively) **and SQL** (a variant of the commonly used SQL language included in the Spark SQL library to work with relational data structures). Software engineers can also create compiled solutions that run on Spark using frameworks such as Java and Microsoft .NET.

### Exploring data with dataframes

Natively, **Spark uses a data structure called** a **resilient distributed dataset (RDD)**; but while you can write code that works directly with RDDs, the most commonly used data structure for working with structured data in Spark is the dataframe, which is provided as part of the Spark SQL library. Dataframes in Spark are similar to those in the ubiquitous Pandas Python library, but optimized to work in Spark's distributed processing environment.

 Note: In addition to the Dataframe API, Spark SQL provides a strongly-typed Dataset API that is supported in Java and Scala. We'll focus on the Dataframe API in this module.

### Loading data into a dataframe

Let's explore a hypothetical example to see how you can use a dataframe to work with data. Suppose you have the following data in a comma-delimited text file named **products.csv** in the primary storage account for an Azure Synapse Analytics workspace:

```csv
ProductID,ProductName,Category,ListPrice
771,"Mountain-100 Silver, 38",Mountain Bikes,3399.9900
772,"Mountain-100 Silver, 42",Mountain Bikes,3399.9900
773,"Mountain-100 Silver, 44",Mountain Bikes,3399.9900
...
```

In a Spark notebook, you could use the following PySpark code to load the data into a dataframe and display the first 10 rows:

```py
%%pyspark
df = spark.read.load('abfss://container@store.dfs.core.windows.net/products.csv',
    format='csv',
    header=True
)
display(df.limit(10))
```

The ***%%pyspark*** line at the beginning **is called a magic**, and **tells Spark that the language used in this cell is PySpark**. You can select the language you want to use as a default in the toolbar of the Notebook interface, and then use a magic to override that choice for a specific cell. For example, here's the equivalent Scala code for the products data example:

```scala
%%spark
val df = spark.read.format("csv").option("header", "true").load("abfss://container@store.dfs.core.windows.net/products.csv")
display(df.limit(10))
```

The magic **%%spark** is used to specify Scala.

Both of these code samples would produce output like this:




