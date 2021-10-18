## PySpark Fundamentals

Spark = tool for doing parallel computation with large datasets. Spark lets you spread data and computations over clusters with multiple nodes.  
pyspark = Python package that integrate Spark with Python.  

The SparkSession.builder.getOrCreate() method returns an existing SparkSession if there's already one in the environment, or creates a new one if necessary.  


```
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
my_spark = SparkSession.builder.getOrCreate()

# Print my_spark
print(my_spark)

# Print the tables in the catalog
print(spark.catalog.listTables())

``` 

One of the advantages of the DataFrame interface is that you can run SQL queries on the tables in your Spark cluster.  
The .sql() method takes a string containing the query and returns a DataFrame with the results.  
The .toPandas() method on a Spark DataFrame returns the corresponding pandas DataFrame.  

```
# query
query = "FROM table SELECT * LIMIT 10"

# Get the first 10 rows of flights
top10 = spark.sql(query)

# Convert the results to a pandas DataFrame
pd_top10 = top10 .toPandas()

# Print the head of pd_counts
print(pd_top10.head())
``` 

The .createDataFrame() method takes a pandas DataFrame and returns a Spark DataFrame. The output of this method is stored locally, not in the SparkSession catalog. This means that you can use all the Spark DataFrame methods on it, but you can't access the data in other contexts. For example, a SQL query (using the .sql() method) that references your DataFrame will throw an error. To access the data in this way, you have to save it as a temporary table. You can do this using the .createTempView() Spark DataFrame method. There is also the method .createOrReplaceTempView(). This safely creates a new temporary table if nothing was there before, or updates an existing table if one was already defined.  


```
# Create pd_temp
pd_temp = pd.DataFrame(np.random.random(10))

# Create spark_temp from pd_temp
spark_temp = spark.createDataFrame(pd_temp)

# Add spark_temp to the catalog
spark_temp.createOrReplaceTempView("temp")

# Examine the tables in the catalog
print(spark.catalog.listTables())
``` 

The .read attribute has several methods for reading different data sources into Spark DataFrames. Using these you can create a DataFrame from a .csv file just like with regular pandas DataFrames.  
The .withColumn() method takes two arguments: a string with the name of your new column, and the new column itself that must be an object of class Column.  
All these methods return a new DataFrame, so you have to overwrite the original DataFrame.  


```
#file path
file_path = "/user/local/datasets/table.csv"

#Read the data
df = spark.read.csv(file_path, header=True)

#Show the data
df.show()

#Add new field
flights = df.withColumn("duration_hrs", df.duration_hrs/60)
``` 

The .filter() method is the Spark counterpart of SQL's WHERE clause and takes either an expression that would follow the WHERE clause of a SQL expression as a string, or a Spark Column of boolean (True/False) values.  

```
#Filter by passing a string
df1 = df.filter("col > 1000")

#Filter by passing a column of boolean values
df2 = df.filter(df.col > 1000)
``` 


The Spark variant of SQL's SELECT is the .select() method. This method takes multiple arguments - one for each column you want to select. These arguments can either be the column name as a string (one for each column) or a column object (using the df.colName syntax). You can also use the .alias() method to rename a column you're selecting. 

```
# by passing a string
df1 = df.select("col1", "col2", "col3")

# df.colName syntax
df2 = df.select(df.col1, df.col2, df.col3)
``` 

Now the .groupBy() DataFrame method, to aggregate. You can use count(), min(), max(), avg(), sum(), ecc. with it.  

```
df.groupBy().min("col").show()
``` 

In PySpark, joins are performed using the DataFrame method .join(). This method takes three arguments. The first is the second DataFrame that you want to join with the first one. The second argument, on, is the name of the key column(s) as a string.  The third argument, how, specifies the kind of join to perform.


```
dfjoin = df1.join(df2, on="col1", how="leftouter")
``` 

The .cast() method is to convert data types.  

```
df = df.withColumn("col", df.col.cast("integer"))
``` 











