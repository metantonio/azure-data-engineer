# Introduction to data engineering on Azure

## Learning Objetives

In this module you will learn how to:

 - Identify common data engineering tasks
 - Describe common data engineering concepts
 - Identify Azure services for data engineering

## Introduction

In most organizations, a **data engineer** is the primary role responsible for **integrating, transforming, and consolidating data** from various structured and unstructured data systems into structures that are suitable **for building analytics solutions**. An **Azure data engineer also helps ensure that data pipelines and data stores are high-performing, efficient, organized, and reliable**, given a specific set of business requirements and constraints.

## What is data engineering?

The **data engineer will often work with multiple types of data** to perform many operations using many scripting or coding languages that are appropriate to their individual organization.

### Types of data

There are three primary types of data that a data engineer will work with.

<a href="#">
    <img src="./img/types_data.jpeg" />
</a>

### Data operations

As a data engineer some of the main tasks that you'll perform in Azure include data integration, data transformation, and data consolidation.

#### Data integration

**Data Integration** involves **establishing links between operational and analytical services and data sources** to enable secure, reliable access to data across multiple systems. For example, a business process might rely on data that is spread across multiple systems, and a data engineer is required to establish links so that the required data can be extracted from all of these systems.

#### Data tranformation

Operational **data usually needs to be transformed into suitable structure and format for analysis**, often as part of an **extract, transform, and load (ETL) process**; though increasingly a variation in which you **extract, load, and transform (ELT)** the data is used to quickly ingest the data into a data lake and then apply "big data" processing techniques to transform it. Regardless of the approach used, the **data is prepared to support downstream analytical needs**.

#### Data consolidation

**Data consolidation is the process of combining data that has been extracted from multiple data sources into a consistent structure** - usually to support analytics and reporting. Commonly, data from operational systems is extracted, transformed, and **loaded into analytical stores such as a data lake or data warehouse**.

### Common languages

Data Engineers must be proficient with a range of tools and scripting languages - in particular **SQL and Python**, and potentially others.

 - **SQL** - One of the most common languages data engineers use is SQL, or Structured Query Language, which is a relatively easy language to learn. SQL uses queries that include SELECT, INSERT, UPDATE, and DELETE statements to directly work with the data stored in tables.

 - **Python** - Python is one of the most popular and fastest growing programming languages in the world. It's used for all sorts of tasks, including web programming and data analysis. It has emerged as the language to learn for machine learning, and is increasing in popularity in data engineering with the use of notebooks.

 - **Others** - Depending upon the needs of the organization and your individual skill set, you may also use other popular languages within or outside of notebooks including R, Java, Scala, .NET, and more. The use of notebooks is growing in popularity, and allows collaboration using different languages within the same notebook.
