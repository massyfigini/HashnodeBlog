# Connect Azure DevOps data to Power BI

*The following are some notes about the connection from Power BI to Azure DevOps data.*

### **1\. Create the token from Azure DevOps**

To read your Azure DevOps data from Power BI you can create a Token from the DevOps user settings page -&gt; Personal Access Tokens

![Picture1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662644505683/28RQoXPgw.png align="left")

Choose New Token, then give it a name, choose the expiration date (max one year) and the scopes: you could grant the full access, but generally the Analytics Read scope it's enough.

![Picture2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662644585596/84Am3sIFh.png align="left")

Copy the token created.

### 2\. oData Queries

Most of the time you can use odata query to filter and connect to DevOps data.  
The simplest way to write an odata query is using the odata extension for Visual Studio Code. In the Marketplace download and install the vscode-odata extension, then create a new file .odata.

The following is a odata query to take some work items data, you can copy it to your file and personalize it with your organization, project and fields (also you can delete the part you won't use, like orderby or top):

```plaintext
https://analytics.dev.azure.com/{organization}/{project}/_odata/v3.0-preview/WorkItems?
    $select=WorkItemId,Title,WorkItemType,State,CreatedDate,OriginalEstimate,Area
    &$filter=startswith(IterationPath,'IT00') and WorkItemType eq 'Task'
    &$orderby=CreatedDate desc
    &$top=10
    &$expand=Iteration($select=IterationPath,StartDate,EndDate)
```

In the $select part choose the field you want, the $filter is the "where" part, here we are taking only the tasks of the IterationPath that start with "Project00". Also we are expanding the iteration, taking the path, start date and end date.  
To query across projects, omit /{project} in the first row of the query.

After you have written your OData query, place your cursor anywhere in the query text and select View &gt; Command Palette (or ctrl+shift+p). In the search box, type odata to bring up all the OData commands and select OData: Combine. This action combines the multiline query into a one-line one.

### 3\. Connect data to Power BI

Now you can run it from Power BI. Open Power BI, select Get Data, and then select the OData feed connector.

![Picture3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662646479898/fHbj6VSxx.png align="left")

Paste your query and choose OK.

![Picture4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662646657933/SAufLFlcW.png align="left")

To use the token created earlier, choose the Basic authentication and paste the token in the password space. You could leave blank the user name space and save.

**NB**: if you publish the report to PBI Service, you have to insert the credential again in the Service. You can't leave the User space blank, but you can write anything you want.

![Picture5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662647278979/s4gme7P-X.png align="left")

After the data has been imported, you will notice that you need to expand the part you have insert in the $expand part of the query.

### 4\. Other connections

There are some part of the Azure DevOps data that aren't accessible with odata in this moment.  
For example some data in the single iteration, like users capacities and users/teams days off.  
For this type of data you can use the web source, using the same type of connection (basic with the token in the password space).

![Picture7.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662649629334/svcmjfYGv.png align="left")

Here are the query for the iteration user capacities and days off, replace {organization}, {project} and {IterationID} with your data:

```plaintext
https://dev.azure.com/{organization}/{project}/_apis/work/teamsettings/iterations/{IterationID}/capacities?api-version=6.0
```

If you have a table with a list of Iterations ID and you want to extract all the capacities and days off for each iteration, you can use a Power Query custom function. To do this you can add a new step and use the following code (replace {previous step}, {organization}, {project} and {Iteration ID field}):

```plaintext
= Table.AddColumn(#"{previous step}", "Custom", each Json.Document(Web.Contents("https://dev.azure.com/{organization}/{project}/_apis/work/teamsettings/iterations/", [RelativePath= [{Iteration ID field}] & "/capacities?api-version=6.0"])))
```

Other data not accessible with odata are data from extensions, like the Time Log extension.  
To connect to these kind of data, create a blank query, right click on it, choose "Advanced Editor", and write this code (replace {organization}):

```plaintext
let
    Source = VSTS.Contents("https://extmgmt.dev.azure.com/consulcesidevteams/_apis/ExtensionManagement/InstalledExtensions/TimeLog/time-logging-extension/Data/Scopes/Default/Current/Collections/TimeLogData/Documents"),
    #"Imported JSON" = Table.FromRecords(Json.Document(Source,65001)[value])
in
    #"Imported JSON"
```

You will have all your Time Log data using the token to connect to it in the usual mode.

**NB**: To use the token to read data from extensions like Time Log, if you had not give the full access to the token, you have to add the Extensions read grant to the token in Azure DevOps.