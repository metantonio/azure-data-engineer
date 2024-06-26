# Load data into a relational data warehouse

A core responsibility for a data engineer is to implement a data ingestion solution that loads new data into a relational data warehouse.

## Learning objectives

In this module, you'll learn how to:

 - Load staging tables in a data warehouse
 - Load dimension tables in a data warehouse
 - Load time dimensions in a data warehouse
 - Load slowly changing dimensions in a data warehouse
 - Load fact tables in a data warehouse
 - Perform post-load optimizations in a data warehouse

## Introduction

Many enterprise analytical solutions include a relational data warehouse. Data engineers are responsible for implementing ingestion solutions that load data into the data warehouse tables, usually on a regular schedule.

As a data engineer, you need to be familiar with the considerations and techniques that apply to loading a data warehouse. In this module, we'll focus on ways that you can use SQL to load data into tables in a dedicated SQL pool in Azure Synapse Analytics.

## Load staging tables

One of the most common patterns for loading a data warehouse is to transfer data from source systems to files in a data lake, ingest the file data into staging tables, and then use SQL statements to load the data from the staging tables into the dimension and fact tables. Usually data loading is performed as a periodic batch process in which inserts and updates to the data warehouse are coordinated to occur at a regular interval (for example, daily, weekly, or monthly).

### Creating staging tables

Many organized warehouses have standard structures for staging the database and might even use a specific schema for staging the data. The following code example creates a staging table for product data that will ultimately be loaded into a dimension table:

 Note: This example creates a staging table in the default dbo schema. You can also create separate schemas for staging tables with a meaningful name, such as stage so architects and users understand the purpose of the schema.

```sql
CREATE TABLE dbo.StageProduct
(
    ProductID NVARCHAR(10) NOT NULL,
    ProductName NVARCHAR(200) NOT NULL,
    ProductCategory NVARCHAR(200) NOT NULL,
    Color NVARCHAR(10),
    Size NVARCHAR(10),
    ListPrice DECIMAL NOT NULL,
    Discontinued BIT NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    CLUSTERED COLUMNSTORE INDEX
);
```

### Using the COPY command

You can use the **COPY statement to load data from the data lake**, as shown in the following example:

 Note: This is generally the recommended approach to load staging tables due to its high performance throughput.

