## T-SQL: DATEDIFF() between working dates

Function for see the difference between dates in working days.

```
CREATE FUNCTION dbo.DifferenceWorkingDays (@Data1 AS DATE, @Data2 AS DATE)

RETURNS INT

AS

BEGIN

    DECLARE @DataVar as DATETIME, @Giorni as INT
    SET @DataVar=@Data2
    SET @Giorni=0
    
    --remove a day at time until reach the day requested    
    WHILE @DataVar > @Data1
    
    BEGIN
       SET @DataVar=DATEADD(d,-1,@DataVar)
       
       --if saturday or sunday remove another day without update the counter
       IF DATEPART(dw,@DataVar)= 1 SET @DataVar=DATEADD(d,-1,@DataVar)
       IF DATEPART(dw,@DataVar)= 7 SET @DataVar=DATEADD(d,-1,@DataVar)
       
       SET @Giorni=@Giorni+1   
    END
  
    RETURN @Giorni

END

GO
``` 

You can call it instead DATEDIFF() if you need the working day difference.  
``` 
SELECT Database.dbo.DifferenceWorkingDays(GETDATE(), '2016-06-24')
``` 
To skip also holiday outside saturday and sundays, you have to add a custom table with this dates and include it in the function.  
If you want to add working days to a date, take a look [here](https://massyfigini.hashnode.dev/t-sql-add-working-days-to-a-date).