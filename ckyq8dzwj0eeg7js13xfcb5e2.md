---
title: "JSON documents in SQL Server and Azure SQL Database"
datePublished: Sat Jan 22 2022 19:34:51 GMT+0000 (Coordinated Universal Time)
cuid: ckyq8dzwj0eeg7js13xfcb5e2
slug: json-documents-in-sql-server-and-azure-sql-database
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681377979306/a7351b3e-d3cf-4fbe-8a36-3876cc39536b.png
tags: sql-server, azure, json, sql

---

These contents working in SQL Server 2016 and later and Azure SQL Database.

### 1\. JSON documents in SQL Server

Use nvarchar(max) data type to store JSON documents up to 2 GB in size in a column of a SQL Server table. If your JSON is less then 8 KB is recommended to use nvarchar(4000) for performance reasons.

To see if a string contains a valid JSON you can use the function ISJSON(). ISJSON(column) It returns 1 if the column contains valid JSON, otherwise returns 0.

```plaintext
-- Extract only the records containing a valid JSON in the Post_Json column
SELECT *
FROM dbo.Posts
WHERE IS_JSON(Post_Json) > 0
```

You can use constraints with ISJSON() function to enforce proper formatting of your JSON column CHECK(ISJSON(column)=1).

### 2\. Query JSON with T-SQL

To query a JSON column, we could use JSON\_VALUE() and JSON\_QUERY() functions, with the JSON column and the JSON path (we start it with $ and navigate the JSON structure using dots).

JSON\_VALUE() to retrieve a single value (number or text) in a JSON column. JSON\_VALUE(jsoncolumn, '$.employee.birthyear') It returns a single text value of type nvarchar(4000).

JSON\_QUERY() to retrieve a JSON object or array from a JSON column. JSON\_QUERY(jsoncolumn, '$.employee.address') It returns a JSON fragment of type nvarchar(max).

We can use JSON\_VALUE and JSON\_QUERY both in the select and in the where part of a query.

```plaintext
-- The Column Post_Json in the table dbo.Posts contains a JSON
SELECT JSON_VALUE(Post_Json, '$.Post.Title') AS Title
	,JSON_QUERY(Post_Json, '$.Post.Author') AS Author  -- used JSON_QUERY because the result is a JSON object not only a value
	,JSON_VALUE(Post_Json, '$.Post.Comments[0].Comment.Text') AS First_Comment  -- first comment in the path
FROM dbo.Posts
WHERE JSON_VALUE(Post_Json, '$.Post.Title') = 'AWS in Python with Boto3'
```

### 3\. Format Query Results as JSON with T-SQL

Now I want to retrieve a JSON from traditional columns that contain structured data. We can use FOR JSON PATH or FOR JSON AUTO

```plaintext
-- With FOR JSON PATH we have full control over the format
SELECT *
FROM dbo.Table1
FOR JSON PATH, ROOT ('Id')

-- With FOR JSON AUTO SQL Server choose the best structure based on the select
SELECT *
FROM dbo.Table1
FOR JSON AUTO
```

### 4\. Update a JSON with T-SQL

To modify a JSON column with T-SQL, we use the function JSON\_MODIFY() with the JSON column, the JSON path and the new value. JSON\_MODIFY(jsoncolumn, '$.employee.birthyear', '1983')

```plaintext
UPDATE dbo.Posts
SET Post_Json = JSON_MODIFY(Post_Json, '$.Post.Title', 'Basic Scala')
WHERE JSON_VALUE(Post_Json, '$.Post.Id') = '61'
```

### 5\. JSON Function Summary in SQL Server

Recap:

* ISJSON(): see if a string contains a valid JSON
    
* JSON\_VALUE(): extract a single value from a JSON string
    
* JSON\_QUERY(): exrtact an object or an array from a JSON string
    
* JSON\_MODIFY(): modify a JSON column
    

### 6\. Importing and parsing JSON

You can import JSON files from disk using OPENROWSET.

```plaintext
DECLARE @JSONfile varchar(max);

SELECT @JSONfile = BulkColumn   -- BulkColumn is the return value from OPENROWSET
FROM OPENROWSET (BULK 'C:\Documents\sqlfiles\json\post.json', SINGLE_CLOB) AS j;  -- SINGLE_CLOB tells OPENROWSET to interpret the document as a single varchar(max) column

PRINT @JSONfile;
```

You can import JSON objects directly from a string using OPENJSON and parse it as a table.

```plaintext
DECLARE @JSONfile varchar(max);

SET @JSONfile = N'[
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 38
},
{
  "firstName": "Jane",
  "lastName": "Doe",
  "age": 25
}';

SELECT *
FROM OPENJSON (@JSONfile) 
WITH (
   firstName varchar(50),
   lastname varchar(50),
   age int
   ) AS People
```

### 7\. Handling missing properties with strict mode

STRICT will raise an error if the property is missing. JSON\_VALUE(jsoncolumn, 'strict$.employee.birthyear')

LAX is the opposite, not raise an error if the property is missing and it is the default. JSON\_VALUE(jsoncolumn, 'lax$.employee.birthyear') is the same of JSON\_VALUE(jsoncolumn, '$.employee.birthyear')