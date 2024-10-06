---
title: "Data Ingestion with pandas"
datePublished: Sat Apr 17 2021 15:32:20 GMT+0000 (Coordinated Universal Time)
cuid: ckrp3365y01drzcs1ecebdsv2
slug: data-ingestion-with-pandas
tags: data, python, pandas

---

### 1\. Importing Data from Flat Files and Spreadsheets

```sql
read_csv for all flat files
import pandas as pd
data = pd.read_csv('file.csv')
data = pd.read_csv('file.tsv', sep='\t')

#choose columns to load by name
col_names = ['STATEFIPS', 'STATE', 'zipcode', 'agi_stub', 'N1']
data = pd.read_csv('file.csv', usecols=col_names)

#choose columns to load by number
col_nums = [0, 1, 2, 3, 4]
data = pd.read_csv('file.csv', usecols=col_nums)

#limiting rows, skip rows, no header in file
data = pd.read_csv('file.csv', nrows=1000, skiprows=1000, header=None)

#assigning column names
colnames = list(another_dataframe)
data = pd.read_csv('file.csv', header=None, names=colnames)

#specifyinf data types
data = pd.read_csv('file.csv', dtype={'zipcode': str})

#customizing missing data values (0 is NaN in the example)
data = pd.read_csv('file.csv', na_values={'zipcode' : 0})

#lines with errors
#with error_bad_lines skip lines with error
#with warn_bad_lines return a warning for bad lines
data = pd.read_csv('file.csv', error_bad_lines=False, warn_bad_lines=True)

#read_excel for excel files
data = pd.read_excel('file.xlsx')   # reads only the first sheet

#many parameters in common with read_csv
#nrows
#skiprows
#usecols: you can specify a range of letters of the spreadsheet file
#in the example I import only columns from W to AB plus AR
data = pd.read_excel('file.xlsx', usecols="W:AB, AR")

#two possibilities to taking the second sheet named sheet2
data = pd.read_excel('file.xlsx', sheet_name=1)   # zero-indexed
data = pd.read_excel('file.xlsx', sheet_name='sheet2')

#import first two sheets
data = pd.read_excel('file.xlsx', sheet_name=[0,1])   
data = pd.read_excel('file.xlsx', sheet_name=['sheet1','sheet2'])

#take all the sheets of a file (one data frame per sheet)
data = pd.read_excel('file.xlsx', sheet_name=None)

#with dtype specify column types
#true_values and false_values useful for convert boolean
data = pd.read_excel('file.xlsx',
                                    dtype = {'col5': bool,
                                                   'col6': bool}
                                    true_values = ["Yes"]
                                    false_values = ["No"]})
#NB: blank values are setting as True!

#for setting datetime columns not dtypes but parse_dates
#if is a standard datetime is enough this:
data = pd.read_excel('file.xlsx',
                                    parse_dates = ['col1','col2'])

#for non standard column date, es. 311299 for 31/12/1999
format_string = '%d%m%y'
```

### 2\. Importing Data from Databases

```sql
import pandas as pd

#library to connect to databases
from sqlalchemy import create_engine

#create the database engine for the db data.db on sqlite
engine = create_engine('sqlite:///data.db')

#view the tables in the database
print(engine.table_names())

#load tab table without any SQL
tabloaded = pd.read_sql('tab', engine)

#create a SQL query to load data from a query
query = 'SELECT col1, col2 FROM tab WHERE col1 > 0'

#load weather with the SQL query
tabloaded = pd.read_sql(query, engine)
```

### 3\. Importing JSON Data and Working with APIs

```sql
import pandas as pd

#load a JSON file to a data frame
jsondata = pd.read_json('file.json')
```

we can specify the orientation of the JSON  
'split' : dict like {index -&gt; \[index\], columns -&gt; \[columns\], data -&gt; \[values\]}  
'records' : list like \[{column -&gt; value}, ... , {column -&gt; value}\]  
'index' : dict like {index -&gt; {column -&gt; value}}  
'columns' : dict like {column -&gt; {index -&gt; value}}  
'values' : just the values array

```sql
jsondata = pd.read_json('file.json', orient = 'split')
```

Import data from an API

```sql
#requests library used for any url
import requests

#example: using the Yelp API
api_url = "https://api.yelp.com/v3/businesses/search"

#set up parameter dictionary according to documentation
params = {"term": "bookstore", "location": "San Francisco"}

#set up header dictionary w/ API key according to documentation
headers = {"Authorization": "Bearer {}".format(api_key)}

#call the API
response = requests.get(api_url, params=params ,headers=headers)

#isolate the JSON data from the response object
data = response.json()

#load businesses data to a data frame
df = pd.DataFrame(data["businesses"])

#library for reading nested json
from pandas.io.json import json_normalize

#flatten data and load to data frame, with _ separators
df = json_normalize(data["businesses"], sep="_")

#flatten categories data, bring in business details
df = json_normalize(data["businesses"], sep="_", record_path="categories",
		     meta=["name", "alias", "rating", 
		               ["coordinates", "latitude"],["coordinates", "longitude"]],
                     meta_prefix="biz_")
```

### 4\. Combining multiple datasets

```sql
import pandas as pd

#append datasets method
appended = df1.append(df2)

#set ignore_index to True to renumber rows
appended = df1.append(df2, ignore_index = True)

#merge(): pandas function and dataframe method for merging datasets
#default is like sql inner join (only values in both dataframes) 
merged = df1.merge(df2, on = 'col1')
merged = df1.merge(df2, left_on = 'col1', right_on = 'col2')
```