# Practice assessment for exam DP-203: Data Engineering on Microsoft Azure

 1. You create a data flow activity in an Azure Synapse Analytics pipeline.

    You plan to use the data flow to read data from a fixed-length text file.

    You need to create the columns from each line of the text file. The solution must ensure that the data flow only writes three of the columns to a CSV file.

    Which three types of tasks should you add to the data flow activity? Each correct answer presents part of the solution.

    - [ ] aggregate
    - [x] derived column
    - [ ] flatten
    - [x] select
    - [x] sink

        - **Derived column**: This task is used to create new columns or modify existing columns based on expressions or conditions. You can use it to parse and transform the fixed-length fields into individual columns.
        - **Select**: This task allows you to choose which columns to include or exclude from the data flow. You will use it to select only the three columns that you want to write to the CSV file.
        - **Sink**: This task is used to write the output of the data flow to a destination, such as a CSV file.

 2. You have source data that contains an array of JSON objects. Each JSON object has a child array of JSON objects.

    You create a data flow activity in an Azure Synapse Analytics pipeline.

    You need to transform the source so that it can be written to an Azure SQL Database table where each row represents an element of the child array, along with the values of its parent element.

    Which type of task should you add to the data flow activity?

    - [x] flatten
    - [ ] parse
    - [ ] pivot
    - [ ] unpivot

         - Flatten flattens JSON arrays.
         - Parse parses data.
         - Unpivot creates new rows based on the names of columns.
         - Pivot creates new columns based on the values of rows.


 3. You have an Azure Data Factory pipeline that uses Apache Spark to transform data.

    You need to run the pipeline.

    Which PowerShell cmdlet should you run?

    - [x] ``Invoke-AzDataFactoryV2Pipeline``
    - [ ] ``Invoke-AzureDataFactoryV2Pipeline``
    - [ ] ``Start-AzDataFactoryV2Pipeline``
    - [ ] ``Start-AzureDataFactoryV2Pipeline``

        - The ``Invoke-AzDataFactoryV2Pipeline`` cmdlet is used to start a Data Factory pipeline.

 4. You have an Azure subscription that contains a Delta Lake solution. The solution contains a table named **employees**.

    You need to view the contents of the employees table from 24 hours ago. You must minimize the time it takes to retrieve the data.

    What should you do?

    - [x] Query the table by using ``TIMESTAMP AS OF``.
    - [ ] Query the table by using ``VERSION AS OF``.
    - [ ] Restore the database from a backup and query the table.
    - [ ] Restore the table from a backup and query the table.


         - Querying the table by using ``TIMESTAMP AS OF`` is the correct way to view historic data in a delta lake.
         - Restoring the database from a backup and querying the table requires a backup and takes time.
         - Restoring the table from a backup and querying the table requires a backup and takes time.
         - Querying the table by using ``VERSION AS OF`` is incorrect. You still need to query the system to see which version you are interested in, which takes more time than using ``TIMESTAMP AS OF``.

 5. You design an Azure Data Factory data flow activity to move large amounts of data from text files to an Azure Synapse Analytics database. You add a data flow script to your data flow. The data flow in the designer has the following tasks:

     - **distinctRows1**: Aggregate data by using myCols that produce columns.
     - **source1**: Import data from DelimitedText1.
     - **derivedColumn1**: Create and update the C1 columns.
     - **select1**: Rename derivedColumn1 as select1 with columns C1.
     - **sink1**: Add a sink dataset.

    You need to ensure that all the rows in source1 are deduplicated.

    What should you do?

    - [ ] Change the incoming stream for derivedColumn1 to distinctRows1.
    - [x] Change the incoming stream for **distinctRows1** to **source1**.
    - [ ] Create a new aggregate task after source1 and copy the script to the aggregate task.
    - [ ] Create a new flowlet task after source1.

         - Changing the incoming stream for **distinctRows1** to **source1** will move the dedupe script right after source1, and only retrieve distinct rows.
         - Creating a new aggregate task after source1 and copying the script to the aggregate task will not work, and cause errors in the flow.
         - Changing the incoming stream for derivedColumn1 to distinctRows1 will break the flow as there will be no data coming into distinctRows1.
         - Creating a new flowlet task after source1 adds a subflow to the task.