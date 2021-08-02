## Insert all year days in a single column on SQL Server


```
--DB1 is the database, Tab1 is the table, Date1 is the date field.
--I want to insert all 2015 dates, from 1/1 to 31/12.

DECLARE @data date='2015-01-01'
WHILE @data < '2016-01-01'
    BEGIN
        INSERT INTO DB.dbo.Tab1 (Date1)
        VALUES (@data)
        SET @data=DATEADD(day,1,@data)
    END
GO
``` 