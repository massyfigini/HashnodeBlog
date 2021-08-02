## Connect Power BI to an Azure Table Storage

In the Azure Portal, select your Storage Account, go to "Tables", and write down the Table's URL.  

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627481014775/3jqHuCFeA.png)  

Then, in the same page go to "Access keys", then click "Show keys" and copy the key.  

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627481643787/5JwNsf_KQ.png)  

Open Power BI Desktop, click "Get data" -> "More..."  

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627483074813/kLlnHf0zHX.png)  

then "Azure" -> "Azure Table Storage" and "Connect"   

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627483207272/JQPTMUVzi.png)  

In the form add the Link without the last part (table name). So in the example URL above it's not 
 [https://massy.table.core.windows.net/Test]() , but  [https://massy.table.core.windows.net/](). Click "OK".  
Then add the key and click "Connect".  
You will see your storage account tables and choose one or more of them, then click "Load" or "Transform Data" (probably you will choose transform to expand all the columns that by default won't be loaded).

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627484077942/0lOA_nrqw.png)  


