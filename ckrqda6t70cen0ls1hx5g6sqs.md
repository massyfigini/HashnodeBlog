---
title: "Basic Python Pandas"
datePublished: Sat Mar 24 2018 14:13:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrqda6t70cen0ls1hx5g6sqs
slug: basic-python-pandas
tags: python, beginner, pandas

---

For dataframes

```python
import pandas as pd
```

| Date Format | Read | Save |
| --- | --- | --- |
| csv | pd.read\_csv() | df.to\_csv() |
| json | pd.read\_json() | df.to\_json() |
| excel | pd.read\_excel() | df.to\_excel() |
| hdf | pd.read\_hdf() | df.to\_hdf() |
| sql | pd.read\_sql() | df.to\_sql() |

```python
df = pd.read_csv('folder/file.csv')     #import csv
xlsx = ('folder/file.xlsx')
df = pd.read_excel(xlsx)     #import excel file

df.head()     #view first 5 lines
df.columns    #columns names
df.describe(include = "all")    #summary of each column (without include, will exclude NaN)
df.info

df['Col1']     #view column Col1 as a series
df[['Col1']]     #view column Col1 as a data frame (with header and row numbers)
y = df[['Col2','Col3','Col4']]     #create a new data frame with 3 columns of the original one
df['NewColumn'] = [1,0,...]       #add column "NewColumn"

df.loc['ID']      #select only the column with the label "ID" (loc function is "label-based")
df.loc[['ID']]      #same result, but the single bracket version gives a Pandas Series, the double bracket version gives a Pandas DataFrame.
df.loc[["IDrow","IDrow2"]]     #Pandas DataFrame with 2 rows. Ok only with the double brackets.
df.loc["IDrow", "ColumnName"]     #select only the element of the row  "IDrow" in the column "ColumnName"
df.loc["IDrow"]["ColumnName"]       #same result
df.loc["ColumnName"].loc["IDrow"]      #same result
df.loc[["IDrow","IDrow2"],["ColumnName", "ColumnName2"]] #Pandas DataFrame with 2 rows and 2 columns.
df.iloc[0, 2]     #access value in the first raw and third column
df.iloc[0:2, 0:3]     #first two raws and first three columns

avg_col1 = df['Col1'].astype('float').mean(axis=0) #average for Col1

df.dtypes    #data types for each column
df[['Col1']] = df[['Col1']].astype('float')    #convert Col1 to float

df.rename(columns={'Col1':'Col2'}, inplace=True)    #rename Col1 in Col2
df.replace("?", np.nan, inplace = True)    #replace "?" to NaN
df.dropna()    #delete record with one or more Nan
df.isnull()    #true if the cell is null

#create bins
bins = np.linspace(min(df['Col1']), max(df['Col1']), 4)    
#create three equal bins
group_names = ['Low', 'Medium', 'High']
df['Col1_bin'] = pd.cut(df['Col1'], bins, labels=group_names, include_lowest=True)
df['Col1_bin].value_counts()    #values for each bin

#dummy variables
dummy_1 = pd.get_dummies(df['Col1'])    #if Col1 is a dummy, split to two columns (0,1)
```