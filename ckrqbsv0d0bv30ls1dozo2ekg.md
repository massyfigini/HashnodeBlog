---
title: "Basic Exploratory Data Analysis with Python"
datePublished: Sun Oct 07 2018 10:51:29 GMT+0000 (Coordinated Universal Time)
cuid: ckrqbsv0d0bv30ls1dozo2ekg
slug: basic-exploratory-data-analysis-with-python
tags: python, pandas, numpy, data-visualization

---

### GOAL: PREDICT THE PRICE

```sql
import pandas as pd
import numpy as np

path='https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/automobileEDA.csv'
#(path='C:/Users/figinim/Documents/Studies/IBM/Resources/automobileEDA.csv')

df = pd.read_csv(path)
```

Print first rows and some info:

```sql
df.head()
```

```sql
! pip install seaborn
import matplotlib.pyplot as plt
import seaborn as sns
```

List the data types for each column:

```sql
print(df.dtypes)
```

Correlation between the variable

```sql
df.corr()
```

Correlation between three specific variable:

```sql
df[['bore','stroke' ,'compression-ratio','horsepower']].corr()
```

Scatterplot of "engine-size" and "price"1:

```sql
sns.regplot(x="engine-size", y="price", data=df)
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627642833820/M49-Ra5td.png align="left")

Correlation between the two:

```sql
df[["engine-size", "price"]].corr()
```

Scatterplot and correlation between highway-mpg and price

```sql
sns.regplot(x="highway-mpg", y="price", data=df)
df[['highway-mpg', 'price']].corr()
```

Boxplots for price for each body-style

```sql
sns.boxplot(x="body-style", y="price", data=df)
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627646826881/oibwY8WSX.png align="left")

Boxplots for price for each engine-location

```sql
sns.boxplot(x="engine-location", y="price", data=df)
```

Find out basic statistical analysis

```sql
df.describe()    #only numeric fields
df.describe(include=['object'])    #with object fields
```

Value counts of a field

```sql
df['drive-wheels'].value_counts()
df['drive-wheels'].value_counts().to_frame()    #in table
```

Make a cross table for engine-location count

```sql
# engine-location as variable
engine_loc_counts = df['engine-location'].value_counts().to_frame()
engine_loc_counts.rename(columns={'engine-location': 'value_counts'}, inplace=True)
engine_loc_counts.index.name = 'engine-location'
engine_loc_counts
```

### GROUPING

Different drive-wheels

```sql
df['drive-wheels'].unique()
```

Price for drive-wheels

```sql
df_group_one = df[['drive-wheels','price']].groupby(['drive-wheels'],as_index=False).mean()
df_group_one
```

Price for drive-wheels and body-style

```sql
df_gptest = df[['drive-wheels','body-style','price']]
grouped_test1 = df_gptest.groupby(['drive-wheels','body-style'],as_index=False).mean()
grouped_test1
```

Pivot for better reading

```sql
grouped_pivot = grouped_test1.pivot(index='drive-wheels',columns='body-style')
grouped_pivot
```

### HEATMAP

```sql
plt.pcolor(grouped_pivot, cmap='RdBu')    # use the grouped results
plt.colorbar()
plt.show()
```

Now with the correct axis values:

```sql
fig, ax = plt.subplots()
im = ax.pcolor(grouped_pivot, cmap='RdBu')
row_labels = grouped_pivot.columns.levels[1]     #label names
col_labels = grouped_pivot.index    #label names
#move ticks and labels to the center
ax.set_xticks(np.arange(grouped_pivot.shape[1]) + 0.5, minor=False)
ax.set_yticks(np.arange(grouped_pivot.shape[0]) + 0.5, minor=False)
ax.set_xticklabels(row_labels, minor=False)     #insert labels
ax.set_yticklabels(col_labels, minor=False)     #insert labels
plt.xticks(rotation=90)    #rotate label if too long
fig.colorbar(im)
plt.show()
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627647596815/oY3vobJFLm.png align="left")

### P-VALUE, PEARSON CORRELATION COEFFICIENT AND ANOVA

By convention, when the  
p-value is &lt; 0.001: we say there is strong evidence that the correlation is significant.  
p-value is &lt; 0.05: there is moderate evidence that the correlation is significant.  
p-value is &lt; 0.1: there is weak evidence that the correlation is significant.  
p-value is &gt; 0.1: there is no evidence that the correlation is significant.

```sql
from scipy import stats
```

Horsepower vs price

```sql
pearson_coef, p_value = stats.pearsonr(df['horsepower'], df['price'])
print("The Pearson Correlation Coefficient is", pearson_coef, " with a P-value of P = ", p_value)
```

The Pearson Correlation Coefficient is 0.8095745670036559 with a P-value of P = 6.369057428260101e-48  
Since the p-value is &lt; 0.001, the correlation between horsepower and price is statistically significant, and the linear relationship is quite strong (~0.809, close to 1)

\*\*ANOVA: Analysis of Variance \*\*  
The Analysis of Variance (ANOVA) is a statistical method used to test whether there are significant differences between the means of two or more groups. ANOVA returns two parameters:

* F-test score: ANOVA assumes the means of all groups are the same, calculates how much the actual means deviate from the assumption, and reports it as the F-test score. A larger score means there is a larger difference between the means.
    
* P-value: P-value tells how statistically significant is our calculated score value.  
    If our price variable is strongly correlated with the variable we are analyzing, expect ANOVA to return a sizeable F-test score and a small p-value.
    

Price for wheel-drive

```sql
grouped_test2=df_gptest[['drive-wheels', 'price']].groupby(['drive-wheels'])
grouped_test2.get_group('4wd')['price']
f_val, p_val = stats.f_oneway(grouped_test2.get_group('fwd')['price'], grouped_test2.get_group('rwd')['price'], grouped_test2.get_group('4wd')['price'])
print( "ANOVA results: F=", f_val, ", P =", p_val)
```

ANOVA results: F= 67.95406500780399 , P = 3.3945443577151245e-23  
This is a great result, with a large F test score showing a strong correlation and a P value of almost 0 implying almost certain statistical significance.