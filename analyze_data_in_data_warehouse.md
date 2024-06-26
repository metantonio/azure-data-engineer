# Analyze data in a relational data warehouse

Relational data warehouses are a core element of most enterprise Business Intelligence (BI) solutions, and are used as the basis for data models, reports, and analysis.

## Learning objectives

In this module, you'll learn how to:

 - Design a schema for a relational data warehouse.
 - Create fact, dimension, and staging tables.
 - Use SQL to load data into data warehouse tables.
 - Use SQL to query relational data warehouse tables.

## Introduction

**Relational data warehouses** are at the center of most enterprise business intelligence (BI) solutions. While the specific details may vary across data warehouse implementations, a common pattern based on a **denormalized, multidimensional schema** has emerged as the standard design for a relational data warehouse.

Azure Synapse Analytics includes a highly scalable relational database engine that is optimized for data warehousing workloads. By using dedicated SQL pools in Azure Synapse Analytics, you can create databases that are capable of hosting and querying huge volumes of data in relational tables.

## Design a data warehouse schema

Like all relational databases, **a data warehouse contains tables** in which the data you want to analyze is stored. Most commonly, these **tables are organized in a schema** that is optimized for multidimensional modeling, in which numerical measures associated **with events known as facts** can be aggregated by the attributes of associated entities across multiple dimensions. For example, measures associated with a sales order (such as the amount paid or the quantity of items ordered) can be aggregated by attributes of the date on which the sale occurred, the customer, the store, and so on.

### Tables in a data warehouse

A common pattern for relational **data warehouses** is to define a **schema** that **includes two kinds of table: dimension tables and fact tables.**

#### Dimension tables

**Dimension tables describe business entities**, such as products, people, places, and dates. **Dimension tables contain columns for attributes of an entity**. For example, a customer entity might have a first name, a last name, an email address, and a postal address (which might consist of a street address, a city, a postal code, and a country or region). In addition to attribute columns, a dimension table contains a unique key column that uniquely identifies each row in the table. In fact, **it's common for a dimension table to include two key columns**:

 - ***A surrogate key*** that is specific to the data warehouse and **uniquely identifies each row** in the dimension table in the data warehouse - usually an incrementing integer number.
 - ***An alternate key***, often a natural or business key that is used to **identify a specific instance of an entity** in the transactional source system from which the entity record originated - such as a product code or a customer ID.

    **Note**: Why have two keys? There are a few good reasons:

    - The data warehouse may be populated with data from multiple source systems, which can lead to the risk of duplicate or incompatible business keys.
    - Simple numeric keys generally perform better in queries that join lots of tables - a common pattern in data warehouses.
    - Attributes of entities may change over time - for example, a customer might change their address. Since the data warehouse is used to support historic reporting, you may want **to retain a record for each instance of an entity at multiple points in time**; so that, for example, sales orders for a specific customer are counted for the city where they lived at the time the order was placed. In this case, multiple customer records would have the same business key associated with the customer, but different surrogate keys for each discrete address where the customer lived at various times.

An example of a dimension table for customer might contain the following data:

CustomerKey	| CustomerAltKey	| Name	| Email	| Street	| City	| PostalCode	| CountryRegion
:---	| :--	| :---	| :---	| :---	| :---:	| ---:	| :---:
123	| I-543	| Navin Jones	| navin1@contoso.com	| 1 Main St.	| Seattle	| 90000	| United States
124	| R-589	| Mary Smith	| mary2@contoso.com	| 234 190th Ave	| Buffalo	| 50001	| United States
125	| I-321	| Antoine Dubois	| antoine1@contoso.com	| 2 Rue Jolie	| Paris	| 20098	| France
126	| I-543	| Navin Jones	| navin1@contoso.com	| 24 125th Ave.	| New York	| 50000	| United States
...	| ...	| ...	| ...	| ...	| ...	| ...	| ...