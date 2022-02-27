## XML documents in SQL Server and Azure SQL Database

### 1. XML documents in SQL Server and Azure SQL Database

While JSON uses nvarchar type, XML has a dedicated column type of the same name in SQL Server and Azure SQL Database.

```
-- Create table and insert data for the examples
CREATE TABLE [dbo].[XMLexample](
	[id] [int] IDENTITY(1,1) NOT NULL,
	[ColumnXML] [xml] NULL
)

INSERT INTO [dbo].[XMLexample]
           ([ColumnXML])
     VALUES
(
'
<breakfast_menu>
  <food>
    <name>Belgian Waffles</name>
    <price>$5.95</price>
    <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
    <calories>650</calories>
  </food>
  <food>
    <name>Strawberry Belgian Waffles</name>
    <price>$7.95</price>
    <description>Light Belgian waffles covered with strawberries and whipped cream</description>
    <calories>900</calories>
  </food>
  <food>
    <name>Berry-Berry Belgian Waffles</name>
    <price>$8.95</price>
    <description>Light Belgian waffles covered with an assortment of fresh berries and whipped cream</description>
    <calories>900</calories>
  </food>
  <food>
    <name>French Toast</name>
    <price>$4.50</price>
    <description>Thick slices made from our homemade sourdough bread</description>
    <calories>600</calories>
  </food>
  <food>
    <name>Homestyle Breakfast</name>
    <price>$6.95</price>
    <description>Two eggs, bacon or sausage, toast, and our ever-popular hash browns</description>
    <calories>950</calories>
  </food>
</breakfast_menu>
'
)
``` 

In SQL Server Management Studio if you load an XML field, the cell is cliccable, if you click on them a new window opens with the XML content.


### 2. Selecting Data from XML

One method to query XML data is using the stored procedure sp_xml_preparedocument to load the xml, and then the OPENXML function to select parts of that.

```
DECLARE @doc int;
DECLARE @xmldoc xml = (SELECT ColumnXML FROM [dbo].[XMLexample] WHERE Id = 1)

EXEC sp_xml_preparedocument @doc OUTPUT, @xmldoc 

SELECT * FROM
OPENXML(@doc, '/breakfast_menu/food')
WITH (
	  name varchar(255) 'name',
	  price varchar(255) 'price',
	  description varchar(255) 'description',
	  calories varchar(255) 'calories'
)
```

You can go more in depth with the syntax of OPENXML [here](https://docs.microsoft.com/en-us/sql/t-sql/functions/openxml-transact-sql?view=sql-server-ver15).

**NB**: 
- OPENXML function can't work directly with an XML text, it needs the sp_xml_preparedocument stored procedure.
- The sp_xml_preparedocument loads the entire document in memory so watch out for large memory consumption if the queried document is big.

Another way of selecting data from an xml is using the xml methods: the xml data type provide multiple methods that help you work with xml data that is stored in a variable or column of xml type.
This is a more scalable way of working with xml data as it does not require a stored procedure that loads the entire document into memory.
These methods receive as a parameter an XQuery expression to identify which xml components should be retrieved or modified.
There are five major methods: query(), value(), nodes(), modify(), and exist().
The value() method extracts a scalar value from an xml field, useful in particular to compare xml data with other columns:  
*value(XQuery, datatype)*  
The query() method extracts an xml value from an xml field:  
*query(XQuery)*  

```
SELECT ColumnXML.value('(/breakfast_menu/food/name)[2]', 'varchar(255)') as FoodName,
		ColumnXML.value('(/breakfast_menu/food/calories)[2]', 'int') as Calories
FROM dbo.XMLexample

SELECT ColumnXML.query('(/breakfast_menu/food/name)') as FoodName
FROM dbo.XMLexample
```

You could read the XQuery language reference [here](https://docs.microsoft.com/en-us/sql/xquery/xquery-expressions?view=sql-server-ver15) to learn more on the XQuery language you can use in SQL Server.

The nodes() method shred an xml data type into relational data to identify nodes that will be mapped into rows. It will return a table with one column which contains a logical copy of the xml that's based on the query expression provided.
The combination of nodes and value uses xml indexes effectively, which means it's more scalable than using OPENXML.

```
DECLARE @xml xml = (SELECT ColumnXML FROM dbo.XMLexample)

SELECT doc.col.value('name[1]', 'varchar(255)') as FoodName,
		doc.col.value('calories[1]', 'int') as Calories
FROM @xml.nodes('/breakfast_menu/food') doc(col)
```


### 3. Updating XML data

We can update an xml document by using the modify() method within a SET statement and the XQuery DML language, that has three case-sensitive keywords: insert, delete and replace value of.

```
-- replace the price of the french toast
UPDATE dbo.XMLexample
SET ColumnXML.modify('
replace value of (/breakfast_menu/food/price/text())[4]
with "$5.00"
')

-- delete the description tag of the Strawberry Belgian Waffles
UPDATE dbo.XMLexample
SET ColumnXML.modify('
delete (/breakfast_menu/food/description)[2]
')

-- insert a description tag for the Strawberry Belgian Waffles
UPDATE dbo.XMLexample
SET ColumnXML.modify('
insert <description>Light Belgian waffles covered with strawberries and whipped cream</description>   
after (/breakfast_menu/food/price)[2] 
')

-- insert a special tag at the end of the Homestyle Breakfast
UPDATE dbo.XMLexample
SET ColumnXML.modify('
insert <special>True</special>   
as last into (/breakfast_menu/food)[5] 
')
```


### 4. Filtering XML data

To filter data based on values that are stored in your XML field, you could use value() to return a scalar and do a comparison or you could use an operator in your XQuery expression, or in alternative you could use the exist() methods.
The result of the exist() method is a boolean value.

```
-- extract only the table records which have a description tag in the xml
SELECT *
FROM dbo.XMLexample
WHERE ColumnXML.exist('//description') = 1
```