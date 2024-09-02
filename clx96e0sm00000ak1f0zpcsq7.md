---
title: "Temporal Tables in SQL Server and Azure SQL Database"
datePublished: Thu Jan 06 2022 23:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clx96e0sm00000ak1f0zpcsq7
slug: temporal-tables-in-sql-server-and-azure-sql-database
tags: sql-server, azure, sql, t-sql

---

Temporal table: to provide information about data in a table at any point in time.  
On SQL Server from the 2016 version.

```sql
-- This statement creates a Temporal Table
CREATE TABLE dbo.ExampleTemporal
(
	Id INT NOT NULL PRIMARY KEY CLUSTERED,
	Col1 INT,
	Col2 varchar(20),
	SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL,
	SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL,
	PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)  --Specifies which are the period column 
)
WITH
	(
		SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ExampleHistory)  --Table used to store the history
	)
```

This is what you see on SSMS after the execution of the script:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659967839489/4PbKewRWM.png align="left")

As you can see, temporal tables are made of two tables:

1. the temporal table that keeps the current data just like your normal tables, with two additional fields: the period columns that represent the period of validity of the data for each row.
    
2. the history table which keeps the previous values of the data, with an entry per each change. Each change have a period of validity that can be used to retrieve value of a row for a period. Changes are stored automatically.
    

What happens to the two tables when you made an INSERT/UPDATE/DELETE?

* When you **INSERT** a new row on a temporal table, the period of validity is set from the current time to the date 9999-12-31.
    
* When you **UPDATE** a row, the previous row was stored in the history table (with the validity that starts with the time the record is created or updated previously and end now) and the data is updated in the temporal table (with the period of validity valid from now to 9999-12-31).
    
* When you **DELETE** a row, the row is removed from the temporal table and stored in the history table with the correspondent period of validity which ends in the moment when the record was deleted.
    

```sql
--Insert an example
INSERT INTO dbo.ExampleTemporal (Id, Col1, Col2)
VALUES (1, 1, 'a')

--Update the example
UPDATE dbo.ExampleTemporal
SET Col1 = 2
WHERE Id = 1

--See only the current record
SELECT * 
FROM dbo.ExampleTemporal

--Query the history table with all the old values
SELECT * 
FROM dbo.ExampleHistory
WHERE Id = 1
```

Selecting the data a from temporal table is no different from selecting data in a normal table.  
But you can use a few keywords in addition to the normal select statement to interact with the period of the data.  
This keywords are:

* **FOR SYSTEM\_TIME AS OF**: used with a specific datetime -&gt; AS OF *datetime*
    
* **FOR SYSTEM\_TIME FROM TO**: used with ranges of datetime -&gt; FROM *start* TO *end*
    
* **FOR SYSTEM\_TIME BETWEEN**: used with ranges of datetime -&gt; BETWEEN *start* AND *end*
    
* **FOR SYSTEM\_TIME CONTAINED IN**: used with ranges of datetime -&gt; CONTAINED IN (*start*, *end*)
    
* **FOR SYSTEM\_TIME ALL**: all changes made to a record
    

```sql
--See only the current record
SELECT * 
FROM dbo.ExampleTemporal

--See the record in a past state (different methods)
SELECT * 
FROM dbo.ExampleTemporal
FOR SYSTEM_TIME AS OF '2022-08-08 16:22'
WHERE Id = 1

SELECT * 
FROM dbo.ExampleTemporal
FOR SYSTEM_TIME BETWEEN '2022-08-05 16:22' AND '2022-08-09'
WHERE Id = 1

SELECT * 
FROM dbo.ExampleTemporal
FOR SYSTEM_TIME FROM '2022-08-05 16:22' TO '2022-08-09'
WHERE Id = 1

--See the current and all the past versions of the record
SELECT * 
FROM dbo.ExampleTemporal
FOR SYSTEM_TIME ALL
WHERE Id = 1
ORDER BY SysEndTime
```

With a temporal table, you can restore data back to a previous state using the FOR\_SYSTEM\_TIME AS OF clause. It restores both the updated and deleted records. You can use it for the entire table or only for part of it using the WHERE clause.

```sql
--Delete the record
DELETE FROM dbo.ExampleTemporal
WHERE Id = 1

--The record is gone
SELECT * FROM dbo.ExampleTemporal WHERE Id = 1

--Insert again the record from a past version of it
INSERT INTO dbo.ExampleTemporal (Id, Col1, Col2)
SELECT Id, Col1, Col2
FROM dbo.ExampleTemporal FOR SYSTEM_TIME AS OF '2022-08-08 16:30'
WHERE Id = 1
```