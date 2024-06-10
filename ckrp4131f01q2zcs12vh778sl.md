---
title: "Use Stored Procedures with Temporary Tables in SSIS"
datePublished: Thu Aug 06 2020 16:03:33 GMT+0000 (Coordinated Universal Time)
cuid: ckrp4131f01q2zcs12vh778sl
slug: use-stored-procedures-with-temporary-tables-in-ssis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718039188840/c3dab437-e6b8-4dbf-9b8a-996c902512f1.png
tags: sql-server, ssis, sql-server-integration-services

---

**PROBLEM**: if you try to execute with a SSIS Data Flow a stored procedure containing a temporary table, you receive this error message:

*Exception from HRESULT: 0xC020204A Error at From SQL Server to Excel \[OLE DB Source \[91\]\]: SSIS Error Code DTS\_E\_OLEDBERROR. An OLE DB error has occurred. Error code: 0x80004005. An OLE DB record is available. Source: "Microsoft SQL Server Native Client 11.0" Hresult: 0x80004005 Description: "The metadata could not be determined because statement 'INSERT INTO #TempTable \[...\]".  
Error at From SQL Server to Excel \[OLE DB Source \[91\]\]: Unable to retrieve column information from the data source. Make sure your target table in the database is available.*

**SOLUTION**: It's incredible that on the web, the answer to this problem is always "SSIS doesn't get along with temporary tables, you have to use table variables or CTEs".  
You can use temp table, you only have to specify to SSIS the resulting dataset of your stored procedure with the WITH RESULT SETS command:

```plaintext
    EXECUTE YourSP
    WITH RESULT SETS
    ((    
        One bigint NOT NULL, 
        Two varchar(35) NULL, 
        Three char(6) NULL
    ))
```

That's all.