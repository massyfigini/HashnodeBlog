## Insert all year days in a single column on SQL Server


```
--Tab1 is the table, Date1 is the date field.
--I want to insert all the 2015 dates, from 1/1 to 31/12.

DECLARE @data date='2015-01-01'
WHILE @data < '2016-01-01'
    BEGIN
        INSERT INTO dbo.Tab1 (Date1)
        VALUES (@data)
        SET @data=DATEADD(day,1,@data)
    END
GO
``` 