## T-SQL: add working days to a date

Add only working date to a function, skip saturdays and sundays.  

```
CREATE FUNCTION dbo.AddWorkingDays(@Data AS DATE, @Giorni AS INT)

RETURNS DATETIME

AS

BEGIN

    DECLARE @DataFinale as DATETIME, @contatore as int
    SET @contatore=0
    
    --add one day at the time until we reach the number we want
    WHILE @contatore < @Giorni
    
    BEGIN
       SET @Data=DATEADD(d,1,@Data)
       
       --if saturday or sunday add another day without update the counter
       IF DATEPART(dw,@Data)= 7 SET @Data=DATEADD(d,1,@Data)
       IF DATEPART(dw,@Data)= 1 SET @Data=DATEADD(d,1,@Data)
       
       SET @contatore=@contatore+1  
    END
    
    SET @DataFinale = @Data
  
    RETURN @DataFinale

END

GO
``` 

You can use it instead the DATEADD to add only working days:

``` 
SELECT Db.dbo.AddWorkingDays(GETDATE(), 26)
``` 

To skip also holiday outside saturday and sundays, you have to add a custom table with this dates and include it in the function.