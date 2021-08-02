## SSIS: Excel destination error message 'cannot convert between unicode and non-unicode'

**GOAL**: move data from SQL Server to Excel with SQL Server Integration Services.  

**PROBLEM**: the package doesn't work and you receive the error "cannot convert between unicode and non-unicode".  

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627633244646/Yx65i9-MXx.png)

**SOLUTION**
1. Add a Data Conversion task between Source and Destination in your Data Flow Task.  
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627633257713/ap59eRx8x.png)
1. Inside the Data Conversion task convert all the string field to "Unicode string [DT_WSTR]":  
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627633271954/5_s3Z9o1R.png)
1. In the Mappings page of the Excel destination task, remap the fields taking the new converted columns instead of the original ones from the Available Input Columns.  
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627633281301/ATceRAYsG.png)