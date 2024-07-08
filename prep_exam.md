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


 6. You have an Azure Stream Analytics solution that receives data from multiple thermostats in a building.

    You need to write a query that returns the average temperature per device every five minutes for readings within that same five minute period.

    Which two windowing functions could you use?

    - [x] ``HoppingWindow``
    - [ ] ``SessionWindow``
    - [ ] "SlidingWindow"
    - [x] ``TumblingWindow``

         - Tumbling windows: This window function generates results at non-overlapping intervals. For example, a tumbling window with a duration of five minutes will provide the average temperature for each five-minute period without any overlap.

         - Hopping windows: This window function generates results at regular intervals but allows for overlapping time periods. For instance, a hopping window with a hop size of five minutes and a window size of five minutes will generate results every five minutes, including data from the overlapping periods.

         - Sliding windows are used to create aggregations for so many events, not at identical timelapses.

         - Snapshot windows aggregate all events with the same timestamp.


 7. You have an Azure Stream Analytics job named Job1. Job1 is configured to use one **Streaming Unit (SU)** and can be parallelized for up to three nodes.

    You need to ensure there are three nodes available for the job.

    What is the minimum number of SUs you should configure?

    - [ ] 3
    - [ ] 6
    - [x] 18
    - [ ] 24

         - **Each six SUs is one node**; therefore three nodes will require a minimum of 18 SUs.

 8. You create an Azure Stream Analytics job. You run the job for five hours.

    You review the logs and notice multiple instances of the following message.

    ```json
    {
        "message Time":"2019-02-04 17:11:52Z",
        "error":null, 
        "message":"First Occurred: 02/04/2019 17:11:48 | Resource Name: ASAjob | Message: Source 'ASAjob' had 24 data errors of kind 'LateInputEvent' between processing times '2019-02-04T17:10:49.7250696Z' and '2019-02-04T17:11:48.7563961Z'. Input event with application timestamp '2019-02-04T17:05:51.6050000' and arrival time '2019-02-04T17:10:44.3090000' was sent later than configured tolerance.",
        "type":"DiagnosticMessage",
        "correlation ID":"49efa148-4asd-4fe0-869d-a40ba4d7ef3b"
    }
    ```

    You need to ensure that these events are not dropped.

    What should you do?

    - [ ] Decrease the number of Streaming Units (SUs) to 3.
    - [ ] Increase the number of Streaming Unit (SUs) for the job to 12.
    - [x] Increase the tolerance for late arrivals.
    - [ ] Increase the tolerance for out-of-order events.

         - Increasing the tolerance for late arrivals ensures that late arrivals are not dropped.
         - The error is about late arrivals, not out-of-order events.
         - Increasing the number of SUs to 12 will not change how late arrivals are handled.
         - Decreasing the number of SUs to 3 will not change how late arrivals are handled.

 9. You have an Azure Stream Analytics job named Job1.

    Job1 runs continuously and **executes non-parallelized queries**.

    You need to minimize the impact of Azure node updates on Job1. The solution must minimize costs.

    To what should you increase the Scale Units (SUs)?

    - [ ] 2
    - [ ] 3
    - [ ] 6
    - [x] 12

         - Update a Node requires a maintenance stop, so Increasing the SUs to 12 still uses two nodes. The other options still use a single node that will stop for maintenance.


 10. You have an **Azure Data Factory pipeline** named Pipeline1.

     You need to ensure that Pipeline1 runs when an email is received.

     What should you use to create the trigger?

- [x] an Azure logic app
- [ ] the Azure Synapse Analytics pipeline designer
- [ ] the Data Factory pipeline designer

    - A logic app can be triggered by an email, and then run a pipeline.
    - Only timer, event hub, and storage triggers can be added from the designer.


11. You have an Azure Data Factory pipeline named Pipeline1. Pipeline1 executes many API write operations every time it runs. Pipeline1 is scheduled to run every five minutes.

    After executing Pipeline1 10 times, you notice the following entry in the logs.

    ``Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.``

    You need to ensure that you can run Pipeline1 every five minutes.

    What should you do?

- [ ] Change the compute size to large.
- [x] Create a new integration runtime and a new Pipeline as a copy of Pipeline1. Configure both pipelines to run every 10 minutes, five minutes apart.
- [ ] Create a second trigger and set each trigger to run every 10 minutes, five minutes apart.
- [ ] Create another pipeline in the data factory and schedule each pipeline to run every 10 minutes, five minutes apart.

     - There is a limit of simultaneous pipelines in an integration runtime. You need to split the pipeline to run into multiple runtimes.

     - Compute size will not affect integration runtime limits.

     - Creating another pipeline in the data factory and scheduling each pipeline to run every 10 minutes, five minutes apart, will cause the same limitation.

     - Creating a second trigger and setting each trigger to run every 10 minutes, five minutes apart, still uses the same integration runtime.


12.  You have an Azure Data Factory named datafactory1.

     You configure datafacotry1 to use Git for source control.

     You make changes to an existing pipeline.

     When you try to publish the changes, you notice the following message displayed when you hover over the Publish All button.

     ``Publish from ADF Studio is disabled to avoid overwriting automated deployments. If required you can change publish setting in Git configuration.``

     You need to allow publishing from the portal.

     What should you do?

