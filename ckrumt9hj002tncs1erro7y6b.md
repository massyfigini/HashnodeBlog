---
title: "T-SQL: Function to create a datetime"
datePublished: Fri Sep 30 2016 12:47:56 GMT+0000 (Coordinated Universal Time)
cuid: ckrumt9hj002tncs1erro7y6b
slug: t-sql-function-to-create-a-datetime
tags: sql-server, sql

---

DATEFROMPARTS() function is from SQL Server version 2012 and later, working on SQL Server 2008 I had to create a function to create a datetime from parts.

```plaintext
CREATE FUNCTION dbo.BuildDate(@Y AS int, @MO AS int, @D as int, @H as int = 0, @MI as int = 0, @S as int = 0)

RETURNS DATETIME

AS
BEGIN
    
    DECLARE @Data as DATETIME
    DECLARE @Y_N AS char(5), @MO_N AS char(3), @D_N as char(3), @H_N as char(3), @MI_N as char(3), @S_N as char(2)
    SET @Data=NULL
    
    SET @Y_N = convert(char(4),@Y)+'-'
    SET @MO_N = convert(char(2),@MO)+'-'
    SET @D_N = convert(char(2),@D)+' '
    SET @H_N = convert(char(2),@H)+':'
    SET @MI_N = convert(char(2),@MI)+':'
    SET @S_N = convert(char(2),@S)
       
    SET @Data = CONVERT(datetime, ''+@Y_N+@MO_N+@D_N+@H_N+@MI_N+@S_N+'')
  
    RETURN @Data
       
END
```

Let's use it:

```plaintext
SELECT dbo.BuildDate(2016,9,30,12,43, default)
```