## T-SQL: create a complete table from ranges

Original tables with no chances of JOIN  

Tab_A: potential order numbers with types

| ID | OrderType | fromNum | toNum |  
| --- | --- | --- | --- |  
| 1 | type_1 | 1001 | 2000 |  
| 2 | type_2 | 2001 | 8000 |  
| ... | ... | ... | ... |  

Tab_B: completed orders

| OrderNum | Customer |
| --- | --- |
| 1507 | Customer_A |
| 6801 | Customer_B |
| ... | ... |

Tab_A doesn't contains every row for each order number, but each row is a range of order numbers.  
I need to create a third table containing every order number inside each range.  


```
--start from the first order type
DECLARE @id AS bigint = (SELECT min(ID) FROM Tab_A) 

--start from range beginning
DECLARE @fromNum as bigint = (SELECT fromNum FROM Tab_A WHERE id=@id) 

--output temporary table
CREATE TABLE #temp
(
OrderNum bigint,
OrderType varchar(255),
)

--insert until the last ID
WHILE (SELECT max(ID) FROM Tab_A)>@id
BEGIN

--insert until the last order number
WHILE (SELECT a_num-@da_num FROM Tabella_A where id=@id)>-1
BEGIN
INSERT INTO #temp
     SELECT @fromNum as OrderNum, OrderType 
     FROM Tab_A
     WHERE id = @id
 SET @fromNum += 1     --next order number
END
SET @id += 1     --next type
END
```

#temp now contains all the potential order numbers with the type, and can be joined with Tab_B  

