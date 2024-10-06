---
title: "Intro to Spark SQL in Python"
datePublished: Sun Nov 21 2021 08:37:28 GMT+0000 (Coordinated Universal Time)
cuid: ckw8zlrt10f4xiks11f0o1rnc
slug: intro-to-spark-sql-in-python
tags: python, spark, sql

---

Spark SQL is a component of Apache Spark that works with tabular data.

```python
# Load data from file
df = spark.read.csv("trains.csv", header=True)

# Create temporary table called table1
df.createOrReplaceTempView("trains")

# Inspect the columns in the table table1
spark.sql("DESCRIBE trains").show()

# Query table1
spark.sql("SELECT * FROM trains WHERE id = 1").show()

# We could also use advanced sql like window functions
query = """
SELECT train_id, station, time, diff_time,
SUM(diff_time) OVER (PARTITION BY train_id ORDER BY time) AS running_total
FROM trains
"""
# Run the query and display the result
spark.sql(query).show()

# There is tipically a dot notation equivalent of every SQL clause including window functions
# Example 1
spark.sql('SELECT train_id, MIN(time) AS start FROM trains GROUP BY train_id').show()
# equivalent to:
df.groupBy('train_id').agg({'time':'min'}).withColumnRenamed('MIN(time)', 'start').show()

# Example 2
df = spark.sql("""
SELECT *, 
LEAD(time,1) OVER(PARTITION BY train_id ORDER BY time) AS time_next 
FROM trains
""")
# equivalent to:
from pyspark.sql import Window
from pyspark.sql.functions import lead
dot_df = df.withColumn('time_next', lead('time', 1)
        .over(Window.partitionBy('train_id')
        .orderBy('time')))
```

Create a User Defined Function:

```python
# create a udf that returns true if a column is less then 5 characters long

from pyspark.sql.functions import udf
from pyspark.sql.types import BooleanType

udf_short = udf(lambda x:
                     True if not x or len(x) < 5 else False,
                     BooleanType())


# we use the udf just as we would a built-in function
df.select(udf_short('data').alias('is_short').show(10)
```