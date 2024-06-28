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

    - [ ] ``HoppingWindow``
    - [ ] ``SessionWindow``
    - [ ] "SlidingWindow"
    - [x] ``TumblingWindow``

         - Tumbling windows have a defined period and can aggregate all events for that same time period. A tumbling window is essentially a specific case of a hopping window where the time period and the event aggregation period are the same.

         - Hopping windows have a defined period and can aggregate the events for a potentially different time period

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

         - Increasing the SUs to 12 still uses two nodes. The other options still use a single node that will stop for maintenance.


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

