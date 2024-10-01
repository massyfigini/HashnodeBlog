---
title: "R: dplyr"
datePublished: Sat May 21 2016 15:26:45 GMT+0000 (Coordinated Universal Time)
cuid: ckrqi2diz0eip0ls18ge2c6h6
slug: r-dplyr
tags: r, data-structures

---

Package to interact easily with tables

```sql
install.packages('dplyr')
library(dplyr)
tab <- tbl_df(originaltable)
```

Five basic functions: select(), filter(), arrange(), mutate() e summarize()

### Select

To select columns

```sql
select(tab, col_1, col_5, col_4, col_20)

#from col_1 to col_5, and col_20:
select(tab, col_1:col_5, col_20)

#all except
select(tab, -col_5)
select(tab, -(col_1:col_5))
```

### Filter

To filter rows

```sql
filter(tab, col_1 == "value")

#comma equals to AND, | to OR:
filter(tab, col_1 == "value", col_5 < "3.0", col_3 != "y")
filter(tab, col_1 == "value" | col_5 < "3.0")

#exclude missing values
filter(tab, !is.na(col_1))
```

### Arrange

To order

```sql
arrange(tab, col_5, col_1)   #asc
arrange(tab, desc(col_5), col_1)   #desc and asc
```

### Mutate

New values based on current values

```sql
mutate(tab, col_21 = (col_5*col_20)+2)
```

### Summarize

To summarize the dataset

```sql
summarize(tab, avg_1 = mean(col_1))

# very useful with the group_by() function
summarize(group_by(tab, col_1), mean(col_2)) 
summarize(group_by(tab, col_1), mean(col_2), n(), n_distinct(col_2), max(col_2), min(col_2), sd(col_2))
```

We can concatenate the different function

```sql
arrange(filter(summarize(group_by(tabella, col_1), numero = n(), media = mean(col_2)), col_5 < "3.0"), desc(media))

#same result, different code
tabella %>% group_by(col_1) %>% summarize(numero = n(), media = mean(col_2)) %>% filter(col_5 < "3.0") %>% arrange(desc(media))
```

**Join**  
inner\_join(), left\_join() right\_join(), full\_join(), semi\_join(), anti\_join()

```sql
inner_join(tab1, tab2, by=c("FieldTab1"="FieldTab2"))
```

**Top\_n**  
first n values

```sql
#first 10 rows ordered by col
top_n(tab, 10,  col)
```