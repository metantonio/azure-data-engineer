# Run Azure Databricks Notebooks with Azure Data Factory

Using pipelines in Azure Data Factory to run notebooks in Azure Databricks enables you to automate data engineering processes at cloud scale.

## Learning objectives

In this module, you'll learn how to:

 - Describe how Azure Databricks notebooks can be run in a pipeline.
 - Create an Azure Data Factory linked service for Azure Databricks.
 - Use a Notebook activity in a pipeline.
 - Pass parameters to a notebook.

## Create a linked service for Azure Databricks

***To run notebooks in an Azure Databricks workspace, the Azure Data Factory pipeline must be able to connect to the workspace***; which requires authentication. To enable this authenticated connection, you must perform two configuration tasks:

 1. Generate an access token for your Azure Databricks workspace.
 2. Create a linked service in your Azure Data Factory resource that uses the access token to connect to Azure Databricks.

### Generating an access token

An access token provides an authentication method for Azure Databricks as an alternative to credentials on the form of a user name and password. You can generate access tokens for applications, specifying an expiration period after which the token must be regenerated and updated in the client applications.

To create an Access token, use the **Generate new token** option on the **Developer** tab of the **User Settings** page in Azure Databricks portal.

<a href="#">
    <img src="./img/access-token.png" />
</a>

### Creating a linked service

To connect to Azure Databricks from Azure Data Factory, ***you need to create a linked service for Azure Databricks compute***. You can create a linked service in the Linked services page in the Manage section of Azure Data Factory Studio.

<a href="#">
    <img src="./img/linked-service.png" />
</a>

When you create an Azure Databricks linked service, you must specify the following configuration settings:

Setting	 | Description
---	 | ---
Name	 | A unique name for the linked service
Description | 	A meaningful description
Integration runtime | 	The integration runtime used to run activities in this linked service. See Integration runtime in Azure Data Factory for more details.
Azure subscription	 | The Azure subscription in which Azure Databricks is provisioned
Databricks workspace	 | The Azure Databricks workspace
Cluster	 | The Spark cluster on which activity code will be run. You can have Azure Databricks dynamically provision a job cluster on-demand or you can specify an existing cluster in the workspace.
Authentication type	 | How the linked connection will be authenticated by Azure Databricks. For example, using an access token (in which case, you need to specify the access token you generated for your workspace).
Cluster configuration | 	The Databricks runtime version, Python version, worker node type, and number of worker nodes for your cluster.

## Use a Notebook activity in a pipeline

After you've created a linked service in Azure Data Factory for your Azure Databricks workspace, you can use it to define the connection for a **Notebook** activity in a pipeline.

To use a **Notebook** ***activity***, create a pipeline and from the **Databricks** category, add a **Notebook** activity to the pipeline designer surface.


<a href="#">
    <img src="./img/notebook-activity.png" />
</a>

Use the following properties of the **Notebook** activity to configure it:

Category |	Setting |	Descriptions
--- |	--- |	---
General	| Name	| A unique name for the activity.
`` | Description	| A meaningful description.
`` | Timeout	| How long the activity should run before automatically canceling.
`` | Retries	| How many times should Azure Data Factory try before failing.
`` | Retry interval	| How long to wait before retrying.
`` | Secure input and output |	Determines if input and output values are logged.
Azure Databricks |	Azure Databricks linked service	| The linked service for the Azure Databricks workspace containing the notebook.
Settings	| Notebook path |	The path to the notebook file in the Workspace.
`` | Base parameters	| Used to pass parameters to the notebook.
`` | Append libraries	| Required code libraries that aren't installed by default.
User properties	| `` | 	Custom user-defined properties.

### Running a pipeline

When the pipeline containing the **Notebook** activity is published, you can run it by defining a ***trigger***. You can then monitor pipeline runs in the **Monitor** section of Azure Data Factory Studio.