## T-SQL: Some notes on Identity


```
--set identity insert to insert data in an identity field
SET IDENTITY_INSERT tablename ON

--you have to use the INSERT INTO explicit form
INSERT INTO tablename (col1, col2)
VALUES (1, 100)

--add an identity integer column with values
ALTER TABLE tablename ADD col3 INT IDENTITY

--index
CREATE CLUSTERED INDEX indexname on tablename(col3)

--be primary key of the table
ALTER TABLE tablename ADD PRIMARY KEY (col3)
``` 