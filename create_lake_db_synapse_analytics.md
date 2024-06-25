# Create a lake database in Azure Synapse Analytics

Why choose between working with files in a data lake or a relational database schema? With lake databases in Azure Synapse Analytics, you can combine the benefits of both.

## Learning Objectives

After completing this module, you will be able to:

 - Understand lake database concepts and components
 - Describe database templates in Azure Synapse Analytics
 - Create a lake database

## Introduction

Data analysts and engineers often find themselves forced to **choose between the flexibility of storing data files** in a data lake, with the advantages of a structured schema in a relational database. Lake databases in **Azure Synapse Analytics provide a way to combine these two approaches** and benefit from an explicit relational schema of tables, views, and relationships that is decoupled from file-based storage.

In this module, you'll learn how to:

 - Understand lake database concepts and components
 - Describe database templates in Azure Synapse Analytics
 - Create a lake database

## Understand lake database concepts

In a traditional relational database, the database schema is composed of tables, views, and other objects. Tables in a relational database define the entities for which data is stored - for example, a retail database might include tables for products, customers, and orders. Each entity consists of a set of attributes that are defined as columns in the table, and each column has a name and a data type. The data for the tables is stored in the database, and is tightly coupled to the table definition; which enforces data types, nullability, key uniqueness, and referential integrity between related keys. All queries and data manipulations must be performed through the database system.

**In a data lake, there is no fixed schema**. **Data is stored in files**, which may be structured, semi-structured, or unstructured. Applications and data analysts can work directly with the files in the data lake using the tools of their choice; without the constraints of a relational database system.

A **lake database provides a relational metadata layer** over one or more files in a data lake. You can create a lake database that includes definitions for tables, including column names and data types as well as relationships between primary and foreign key columns. The **tables reference files in the data lake**, enabling you to apply relational semantics to working with the data and querying it using SQL. However, the **storage of the data files is decoupled from the database schema**; enabling more flexibility than a relational database system typically offers.

### Lake database schema

You can create a lake database in Azure Synapse Analytics, and define the tables that represent the entities for which you need to store data. You can apply proven data modeling principles to create relationships between tables and use appropriate naming conventions for tables, columns, and other database objects.

Azure Synapse Analytics includes a graphical database design interface that you can use to model complex database schema, using many of the same best practices for database design that you would apply to a traditional database.

### Lake database storage

The data for the tables in your lake database is stored in the data lake as Parquet or CSV files. The files can be managed independently of the database tables, making it easier to manage data ingestion and manipulation with a wide variety of data processing tools and technologies.

### Lake database compute

To query and manipulate the data through the tables you have defined, you can use an Azure Synapse serverless SQL pool to run SQL queries or an Azure Synapse Apache Spark pool to work with the tables using the Spark SQL API.

## Explore database templates

You can** create a Lake database from an empty schema**, to which you add definitions for tables and the relationships between them. However, **Azure Synapse Analytics provides** a comprehensive **collection of database templates** that reflect common schemas found in multiple business scenarios; including:

 - Agriculture
 - Automotive
 - Banking
 - Consumer goods
 - Energy and commodity trading
 - Freight and logistics
 - Fund management
 - Healthcare insurance
 - Healthcare provider
 - Manufacturing
 - Retail
 - and many others...

<a href="#">
    <img src="./img/gallery.png" />
</a>

You can use one of the enterprise database templates as the starting point for creating your lake database, or you can start with a blank schema and add and modify tables from the templates as required.

## Create a lake database

You can create a lake database using the lake database designer in Azure Synapse Studio. Start by adding a new lake database on the **Data** page, selecting a template from the gallery or starting with a blank lake database; and then add and customize tables using the visual database designer interface.

As you create each table, you can specify the type and location of the files you want to use to store the underlying data, or you can create a table from existing files that are already in the data lake. In most cases, it's advisable to store all of the database files in a consistent format within the same root folder in the data lake.

### Database designer

The database designer interface in Azure Synapse Studio provides a drag-and-drop surface on which you can edit the tables in your database and the relationships between them.

<a href="#">
    <img src="./img/database-designer-small.png" />
</a>

Using the database designer, you can define the schema for your database by adding or removing tables and:

 - **Specifying** the name and storage settings for each **table**.
 - **Specifying** the names, key usage, nullability, and data types **for each column**.
 - **Defining relationships** between key columns in tables.
 
When your database schema is ready for use, you can publish the database and start using it.

## Use a lake database

After creating a lake database, you can store data files that match the table schemas in the appropriate folders in the data lake, and query them using SQL.

### Using a serverless SQL pool

You can **query a lake database** **in a SQL script** by using a **serverless SQL pool**.

For example, suppose a lake *database* named **RetailDB** contains an **Customer** table. You could query it using a standard **SELECT statement** like this:

```sql
USE RetailDB;
GO

SELECT CustomerID, FirstName, LastName
FROM Customer
ORDER BY LastName;
```

There is **no need to use an OPENROWSET function** or include any additional code **to access the data from** the underlying **file storage**. The serverless SQL pool handles the mapping to the files for you.

### Using an Apache Spark pool

In addition to using a serverless SQL pool, you can **work with lake database tables using Spark SQL** in an **Apache Spark pool**.

For example, you could use the following code to **insert a new customer** record into the **Customer** table.

```sql
%%sql
INSERT INTO `RetailDB`.`Customer` VALUES (123, 'John', 'Yang')
```

You could then use the following code to query the table:

```sql
%%sql
SELECT * FROM `RetailDB`.`Customer` WHERE CustomerID = 123
```

