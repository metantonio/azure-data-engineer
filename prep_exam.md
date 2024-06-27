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

        - Derived column: This task is used to create new columns or modify existing columns based on expressions or conditions. You can use it to parse and transform the fixed-length fields into individual columns.
        - Select: This task allows you to choose which columns to include or exclude from the data flow. You will use it to select only the three columns that you want to write to the CSV file.
        - Sink: This task is used to write the output of the data flow to a destination, such as a CSV file.