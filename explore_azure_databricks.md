# Explore Azure Databricks

Azure Databricks is a cloud service that provides a scalable platform for data analytics using Apache Spark.

### Learning objectives

In this module, you'll learn how to:

 - Provision an Azure Databricks workspace.
 - Identify core workloads and personas for Azure Databricks.
 - Describe key concepts of an Azure Databricks solution.

## Introduction

**Azure Databricks is a fully managed, cloud-based data analytics platform**, which empowers developers to accelerate AI and innovation by simplifying the process of building enterprise-grade data applications. Built as a joint effort by Microsoft and the team that started Apache Spark, **Azure Databricks provides data science, engineering, and analytical teams with a single platform for [data lakehouse](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/) analytics and machine learning**.

By combining the power of Databricks, an end-to-end, managed Apache Spark platform optimized for the cloud, with the enterprise scale and security of Microsoft's Azure platform, Azure Databricks enables organizations to run large-scale data analytics workloads that power comprehensive business intelligence and AI solutions.

## Get started with Azure Databricks

Azure Databricks is a cloud-based distributed platform for data processing and analytics in a data lakehouse. Databricks is built on Apache Spark and related open source technologies, and is designed to unify data science, data engineering, and business data analytics in an easy to use environment that enables users to spend more time working effectively with data, and less time focused on managing clusters and infrastructure. As the platform has evolved, it has kept up to date with the latest advances in the Spark runtime and other technologies, and added usability features to support common data workloads in a single, centrally managed interface.

Azure Databricks is hosted on the Microsoft Azure cloud platform, and integrated with Azure services such as Microsoft Entra ID, Azure Storage, Azure Synapse Analytics, and Azure Machine Learning. Organizations can apply their existing capabilities with the Databricks platform, and build fully integrated data analytics solutions that work with cloud infrastructure used by other enterprise applications.

### Creating an Azure Databricks workspace

To use Azure Databricks, you **must create an Azure Databricks workspace** in your Azure subscription. You can accomplish this by:

 - Using the Azure portal user interface.
 - Using an Azure Resource Manager (ARM) or Bicep template.
 - Using the ``New-AzDatabricksWorkspace`` Azure PowerShell cmdlet
 - Using the ``az databricks workspace create`` Azure command line interface (CLI) command.

When you create a workspace, you must specify one of the following pricing tiers:

 - **Standard** - Core Apache Spark capabilities with Microsoft Entra integration.
 - **Premium** - Role-based access controls and other enterprise-level features.
 - **Trial** - A 14-day free trial of a premium-level workspace

<a href="#">
    <img src="./img/create-workspace.png" />
</a>

### Using the Azure Databricks portal

After you've provisioned an Azure Databricks workspace, you can **use the Azure Databricks portal to work with data and compute resources**. The Azure Databricks portal is a web-based user interface through which you can create and manage workspace resources (such as Spark clusters) and use notebooks and queries to work with data in files and tables.

<a href="#">
    <img src="./img/azure-databricks-portal.png" />
</a>

## Identify Azure Databricks workloads

Azure Databricks is a comprehensive platform that offers many data processing capabilities. While you can use the service to support any workload that requires scalable data processing, Azure Databricks particularly supports the following types of data workload:

 - Data Science and Engineering
 - Machine Learning
 - SQL*

**SQL workloads are only available in premium tier workspaces.*

### Data Science and Engineering

**Azure Databricks provides Apache Spark** based ingestion, processing, and analysis of large volumes of data in a data lakehouse. Data engineers, data scientists, and data analysts can ***use interactive notebooks to run code in Python, Scala, SparkSQL, or other languages*** to cleanse, transform, aggregate, and analyze data.

<a href="#">
    <img src="./img/data-engineering.png" />
</a>

### Machine Learning

**Azure Databricks supports machine learning workloads** that involve data exploration and preparation, training and evaluating machine learning models, and serving models to generate predictions for applications and analyses. Data scientists and ML engineers can use AutoML to quickly train predictive models, or apply their skills with common machine learning frameworks such as SparkML, Scikit-Learn, PyTorch, and Tensorflow. They can also manage the end-to-end machine learning lifecycle with **MLFlow**.

<a href="#">
    <img src="./img/machine-learning.png" />
</a>

### Data warehousing

**Azure Databricks supports SQL-based querying** for data stored in tables in a SQL Warehouse. This capability enables data analysts to query, aggregate, summarize, and visualize data using familiar SQL syntax and a wide range of SQL-based data analysis and visualization tools.

<a href="#">
    <img src="./img/sql-portal.png" />
</a>

#### Note

SQL Warehouses are only available in ***premium*** Azure Databricks workspaces.


## Understand key concepts

**Azure Databricks is an amalgamation of multiple technologies** that enable you to work with data at scale. Before using Azure Databricks, there are some key concepts that you should understand.

 1. **Apache Spark clusters** - Spark is a distributed data processing solution that makes use of clusters to scale processing across multiple compute nodes. Each Spark cluster has a driver node to coordinate processing jobs, and one or more worker nodes on which the processing occurs. This distributed model enables each node to operate on a subset of the job in parallel; reducing the overall time for the job to complete. To learn more about clusters in Azure Databricks, see Clusters in the Azure Databricks documentation.

 2. **Data lake storage** - While each cluster node has its own local file system (on which operating system and other node-specific files are stored), the nodes in a cluster also have access to a shared, distributed file system in which they can access and operate on data files. This shared data storage, known as a data lake, enables you to mount cloud storage, such as Azure Data Lake Storage or a Microsoft OneLake data store, and use it to work with and persist file-based data in any format.
 
 3. **Metastore** - Azure Databricks uses a metastore to define a relational schema of tables over file-based data. The tables are based on the Delta Lake format and can be queried using SQL syntax to access the data in the underlying files. The table definitions and details of the file system locations on which they're based are stored in the metastore, abstracting the data objects that you can use for analytics and data processing from the physical storage where the data files are stored. Azure Databricks **metastores are managed in [Unity Catalog](https://learn.microsoft.com/en-us/azure/databricks/connect/unity-catalog/)**, which provides centralized data storage, access management, and governance (though depending on how your Azure Databricks workspace is configured, you may also use a legacy Hive Metastore with data files stored in a Databricks File System (DBFS) data lake).
 
 4. **Notebooks** - One of the most common ways for data analysts, data scientists, data engineers, and developers to work with Spark is to write code in notebooks. Notebooks provide an interactive environment in which you can combine text and graphics in Markdown format with cells containing code that you run interactively in the notebook session. To learn more about notebooks, see Notebooks in the Azure Databricks documentation.
 
 5. **SQL Warehouses** - SQL Warehouses are relational compute resources with endpoints that enable client applications to connect to an Azure Databricks workspace and use SQL to work with data in tables. The results of SQL queries can be used to create data visualizations and dashboards to support business analytics and decision making. SQL Warehouses are only available in premium tier Azure Databricks workspaces. To learn more about SQL Warehouses, see [SQL Warehouses in the Azure Databricks documentation](https://learn.microsoft.com/en-us/azure/databricks/compute/sql-warehouse/create).


## Exercise - Explore Azure Databricks

<a href="https://microsoftlearning.github.io/mslearn-databricks/Instructions/Exercises/01-Explore-Azure-Databricks.html" target="_blank">
    Exercise
</a>

## Knowledge check

1. You want to scan data assets in a dedicated SQL pool in your Azure Synapse Analytics workspace. What kind of source should you register in Microsoft Purview?  

    - [x] Azure Synapse Analytics.
    - [ ] Azure Data Lake Storage Gen2
    - [ ] Azure SQL Database