- [x] Change the Automated publish config setting.
- [ ] Select **Override live mode** in the Git Configuration. (Please, don't even think about use this ``ಠ_ಠ``)
- [ ] Use a Git client to merge the collaboration branch into the live branch.
- [ ] Use the browser to create a pull request.

     - Changing the Automated publish config setting defaults to Disable publish when Git is configured.

     - Selecting Override live mode in the Git configuration copies the data from the collaboration branch to the live branch.

     - Using a Git client to merge the collaboration branch into the live branch does the same thing as Override live mode.

     - Using the browser to create a pull request creates a pull request that must be approved, but still does not publish from Data Factory.

13. You have an Azure Data Factory pipeline named Pipeline1.

    Which two types of triggers can you use to start Pipeline1 directly? Each correct answer presents a complete solution.

- [x] custom event
- [ ] SharePoint list
- [x] tumbling window
- [ ] Twitter post

     - Tumbling window is a valid type of trigger in Data Factory.
     - Custom event is a valid type of trigger in Data Factory.
     - You cannot use SharePoint to trigger a Data Factory pipeline directly. You can do it from a logic app.
     - You cannot use Twitter to trigger a Data Factory pipeline directly. You can do it from a logic app.

14. You have an Azure Data Factory pipeline named Pipeline1.

    You need to send an email message if Pipeline1 fails.

    What should you do?

- [ ] Create a fail activity in the pipeline and set a Failure predecessor on the activity for the last activity in Pipeline1.
- [ ] Create a metric in the Data Factory resource.
- [x] Create an alert in the Data Factory resource.
- [ ] Create an if condition activity in the pipeline and set a Failure predecessor on the activity for the last activity in Pipeline1.

     - An alert can trigger an email.
     - Fail activities can only set up a failure message, not send emails. Metrics do not trigger events. An If condition creates a branching option in the pipeline, but by itself, it cannot send an email.

15. You are creating an Azure Data Factory pipeline.

    You ***need to store the passwords used to connect to resources***.

    Where should you store the passwords?

- [x] Azure Key Vault
- [ ] Azure Repos
- [ ] Azure SQL Database
- [ ] Data Factory

     - Passwords for resources are not stored in the Data Factory pipeline. It is recommended that the passwords be stored in Key Vault so they can be stored securely.

16. You are testing a change to an Azure Data Factory pipeline.

    You **need to check the change into source control without affecting other users’ work** in the data factory.

    What should you do?

- [x] Save the change to a forked branch in the source control project.
- [ ] Save the change to the master branch of the source control project.
- [ ] Save the changed pipeline to your workstation.

     - Create a forked branch won't affect other users' work and you will save the change into the source control

17. You have an Azure Synapse Analytics data pipeline.

    You need to run the pipeline at scheduled intervals.

    What should you configure?

- [ ] a control flow
- [ ] a sink
- [x] a trigger
- [ ] an activity

     - **A trigger is needed to initiate a pipeline run**. Control flow is an activity that implements processing logic. Activities are tasks within a pipeline that cannot trigger the pipeline. A sink represents a target in a data flow but does not provide trigger capability.


18. You are developing an **Apache Spark pipeline** to ***transform data from a source to a target***.

    You need to filter the data in a column named **Category** where the category is cars.

    Which command should you run?

- [x] df.select("ProductName", "ListPrice").where((df["Category"] == "Cars"))
- [ ] df.select("ProductName", "ListPrice") | where((df["Category"] == "Cars"))
- [ ] df.select("ProductName", "ListPrice").where((df["Category"] -eq "Cars"))
- [ ] df.select("ProductName", "ListPrice") | where((df["Category"] -eq "Cars"))

     - The correct format of the ``where`` statement is putting ``.where`` after the select statement on Python (is a method).

19. You have a database named **DB1** and a data warehouse named **DW1**.

    You need to ensure that all changes to **DB1** are stored in **DW1**.The solution must capture the new value and the existing value and ***store each value as a new record***.

    What should you include in the solution?

- [x] change data capture
- [ ] change tracking
- [ ] merge replication
- [ ] transactional replication

     - ***Change data capture*** captures every change to the data and presents the new values as a new row in the tables.

20. You have a database named DB1 and a data warehouse named DW1.

    You need to ensure that all changes to DB1 are stored in DW1. The solution must meet the following requirements:

     - Identify each row that has changed.
     - Minimize the performance impact on the source system.

    What should you include in the solution?

- [ ] change data capture
- [x] change tracking
- [ ] merge replication
- [ ] transactional replication

     - Change tracking captures the fact that a row was changed without tracking the data that was changed. Change tracking requires less server resources than change data capture.

21. You have a database named DB1 and a data warehouse named DW1.

    You need to ensure that all changes to DB1 are stored in DW1. The solution must meet the following requirements:

     - Identify that a row has changed, but not the final value of the row.
     - Minimize the performance impact on the source system.

    What should you include in the solution?

- [ ] change data capture
- [x] change tracking
- [ ] merge replication
- [ ] snapshot replication

     - Change tracking captures the fact that a row was changed without tracking the data that was changed. Change tracking requires fewer server resources than change data capture.

22. You have an Azure subscription that contains an Azure Stream Analytics solution.

    You need to write a query that calculates the average rainfall per hour. The solution must segment the data stream into a contiguous **series of fixed-size**, **non-overlapping** time segments.

    Which windowing function should you use?

- [ ] collect
- [ ] sliding
- [x] tumbling
- [ ] VARP

     - Sliding windows generate events for points in time when the content of the window actually changes. Events in hopping and sliding windows can belong to more than one window result set. ***Tumbling window functions segment a data stream into a contiguous series of fixed-size, non-overlapping time segments. Events cannot belong to more than one tumbling window***. VARP and collect are aggregated functions, not windowing.

23. You use an Azure Databricks pipeline to process a stateful streaming operation.

    You need to reduce the amount of state data to improve latency during a long-running steaming operation.

    What should you use in the streaming DataFrame?

- [ ] a partition
- [ ] a tumbling window
- [x] a watermark
- [ ] RocksDB state management

     - **Watermarks interact with output modes** to control when data is written to the sink. Because watermarks reduce the total amount of state information to be processed, effective use of watermarks is essential for efficient stateful streaming throughput. **Partitions are useful to improve performance but not to reduce state information**. A tumbling window segments a data stream into a contiguous series of fixed-size time segments. RocksDB state management helps debug job slowness.

24. You have 500 IoT devices and an Azure subscription.

    You plan to build a data pipeline that will process real-time data from the devices.

    You need to ensure that the devices can send messages to the subscription.

    What should you deploy?

- [x] an Azure event hub
- [ ] an Azure Storage account
- [ ] an Azure Stream Analytics workspace

     - To send real-time data from IoT devices to an Azure subscription, the messages are received by an event hub.

25. You have 100 retail stores distributed across **Asia, Europe, and North America**.

    You are developing an analytical workload using Azure Stream Analytics that contains sales data for stores in different regions. The workload contains a **fact table** with the following columns:

    - **Date**: Contains the order date
    - **Customer**: Contains the customer ID
    - **Store**: Contains the store ID
    - **Region** Contains the region ID
    - **Product**: Contains the product ID
    - **Price**: Contains the unit price per product
    - **Quantity**: Contains the quantity sold
    - **Amount**: Contains the price multiplied by quantity

    You need to design a **partition solution for the fact table**. The solution must meet the following requirements:

    - Optimize read performance when querying sales data for a single region in a given month.
    - Optimize read performance when querying sales data for all regions in a given month.
    - Minimize the number of partitions.

    Which column should you use for partitioning?

- [ ] Date partitioned by month
- [x] Product
- [ ] Region
- [ ] Store

     - ***Product ensures parallelism when querying data from a given month within the same region, or multiple regions***.
     - Using date and partitioning by month, all sales for a month will be in the same partition, not providing parallelism.
     - All sales for a given region will be in the same partition, not providing parallelism.
     - Since a store is in a single region, it will still not provide parallelism for the same region.

26. You plan to deploy an app that will distribute files across multiple Azure Storage accounts.

    You need to recommend a **partitioning strategy** that meets the following requirements:

     - Optimizes the data distribution balance.
     - Minimizes the creation of extra tables.

    What should you recommend?

- [x] Hash
- [ ] lookup
- [ ] range

     - Lookup partitioning requires a lookup table to identify which partition data should reside in. Range partitioning does not provide optimization for balancing.
     - Hash partitioning is optimized for data distribution and uses a hash function to eliminate the need for a lookup table.

27. You are evaluating the use of Azure Data Lake Storage Gen2.

    What should you consider when choosing a **partitioning strategy**?

- [ ] access policies
- [x] data residency
- [ ] file size
- [ ] geo-replication requirements

     - Geo-replication will affect pricing, not partitioning.
     - Data residency must be considered to identify whether different datasets can only exist in specific regions.
     - File size does not affect partitioning.
     - Access policies can be applied at folder and container levels, and do not require partitioning.

28. You are importing data into an Azure Synapse Analytics database. The data is being ***inserted by using PolyBase***.

    You need to **maximize network throughput for the import process**.

    What should you use?

- [ ] Scale the target database out.
- [ ] Scale the target database up.
- [x] Shard the source data across multiple files.
- [ ] Shard the source data across multiple storage accounts.

     - Sharding the source data into multiple files will increase the amount of bandwidth available to the import process.

29. You have an Azure Synapse Analytics database named DB1.

    You plan to import data into DB1.

    You need to maximize the performance of the data import.

    What should you implement?

- [ ] functional partitioning on the source data
- [x] horizontal partitioning on the source data
- [ ] table partitioning on the target database
- [ ] vertical partitioning on the source data

     - By using horizontal partitioning, you can improve the performance of the data load. As more server resources and bandwidth are available to the source files, the import process gets faster.


30. You are designing a database solution that will host data for multiple business units.

    You need to ensure that queries from one business unit do not affect the other business units.

    Which type of partitioning should you use?

- [x] functional
- [ ] horizontal
- [ ] table
- [ ] vertical 

     - By using functional partitioning, **different users of the database can be isolated from each other** to ensure that one business unit does not affect another business unit.

31. You plan to deploy an Azure Synapse Analytics solution that will use the ``Retail database template`` and include three tables from the Business Metrics category.

    You need to create a one-to-many relationship from a table in Retail to a table in Business Metrics.

    What should you do first?

- [x] Create a database.
- [ ] Publish the database.
- [ ] Select the table in Business Metric.
- [ ] Select the table in Retail. 

     - You cannot add relationships until a database is created. You can only view relationships before a database is created. You can only publish the database after the database has been created.

32. You have a data solution that includes an Azure SQL **database named SQL1** and an Azure Synapse **database named** **SYN1**. SQL1 contains a table named Table1. Data is loaded from SQL1 to the SYN1.

    You need to **ensure that Table1 supports incremental loading**.

    What should you do?

- [x] Add a new column to track lineage in Table1.
- [ ] Define a new foreign key in Table1.
- [ ] Enable data classification in Microsoft Purview.
- [ ] Enable data lineage in Microsoft Purview.

     - A new column of type date or int can be used to track lineage in a table and be used for filtering during an incremental load.
     - Data lineage in Microsoft Purview cannot be used to assist an incremental load. It is just used for tracing lineage.
     - Data classification cannot be used for incremental loading.
     - Foreign keys are used for relationship between tables, not lineage.

33. You have an Azure subscription that uses Microsoft Purview.

    You need to identify which assets have been cataloged by Microsoft Purview.

    What should you use?

- [ ] Azure Data Factory
- [ ] Azure Data Studio
- [ ] the Microsoft Purview compliance portal
- [x] the Microsoft Purview governance portal

     - The governance portal allows you to view and search against the metadata within your Microsoft Purview catalog,

34. You need to limit sensitive data exposure to non-privileged users. You must be able to grant and revoke access to the sensitive data.

What should you implement?

- [ ] Always Encrypted
- [x] dynamic data masking
- [ ] row-level security (RLS)
- [ ] transparent data encryption (TDE)

    -  Dynamic data masking helps prevent unauthorized access to sensitive data by enabling customers to designate how much of the sensitive data to reveal with minimal impact on the application layer. It is a policy-based security feature that hides the sensitive data in the result set of a query over designated database fields, while the data in the database is not changed.

35. You have an Azure Data Lake Storage Gen2 account.

    You grant developers Read and Write permissions by using ACLs to the files in the path \root\input\cleaned.

    The developers report that they cannot open the files.

    How should you modify the permissions to ensure that the developers can open the files?

- [ ] Add Contributor permission to the developers.
- [ ] Add Execute permissions to the files.
- [x] Grant Execute permissions to all folders.
- [ ] Grant Execute permissions to the root folder only.

     - If you are granting permissions by using only ACLs (not Azure RBAC), then to grant a security principal read or write access to a file, you will need to grant the security principal Execute permissions to the root folder of the container and to each folder in the hierarchy of folders that lead to the file.
     - Adding Contributor permissions to the developers will not help as this type of permission does not provide access to the data.

36. You have an Azure Storage account named account1.

    You need to ensure that requests to account1 can only be made from specific domains.

    What should you configure?

- [ ] blob public access
- [ ] CDN
- [x] CORS
- [ ] secure transfer

     - By using CORS, you can specify which domains a web request is allowed to respond to. If the domain is not listed as an approved domain, the request will be rejected.

37. You have an Azure Synapse Analytics workspace.

    You need to measure the performance of SQL queries running on the dedicated SQL pool.

    Which two actions achieve the goal? Each correct answer presents a complete solution

- [ ] From the Monitor page of Azure Synapse Studio, review the Pipeline runs tab.
- [x] From the Monitor page of Azure Synapse Studio, review the SQL requests tab.
- [x] Query the ``sys.dm_pdw_exec_request`` view.
- [ ] Query the ``sys.dm_pdw_exec_sessions`` view.

     - You should open the Monitor page and review the SQL request tab where you will find all the queries running on the dedicated SQL pools.
     - You should query the ``sys.dm_pdw_exec_requests`` dynamic management view, as it contains information about the queries, including their duration.
     - The ``sys.dm_pdw_exec_sessions`` dynamic management view contains information about connections to the database.
     - Opening the Monitor page and reviewing the Pipeline runs tab displays information about the pipelines.

38. You have an Azure Synapse Analytics workspace.

    You need to configure the diagnostics settings for pipeline runs. You must retain the data ***for auditing purposes indefinitely*** and minimize costs associated with retaining the data.

    Which destination should you use?

- [x] Archive to a storage account.
- [ ] Send to a Log Analytics workspace.
- [ ] Send to a partner solution.
- [ ] Stream to an Azure event hub.

    - You should choose to archive to a storage account as it is useful for audit, static analysis, or backup. Compared to using Azure Monitor logs or a Log Analytics workspace, this storage is less expensive, and logs can be kept there indefinitely.

     - You should not choose to stream to an event hub since the data can be sent to external systems, such as third-party SIEMs and other Log Analytics solutions.

     - You should not choose to send the data to a Log Analytics workspace, as this option is used to help you to integrate the data into queries, alerts, and visualizations with existing log data.

     - You should not send the data to a partner solution, as this is only useful when using a partner.

39. You have two Azure Data Factory pipelines.

    You need to monitor the runtimes of the pipelines.

    What should you do?

- [ ] From Azure Data Studio, use the performance monitor view.
- [ ] From the Azure Monitor blade of the Azure portal, review the metrics.
- [x] From the Data Factory blade of the Azure portal, review the Monitor & Manage tile.

     - The runtimes of existing pipelines is available in the Azure portal, on the Data Factory blade, under the Monitor & Manage tile.
     - Azure Data Studio is not used to monitor Data Factory.

40. You have an Azure Data Factory named ADF1.

    You need to ensure that you can analyze pipeline runtimes for ADF1 for the ***last 90 days***.

    What should you use?    

- [ ] Azure Data Factory
- [x] Azure Monitor
- [ ] Azure Stream Analytics
- [ ] Azure App Insights

     - Data Factory only stores pipeline runtimes for ***45 days***. To view the data for a longer period, that data must be sent to Azure Monitor, where the information can then be retrieved and viewed.

41. You have an Azure Data Factory pipeline named S3toDataLake1 that copies data between Amazon S3 storage and Azure Data Lake storage.

    You need to use the Azure Data Factory Pipeline runs dashboard to view the history of runs over a specific time range and group them by tags for S3toDataLake1.

    Which view should you use?

- [ ] Activity
- [ ] Debug
- [x] Gantt
- [ ] List

     - Correct: Gantt view allows you to see all the pipeline runs grouped by name, annotation, or tag created in the pipeline, and it also displays bars relative to how long the run took.

42. You have an Azure Data Factory named ADF1.

    You configure ADF1 to send data to Log Analytics in Azure-Diagnostics mode.

    You need to review the data.

    Which table should you query?

- [ ] ``ADFActivityRun``
- [ ] ``ADFPipelineRun``
- [ ] ``ADFSSISIntegrationRuntimeLogs``
- [ ] ``ADFSSISPackageExecutableStatistics``
- [x] ``AzureDiagnostics``

     - When Data Factory is configured to send logging data to Log Analytics and is in Azure-Diagnostics mode, the data will be sent to the ``AzureDiagnostics`` table in Log Analytics.

     - The ``ADFActivityRun``, ``ADFPipelineRun``, ``ADFSSISIntegrationRuntimeLogs``, and ``ADFSSISPackageExecutableStatistics`` tables are used when the Data Factory is in Resource-Specific mode.

43. You monitor an Apache Spark job that has been slower than usual during the last two days. ***The job runs a single SQL statement in which two tables are joined***.

    You discover that one of the tables has significant data skew.

    You need to improve job performance.

    Which hint should you use in the query?

- [ ] ``COALESCE``
- [ ] ``REBALANCE``
- [ ] `REPARTITION`
- [x] ``SKEW``

     - You should use the SKEW hint in the query.
     - The COALESCE hint reduces the number of partitions to the specified number of partitions.
     - The REPARTITION hint is used to specify the number of partitions using the specified partitioning expressions.
     - The REBALANCE hint can be used to rebalance the query result output partitions, so that every partition is a reasonable size (not too small and not too big).

44. You monitor an Azure Data Factory pipeline that occasionally fails.

    You need to implement an alert that will contain failed pipeline run metrics. The solution must minimize development effort.

    Which two actions achieve the goal? Each correct answer presents a complete solution.

- [x] From Azure portal, create an alert and add the metrics.
- [x] From the Monitor page of Azure Data Factory Studio, create an alert.
- [ ] Implement a Web activity in the pipeline.
- [ ] Implement a WebHook activity in the pipeline.

     - The options that do not require any development effort are to create an alert by using Azure Monitor or in Azure Data Factory. You can still create custom alerts by implementing a Web activity or a WebHook activity in the pipeline, but some services will require additional effort to read the alert.

45. You have an Azure Synapse Analytics workspace.

    Users report that queries that have a label of ``‘query1’`` are slow to complete.

    You need to identify all the queries that have a label of ``‘query1’``.

    Which query should you run?

- [ ] ``SELECT * FROM sys.dm_pdw_dms_workers WHERE label = 'query1'``
- [x] ``SELECT * FROM sys.dm_pdw_exec_requests WHERE label = 'query1'``
- [ ] ``SELECT * FROM sys.dm_pdw_request_steps WHERE label = 'query1'``
- [ ] ``SELECT * FROM sys.dm_pdw_sql_requests WHERE label = 'query1'``

     - Labels for queries are available ``from sys.dm_pdw_exec_requests``. Once the request IDs for the queries are identified, the request IDs can be used for the other dynamic management views.

46. You have an Azure Synapse Analytics workspace that includes an Azure Synapse Analytics cluster named Cluster1.

    You need to review the estimated execution plan for a query on a specific node of Cluster1. The query has a spid of 94 and a distribution ID of 5.

    Which command should you run?

- [x] `DBCC PDW_SHOWEXECUTIONPLAN (5, 94)`
- [ ] ``DBCC SHOWEXECUTIONPLAN (5,94)``
- [ ] ``SELECT * FROM sys.dm_exec_query_plan WHERE spid = 94 AND distribution_id = 5``
- [ ] `SELECT * FROM sys.pdw_nodes_exec_query_plan WHERE spid = 94 AND distribution_id = 5`

     - The execution plan for the specific distribution is available by busing the ``DBCC PDW_SHOWEXECUTIONPLAN`` command.

47. You have an Azure Synapse Analytics database named DB1.

    You need to increase the ***concurrency*** available for DB1.

    Which cmdlet should you run?

- [x] `Set-AzSqlDatabase`
- [ ] `Start-AzSqlDatabaseActivty`
- [ ] ``Update-AzKustoDatabase``
- [ ] ``Update-AzSynapseSqlDatabase``

     - Increasing the concurrency on a database requires scaling the database up by using the ``Set-AzSqlDatabase`` cmdlet.

48. You have an Azure subscription that contains an Azure SQL database named DB1.

    You need to implement row-level security (RLS) for DB1. The solution must block users from updating rows with values that violate RLS.

    Which block predicate should you use?

- [ ] ``AFTER INSERT``
- [x] ``AFTER UPDATE``
- [ ] ``BEFORE DELETE``
- [ ] ``BEFORE UPDATE``

    - AFTER UPDATE prevents users from updating rows to values that violate the predicate. 
    - AFTER INSERT prevents users from inserting rows that violate the predicate. 
    - BEFORE UPDATE prevents users from updating rows that currently violate the predicate. Blocks delete operations if the row violates the predicate

49. You have an Azure Synapse Analytics SQL pool.

    You need to monitor **currently-executing query executions** in the SQL pool.

    Which three dynamic management views should you use? Each correct answer presents part of the solution.

- [ ] sys.dm_exec_cached_plans
- [x] sys.dm_pdw_exec_requests
- [ ] sys.dm_pdw_errors
- [x] sys.dm_pdw_request_steps
- [x] sys.dm_pdw_sql_requests

     - An Azure Synapse Analytics SQL pool uses the ``sys.dm_pdw_exec_requests``, ``sys.dm_pdw_request_steps``, and ``sys.dm_pdw_sql_requests`` views to monitor SQL pool activity. The ``sys.dm_exec_cached_plans`` view stores execution plans that do not show current activity. The ``sys.dm_pdw_errors`` view stores errors, not current activity.

50. You have a **job that aggregates data over a five-second tumbling window**.

    You are monitoring the job and notice that the **SU (Memory) %** Utilization metric is more than 80 percent, and the Backlogged Input Events metric shows values greater than 0.

    What should you do to resolve the performance issue?

- [ ] Change the compatibility level.
- [ ] Change the tumbling window to a snapshot window.
- [ ] Create a user-defined aggregate to perform the aggregation.
- [x] Increase the number of the Streaming Units (SU).

     - You should **increase the number of SUs because the job is running out of resources**. *When the Backlogged Input Events metric is greater than zero, the job is not able to process all incoming events*.

     - You should not change the compatibility level, as this option is not responsible for faster event processing.

     - You should not write a user aggregate, as it this is useful only if the aggregation function is available in the SQL dialect of the query.

     - You should not change the tumbling window to a snapshot window, as this can lead to data loss.

51. Data in a relational database table is… 

    - [x] Structured
    - [ ] Semi-stuctured
    - [ ] Unstuctured

52. In a data lake, data is stored in? 

    - [ ] Relational tables
    - [x] Files
    - [ ] A single  JSON document

53. Which of the following Azure services provides capabilities for running data pipelines AND managing analytical data in a data lake or relational data warehouse? 

    - [ ] Azure Stream Analytics
    - [x] Azure Synapse Analytics
    - [ ] Azure Databricks

54. Azure Data Lake Storage Gen2 stores data in… 

    - [ ] A document database hosted in Azure Cosmos DB.
    - [x] An HDFS-compatible file system hosted in Azure Storage.
    - [ ] A relational data warehouse hosted in Azure Synapse Analytics.

55. What option must you enable to use Azure Data Lake Storage Gen2? 

    - [ ] Global replication
    - [ ] Data encryption
    - [x] Hierarchical namespace

56. Which feature of Azure Synapse Analytics enables you to transfer data from one store to another and apply transformations to the data at scheduled intervals?

    - [ ] Serverless SQL pool
    - [ ] Apache Spark pool
    - [x] Pipelines

57. You want to create a data warehouse in Azure Synapse Analytics in which the data is stored and queried in a relational data store. What kind of pool should you create? 

    - [ ] Serverless SQL pool
    - [x] Dedicated SQL pool
    - [ ] Apache Spark pool

58. A data analyst wants to analyze data by using Python code combined with text descriptions of the insights gained from the analysis. What should they use to perform the analysis? 

    - [x] A notebook connected to an Apache Spark pool.
    - [ ] A SQL script connected to a serverless SQL pool.
    - [ ] A KQL script connected to a Data Explorer pool.

59. What function is used to read the data in files stored in a data lake? 

    - [ ] FORMAT
    - [ ] ROWSET
    - [x] OPENROWSET

60. What character in file path can be used to select all the file/folders that match rest of the path? 

    - [ ] &
    - [x] *
    - [ ] /

61. Which external database object encapsulates the connection information to a file location in a data lake store? 

    - [ ] FILE FORMAT
    - [x] DATA SOURCE
    - [ ] EXTERNAL TABLE

62. You need to store the results of a query in a serverless SQL pool as files in a data lake. Which SQL statement should you use? 

    - [ ] BULK INSERT
    - [x] CREATE EXTERNAL TABLE AS SELECT
    - [ ] COPY

63. Which of the following file formats can you use to persist the results of a query? 

    - [ ] CSV only
    - [ ] Parquet only
    - [x] CSV and Parquet

64. You drop an existing external table from a database in a serverless SQL pool. What else must you do before recreating an external table with the same location?  

    - [x] Delete the folder containing the data files for dropped table.
    - [ ] Drop and recreate the database
    - [ ] Create an Apache Spark pool

65. Which if the following statements is true of a lake database?  

    - [ ] Data is stored in a relational database store and cannot be directly accessed in the data lake files.
    - [ ] Data is stored in files that cannot be queried using SQL.
    - [x] A relational schema is overlaid on the underlying files, and can be queried using a serverless SQL pool or a Spark pool.

66. You need to create a new lake database for a retail solution. What's the most efficient way to do this? 

    - [ ] Create a sample database in Azure SQL Database and export the SQL scripts to create the schema for the lake database.
    - [x] Start with the Retail database template in Azure Synapse Studio, and adapt it as necessary.
    - [ ] Start with an empty database and create a normalized schema.

67. You have Parquet files in an existing data lake folder for which you want to create a table in a lake database. What should you do? 

    - [ ] Use a CREATE EXTERNAL TABLE AS SELECT (CETAS) query to create the table.
    - [ ] Convert the files in the folder to CSV format.
    - [x] Use the database designer to create a table based on the existing folder.

68. Which definition best describes Apache Spark? 

    - [ ] A highly scalable relational database management system.
    - [ ] A virtual server with a Python runtime.
    - [x] A distributed platform for parallel data processing using multiple languages.

69. You need to use Spark to analyze data in a parquet file. What should you do? 

    - [x] Load the parquet file into a dataframe.
    - [ ] Import the data into a table in a serverless SQL pool.
    - [ ] Convert the data to CSV format.

70. You want to write code in a notebook cell that uses a SQL query to retrieve data from a view in the Spark catalog. Which magic should you use? 

    - [ ] %%spark
    - [ ] %%pyspark
    - [x] %%sql

71. Which method of the Dataframe object is used to save a dataframe as a file? 

    - [ ] toFile()
    - [x] write()
    - [ ] save()

72. Which method is used to split the data across folders when saving a dataframe? 

    - [ ] splitBy()
    - [ ] distributeBy()
    - [x] partitionBy()

73. What happens if you drop an external table that is based on existing files? 

    - [ ] An error – you must delete the files first
    - [x] The table is dropped from the metastore but the files remain unaffected
    - [ ] The table is dropped from the metastore and the files are deleted

74. Which of the following descriptions best fits Delta Lake? 

    - [ ] A Spark API for exporting data from a relational database into CSV files.
    - [x] A relational storage layer for Spark that supports tables based on Parquet files.
    - [ ] A synchronization solution that replicates data between SQL pools and Spark pools.

75. You've loaded a Spark dataframe with data, that you now want to use in a Delta Lake table. What format should you use to write the dataframe to storage? 

    - [ ] CSV
    - [x] PARQUET
    - [ ] DELTA

76. What feature of Delta Lake enables you to retrieve data from previous versions of a table? 

    - [ ] Spark Structured Streaming
    - [x] Time Travel
    - [ ] Catalog Tables

77. You have a managed catalog table that contains Delta Lake data. If you drop the table, what will happen? 

    - [x] The table metadata and data files will be deleted.
    - [ ] The table metadata will be removed from the catalog, but the data files will remain intact.
    - [ ] The table metadata will remain in the catalog, but the data files will be deleted.

78.  When using Spark Structured Streaming, a Delta Lake table can be which of the following? 

    - [ ] Only a source
    - [ ] Only a sink
    - [x] Either a source or a sink

79. In which of the following table types should an insurance company store details of customer attributes by which claims will be aggregated?? 

    - [ ] Staging table
    - [x] Dimension table
    - [ ] Fact table

80. You create a dimension table for product data, assigning a unique numeric key for each row in a column named **ProductKey**. The **ProductKey** is only defined in the data warehouse. What kind of key is **ProductKey**?

    - [x] A surrogate key
    - [ ] An alternate key
    - [ ] A business key

81. What distribution option would be best for a sales fact table that will contain billions of records?

    - [x] HASH
    - [ ] ROUND_ROBIN
    - [ ] REPLICATE

82. You need to write a query to return the total of the **UnitsProduced** numeric measure in the **FactProduction** table aggregated by the **ProductName** attribute in the **FactProduct** table. Both tables include a **ProductKey** surrogate key field. What should you do?

    - [ ] Use two SELECT queries with a UNION ALL clause to combine the rows in the FactProduction table with those in the FactProduct table.
    - [ ] Use a SELECT query against the FactProduction table with a WHERE clause to filter out rows with a ProductKey that doesn't exist in the FactProduct table.
    - [x] Use a SELECT query with a SUM function to total the UnitsProduced metric, using a JOIN on the ProductKey surrogate key to match the FactProduction records to the FactProduct records and a GROUP BY clause to aggregate by ProductName.

83. You use the RANK function in a query to rank customers in order of the number of purchases they have made. Five customers have made the same number of purchases and are all ranked equally as 1. What rank will the customer with the next highest number of purchases be assigned?

    - [ ] Two
    - [x] Six
    - [ ] One

84. You need to compare approximate production volumes by product while optimizing query response time. Which function should you use?

    - [ ] COUNT
    - [ ] NTILE
    - [x] APPROX_COUNT_DISTINCT

85. In which order should you load tables in the data warehouse?  

    - [x] Staging tables, then dimension tables, then fact tables
    - [ ] Staging tables, then fact tables, then dimension tables
    - [ ] Dimension tables, then staging tables, then fact tables

86. Which command should you use to load a staging table with data from files in the data lake?

    - [x] COPY
    - [ ] LOAD
    - [ ] INSERT

87. When a customer changes their phone number, the change should be made in the existing row for that customer in the dimension table. What type of slowly changing dimension does this scenario require? 

    - [ ] Type 0
    - [x] Type 1
    - [ ] Type 2

88. What does a pipeline use to access external data source and processing resources?   

    - [ ] Data Explorer pools
    - [x] Linked services
    - [ ] External tables

89. What kind of object should you add to a data flow to define a target to which data is loaded? 

    - [ ] Source
    - [ ] Transformation
    - [x] Sink

90. What must you create to run a pipeline at scheduled intervals? 

    - [ ] A control flow
    - [x] A trigger
    - [ ] An activity

91. What kind of pool is required to run a Synapse notebook in a pipeline?   

    - [ ] A Dedicated SQL pool
    - [ ] A Data Explorer pool
    - [x] An Apache Spark pool

92. What kind of pipeline activity encapsulates a Synapse notebook? 

    - [x] Notebook activity
    - [ ] HDInsight Spark activity
    - [ ] Script activity

93. A notebook cell contains variable declarations. How can you use them as parameters? 

    - [ ] Add a %%Spark magic at the beginning of the cell
    - [x] Toggle the Parameters cell setting for the cell
    - [ ] Use the var keyword for each variable declaration

94. Which of the following descriptions matches a hybrid transactional/analytical processing (HTAP) architecture.  

    - [ ] Business applications store data in an operational data store, which is also used to support analytical queries for reporting.
    - [x] Business applications store data in an operational data store, which is synchronized with low latency to a separate analytical store for reporting and analysis.
    - [ ] Business applications store operational data in an analytical data store that is optimized for queries to support reporting and analysis.

95. You want to use Azure Synapse Analytics to analyze operational data stored in a Cosmos DB for NoSQL container. Which Azure Synapse Link service should you use?

    - [ ] Azure Synapse Link for SQL
    - [ ] Azure Synapse Link for Dataverse
    - [x] Azure Synapse Link for Azure Cosmos DB

96. You plan to use Azure Synapse Link for Dataverse to analyze business data in your Azure Synapse Analytics workspace. Where is the replicated data from Dataverse stored? 

    - [ ] In an Azure Synapse dedicated SQL pool
    - [x] In an Azure Data Lake Gen2 storage container.
    - [ ] In an Azure Cosmos DB container.

97. You have an Azure Cosmos DB for NoSQL account and an Azure Synapse Analytics workspace. What must you do first to enable HTAP integration with Azure Synapse Analytics? 

    - [ ] Configure global replication in Azure Cosmos DB.
    - [ ] Create a dedicated SQL pool in Azure Synapse Analytics.
    - [x] Enable Azure Synapse Link in Azure Cosmos DB.

98. You have an existing container in a Cosmos DB core (SQL) database. What must you do to enable analytical queries over Azure Synapse Link from Azure Synapse Analytics? 

    - [ ] Delete and recreate the container.
    - [x] Enable Azure Synapse Link in the container to create an analytical store.
    - [ ] Add an item to the container.

99. You plan to use a Spark pool in Azure Synapse Analytics to query an existing analytical store in Azure Cosmos DB. What must you do? 

    - [x] Create a linked service for the Azure Cosmos DB database where the analytical store enabled container is defined.
    - [ ] Disable automatic pausing for the Spark pool in Azure Synapse Analytics.
    - [ ] Install the Azure Cosmos DB SDK for Python package in the Spark pool.

100. You're writing PySpark code to load data from an Azure Cosmos DB analytical store into a dataframe. What format should you specify? 

    - [ ] cosmos.json
    - [x] cosmos.olap
    - [ ] cosmos.sql

101. You're writing a SQL code in a serverless SQL pool to query an analytical store in Azure Cosmos DB. What function should you use? 

    - [ ] OPENDATASET
    - [ ] ROW
    - [x] OPENROWSET

102. From which of the following data sources can you use Azure Synapse Link for SQL to replicate data to Azure Synapse Analytics? 

    - [ ] Azure Cosmos DB.
    - [x] SQL Server 2022.
    - [ ] Azure SQL Managed Instance.

103. What must you create in your Azure Synapse Analytics workspace as a target database for Azure Synapse Link for Azure SQL Database? 

    - [ ] A serverless SQL pool
    - [ ] An Apache Spark pool
    - [x] A dedicated SQL pool

104. You plan to use Azure Synapse Link for SQL to replicate tables from SQL Server 2022 to Azure Synapse Analytics. What additional Azure resource must you create? 

    - [x] An Azure Storage account with an Azure Data Lake Storage Gen2 container
    - [ ] An Azure Key Vault containing the SQL Server admin password
    - [ ] An Azure Application Insights resource

105. Which definition of stream processing is correct? 

    - [x] Data is processed continually as new data records arrive.
    - [ ] Data is collected in a temporary store, and all records are processed together as a batch.
    - [ ] Data that is incomplete or contains errors is redirected to separate storage for correction by a human operator.

106. You need to process a stream of sensor data, aggregating values over one minute windows and storing the results in a data lake. Which service should you use?

    - [ ] Azure SQL Database
    - [ ] Azure Cosmos DB
    - [x] Azure Stream Analytics

107. You want to aggregate event data by contiguous, fixed-length, non-overlapping temporal intervals. What kind of window should you use? 

    - [ ] Sliding
    - [ ] Session
    - [x] Tumbling

108. Which type of output should you use to ingest the results of an Azure Stream Analytics job into a dedicated SQL pool table in Azure Synapse Analytics? 

    - [x] Azure Synapse Analytics
    - [ ] Blob storage/ADLS Gen2
    - [ ] Azure Event Hubs

109. Which type of output should be used to ingest the results of an Azure Stream Analytics job into files in a data lake for analysis in Azure Synapse Analytics? 

    - [ ] Azure Synapse Analytics
    - [x] Blob storage/ADLS Gen2
    - [ ] Azure Event Hubs

110. Which type of Azure Stream Analytics output should you use to support real-time visualizations in Microsoft Power BI? 

    - [ ] Azure Synapse Analytics
    - [ ] Azure Event Hubs
    - [x] Power BI

111. You want to use an output to write the results of a Stream Analytics query to a table named device-events in a dataset named realtime-data in a Power BI workspace named analytics workspace. What should you do?

    - [x] Create only the workspace. The dataset and table will be created automatically.
    - [ ] Create the workspace and dataset. The table will be created automatically.
    - [ ] Create the workspace, dataset, and table before creating the output.

112. You want to create a visualization that updates dynamically based on a table in a streaming dataset in Power BI. What should you do? 

    - [ ] Create a report from the dataset.
    - [x] Create a dashboard with a tile based on the streaming dataset.
    - [ ] Export the streaming dataset to Excel and create a report from the Excel workbook.

113. What does Microsoft Purview do with the data it discovers from your registered sources? 

    - [x] It catalogs and classifies the data that's scanned.
    - [ ] It moves the data to your Azure subscription, automatically creating the necessary storage accounts.
    - [ ] It performs data transformations to match your on-premises schemas.

114. Where would you register your data sources for use in Microsoft Purview? 

    - [ ] On the Overview tab of the Microsoft Purview account page.
    - [ ] On the Managed Resources tab of the Microsoft Purview account page.
    - [x] In the Microsoft Purview governance portal.

115. What aspect of Microsoft Purview is used to configure the data discovery for your data sources? 

    - [x] Scan rules
    - [ ] Collections
    - [ ] Classifications

116. You want to scan data assets in a dedicated SQL pool in your Azure Synapse Analytics workspace. What kind of source should you register in Microsoft Purview?  

    - [x] Azure Synapse Analytics.
    - [ ] Azure Data Lake Storage Gen2
    - [ ] Azure SQL Database

117. You want to scan data assets in the default data lake used by your Azure Synapse Analytics workspace. What kind of source should you register in Microsoft Purview? 

    - [ ] Azure Synapse Analytics.
    - [x] Azure Data Lake Storage Gen2
    - [ ] Azure Cosmos DB

118. You want data analysts using Synapse Studio to be able to find data assets that are registered in a Microsoft Purview collection. What should you do? 

    - [ ] Register an Azure Synapse Analytics source in the Purview account
    - [ ] Add a Data Explorer pool to the Synapse Workspace
    - [x] Connect the Purview account to the Synapse analytics workspace

119. Which of the following pipeline activities records data lineage data in a connected Purview account? 

    - [ ] Get Metadata
    - [x] Copy Data
    - [ ] Lookup

120. You plan to create an Azure Databricks workspace and use it to work with a SQL Warehouse. Which of the following pricing tiers can you select?   

    - [ ] Enterprise
    - [ ] Standard
    - [x] Premium

121. You've created an Azure Databricks workspace in which you plan to use code to process data files. What must you create in the workspace? 

    - [ ] A SQL Warehouse
    - [x] A Spark cluster
    - [ ] A Windows Server virtual machine

122. You want to use Python code to interactively explore data in a text file that you've uploaded to your Azure Databricks workspace. What should you create? 

    - [ ] A SQL query
    - [ ] An Azure function
    - [x] A Notebook

123. Which definition best describes Apache Spark? 

    - [ ] A highly scalable relational database management system.
    - [ ] A virtual server with a Python runtime.
    - [x] A distributed platform for parallel data processing using multiple languages.

124. You need to use Spark to analyze data in a parquet file. What should you do? 

    - [x] Load the parquet file into a dataframe.
    - [ ] Import the data into a table in a serverless SQL pool.
    - [ ] Convert the data to CSV format.

125. You want to write code in a notebook cell that uses a SQL query to retrieve data from a view in the Spark catalog. Which magic should you use? 

    - [ ] %%spark
    - [ ] %%pyspark
    - [x] %%sql

126. You want to connect to an Azure Databricks workspace from Azure Data Factory. What must you define in Azure Data Factory?

    - [ ] A global parameter
    - [x] A linked service
    - [ ] A customer managed key

127. You need to run a notebook in the Azure Databricks workspace referenced by a linked service. What type of activity should you add to a pipeline? 

    - [x] Notebook
    - [ ] Python
    - [ ] Jar

128. You need to use a parameter in a notebook. Which library should you use to define parameters with default values and get parameter values that are passed to the notebook? 
    
    - [ ] notebook
    - [ ] argparse
    - [x] dbutils.widget