```sql
COPY INTO dbo.StageProduct
    (ProductID, ProductName, ...)
FROM 'https://mydatalake.../data/products*.parquet'
WITH
(
    FILE_TYPE = 'PARQUET',
    MAXERRORS = 0,
    IDENTITY_INSERT = 'OFF'
);
```

 Tip: To learn more about the COPY statement, see [COPY (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) in the Transact-SQL documentation.

### Using external tables

In some cases, if the data to be loaded is stored in files with an appropriate structure, **it can be more effective to create external tables that reference the file location**. This way, the data can be read directly from the source files instead of being loaded into the relational store. The following example, shows how to create an external table that references files in the data lake associated with the Azure Synapse Analytics workspace:

```sql
CREATE EXTERNAL TABLE dbo.ExternalStageProduct
 (
     ProductID NVARCHAR(10) NOT NULL,
     ProductName NVARCHAR(10) NOT NULL,
 ...
 )
WITH
 (
    DATE_SOURCE = StagedFiles,
    LOCATION = 'folder_name/*.parquet',
    FILE_FORMAT = ParquetFormat
 );
GO
```

 Tip: For more information about using external tables, see [Use external tables with Synapse SQL](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-tables-external-tables?tabs=hadoop) in the Azure Synapse Analytics documentation.

## Load dimension tables

After staging dimension data, you can load it into dimension tables using SQL.

### Using a CREATE TABLE AS (CTAS) statement

One of the simplest ways to load data into a new dimension table is to use a CREATE TABLE AS (CTAS) expression. This statement creates a new table based on the results of a SELECT statement.

```sql
CREATE TABLE dbo.DimProduct
WITH
(
    DISTRIBUTION = REPLICATE,
    CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT ROW_NUMBER() OVER(ORDER BY ProdID) AS ProdKey,
    ProdID as ProdAltKey,
    ProductName,
    ProductCategory,
    Color,
    Size,
    ListPrice,
    Discontinued
FROM dbo.StageProduct;
```

 Note: You can't use **IDENTITY** to generate a **unique integer value** for the surrogate key when using a CTAS statement, so this example uses the ROW_NUMBER function to generate an incrementing row number for each row in the results ordered by the **ProductID** business key in the staged data.

You can also load a combination of new and updated data into a dimension table by using a **CREATE TABLE AS (CTAS)** statement to create a new table that UNIONs the existing rows from the dimension table with the new and updated records from the staging table. After creating the new table, you can delete or rename the current dimension table, and rename the new table to replace it.

```sql
CREATE TABLE dbo.DimProductUpsert
WITH
(
    DISTRIBUTION = REPLICATE,
    CLUSTERED COLUMNSTORE INDEX
)
AS
-- New or updated rows
SELECT  stg.ProductID AS ProductBusinessKey,
        stg.ProductName,
        stg.ProductCategory,
        stg.Color,
        stg.Size,
        stg.ListPrice,
        stg.Discontinued
FROM    dbo.StageProduct AS stg
UNION ALL  
-- Existing rows
SELECT  dim.ProductBusinessKey,
        dim.ProductName,
        dim.ProductCategory,
        dim.Color,
        dim.Size,
        dim.ListPrice,
        dim.Discontinued
FROM    dbo.DimProduct AS dim
WHERE NOT EXISTS
(   SELECT  *
    FROM dbo.StageProduct AS stg
    WHERE stg.ProductId = dim.ProductBusinessKey
);

RENAME OBJECT dbo.DimProduct TO DimProductArchive;
RENAME OBJECT dbo.DimProductUpsert TO DimProduct;
```

While this technique is effective in merging new and existing dimension data, lack of support for IDENTITY columns means that it's difficult to generate a surrogate key.

 Tip: [For more information, see CREATE TABLE AS SELECT (CTAS) in the Azure Synapse Analytics documentation.](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas)

### Using an INSERT statement

When **you need to load staged data into an existing dimension table, you can use an INSERT statement**. This approach works if the staged data contains only records for new dimension entities (not updates to existing entities). This approach is much less complicated than the technique in the last section, which required a UNION ALL and then renaming table objects.

```sql
INSERT INTO dbo.DimCustomer
SELECT CustomerNo AS CustAltKey,
    CustomerName,
    EmailAddress,
    Phone,
    StreetAddress,
    City,
    PostalCode,
    CountryRegion
FROM dbo.StageCustomers
```

 Note: Assuming the **DimCustomer** dimension table is defined with an **IDENTITY** **CustomerKey** column for the surrogate key (as described in the previous unit), the key will be generated automatically and the remaining columns will be populated using the values retrieved from the staging table by the **SELECT** query.

## Load time dimension tables

Time dimension tables store a record for each time interval based on the grain by which you want to aggregate data over time. For example, a time dimension table at the date grain contains a record for each date between the earliest and latest dates referenced by the data in related fact tables.

The following code example shows how you can generate a sequence of time dimension values based on a date grain.

```sql
-- Create a temporary table for the dates we need
CREATE TABLE #TmpStageDate (DateVal DATE NOT NULL)

-- Populate the temp table with a range of dates
DECLARE @StartDate DATE
DECLARE @EndDate DATE
SET @StartDate = '2019-01-01'
SET @EndDate = '2023-12-31'
DECLARE @LoopDate = @StartDate
WHILE @LoopDate <= @EndDate
BEGIN
    INSERT INTO #TmpStageDate VALUES
    (
        @LoopDate
    )
    SET @LoopDate = DATEADD(dd, 1, @LoopDate)
END

-- Insert the dates and calculated attributes into the dimension table
INSERT INTO dbo.DimDate
SELECT CAST(CONVERT(VARCHAR(8), DateVal, 112) as INT), -- date key
    DateVal, --date alt key
    Day(DateVal) -- day number of month
    --,  other derived temporal fields as required
FROM #TmpStageDate
GO

--Drop temporary table
DROP TABLE #TmpStageDate
```

 Tip: Scripting this in SQL may be time-consuming in a dedicated SQL pool – it may be more efficient to prepare the data in Microsoft Excel or an external script and import it using the COPY statement.

As the data warehouse is populated in the future with new fact data, you periodically need to extend the range of dates related time dimension tables.

## Load slowly changing dimensions

In most relational data warehouses, you need to handle updates to dimension data and support what are commonly referred to as **slowly changing dimensions (SCDs)**.

### Types of slowly changing dimension

There are multiple kinds of slowly changing dimension, of which **three are commonly implemented**:

#### Type 0

**Type 0 dimension data can't be changed**. Any attempted changes fail.

DateKey	| DateAltKey	| Day| Month	| Year
---	| :---:	| :---: | :---:	| :---:
20230101	| 01-01-2023	| Sunday	| January	| 2023

#### Type 1

In **type 1 dimensions, the dimension record is updated in-place**. Changes made to an existing dimension row apply to all previously loaded facts related to the dimension.

StoreKey	| StoreAltKey	| StoreName
---	| :---:	| :--- 
123 | 	EH199J| 	High Street Store Town Central Store

#### Type 2

In a **type 2 dimension, a change to a dimension results in a new dimension row**. Existing rows for previous versions of the dimension are retained for historical fact analysis and the new row is applied to future fact table entries.

CustomerKey	| CustomerAltKey	| Name	| Address	| City	| DateFrom	| DateTo	| IsCurrent
---	| ---	| ---	| ---	| ---	| ---	| ---	| ---
1211	| jo@contoso.com	| Jo Smith	| 999 Main St	| Seattle	| 20190101	| 20230105	| False
2996	| jo@contoso.com	| Jo Smith	| 1234 9th Ave	| Boston	| 20230106	|	| True

 Note: Type 2 dimensions often include columns to track the effective time periods for each version of an entity, and/or a flag to indicate which row represents the current version of the entity. If you’re using an incrementing surrogate key and you only need to track the most recently added version of an entity, then you may not need these columns; but before making that decision, consider how you’ll look up the appropriate version of an entity when a new fact is entered based on the time at which the event the fact relates to occurred.

### Combining INSERT and UPDATE statements

Logic to implement **Type 1** and **Type 2** updates can be complex, and there are various techniques you can use. For example, you could use a combination of **UPDATE** and **INSERT** statements.

```sql
-- New Customers
INSERT INTO dbo.DimCustomer
SELECT stg.*
FROM dbo.StageCustomers AS stg
WHERE NOT EXISTS
    (SELECT * FROM dbo.DimCustomer AS dim
    WHERE dim.CustomerAltKey = stg.CustNo)

-- Type 1 updates (name)
UPDATE dbo.DimCustomer
SET CustomerName = stg.CustomerName
FROM dbo.StageCustomers AS stg
WHERE dbo.DimCustomer.CustomerAltKey = stg.CustomerNo;

-- Type 2 updates (StreetAddress)
INSERT INTO dbo.DimCustomer
SELECT stg.*
FROM dbo.StageCustomers AS stg
JOIN dbo.DimCustomer AS dim
ON stg.CustNo = dim.CustomerAltKey
AND stg.StreetAddress <> dim.StreetAddress;
```

In the previous example, it's assumed that an incrementing surrogate key based on an **IDENTITY** column identifies each row, and that the highest value surrogate key for a given alternate key indicates the most recent or "current" instance of the dimension entity associated with that alternate key. In practice, many data warehouse designers include a Boolean column to indicate the current active instance of a changing dimension or use DateTime fields to indicate the active time periods for each version of the dimension instance. With these approaches, the logic for a type 2 change must include an INSERT of the new dimension row and an UPDATE to mark the current row as inactive.

### Using a MERGE statement

As an alternative to using multiple **INSERT** and **UPDATE** statements, you can use a single **MERGE** statement to perform an "upsert" operation to insert new records and update existing ones.

```sql
MERGE dbo.DimProduct AS tgt
    USING (SELECT * FROM dbo.StageProducts) AS src
    ON src.ProductID = tgt.ProductBusinessKey
WHEN MATCHED THEN
    -- Type 1 updates
    UPDATE SET
        tgt.ProductName = src.ProductName,
        tgt.ProductCategory = src.ProductCategory,
        tgt.Color = src.Color,
        tgt.Size = src.Size,
        tgt.ListPrice = src.ListPrice,
        tgt.Discontinued = src.Discontinued
WHEN NOT MATCHED THEN
    -- New products
    INSERT VALUES
        (src.ProductID,
        src.ProductName,
        src.ProductCategory,
        src.Color,
        src.Size,
        src.ListPrice,
        src.Discontinued);
```

 Note: For more information about the MERGE statement, see the [MERGE documentation for Azure Synapse Analytics](https://learn.microsoft.com/en-us/sql/t-sql/statements/merge-transact-sql?view=azure-sqldw-latest&preserve-view=true).

 